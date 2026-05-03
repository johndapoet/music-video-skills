# Lyric Recognition and Overlay

Use this reference when the user asks to recognize lyrics from audio/video, create timed lyric captions, fix lyric sync, improve lyric line breaks, highlight words, or embed lyrics into a Remotion music video.

## Contents

- Workflow
- Word-level transcription script
- Output files
- Shared Whisper cache
- Line-breaking rules
- Lyric typography design
- Remotion overlay pattern
- Sync calibration

## Workflow

1. Check for an existing recognizer before installing anything:

```bash
which whisper
which mlx_whisper
which whisper-cli
python3 -m pip show openai-whisper faster-whisper whisperx vosk pocketsphinx SpeechRecognition stable-ts
```

2. If no recognizer is available, prefer the Remotion captioning path:

```bash
npm install @remotion/install-whisper-cpp @remotion/captions
```

3. Use one shared Whisper.cpp/model cache for all projects. The script defaults to `~/.cache/remotion-music-video-director/whisper.cpp`, so each project does not download another large Whisper.cpp checkout.
4. Add the script below to the target Remotion project as `scripts/transcribe-lyrics.mjs`.
5. Put the song wherever the project keeps its other media assets (typically `public/` or a subfolder like `public/songs/`), then run the script with the audio path:

```bash
node scripts/transcribe-lyrics.mjs public/song.mp3
```

The script writes every lyrics output **into the same folder as the audio file**, using the audio basename as the prefix so multiple songs in one folder do not collide. For `public/song.mp3` you get `public/song-words.json`, `public/song-lines.json`, etc. For `public/songs/track-1.mp3` you get `public/songs/track-1-words.json`, and so on. Override with `LYRICS_OUTPUT_DIR` or `LYRICS_OUTPUT_PREFIX` if a project needs a different convention.

6. Review the `*-transcript.txt` file next to the audio with the user before treating lyrics as confirmed.
7. Use `*-lines.json` for readable lyric lines.
8. Use `*-words.json` for word-level highlighting and exact sync.
9. Keep `*-captions.json` only as compatibility data for `@remotion/captions`.

If network access fails while installing packages, cloning Whisper.cpp, or downloading a model, rerun the command with user approval for network access. If the first Whisper.cpp checkout is incomplete, remove the incomplete checkout before retrying.

## Word-Level Transcription Script

This script requests token-level Whisper timestamps, converts caption chunks into word tokens, groups words into readable lyric lines, and writes separate word and line JSON files. This avoids two common failures: captions drifting away from the music and lyric lines breaking in disconnected phrases.

```js
import { spawnSync } from "node:child_process";
import fs from "node:fs";
import os from "node:os";
import path from "node:path";
import {
  downloadWhisperModel,
  installWhisperCpp,
  toCaptions,
  transcribe,
} from "@remotion/install-whisper-cpp";

const root = process.cwd();
const inputArg = process.argv[2] ?? process.env.LYRICS_AUDIO ?? "public/song.mp3";
const audioPath = path.isAbsolute(inputArg) ? inputArg : path.join(root, inputArg);
const audioDir = path.dirname(audioPath);
const audioBaseName = path.basename(audioPath, path.extname(audioPath));
const outputDir = process.env.LYRICS_OUTPUT_DIR
  ? path.resolve(process.env.LYRICS_OUTPUT_DIR)
  : audioDir;
const outputPrefix = process.env.LYRICS_OUTPUT_PREFIX ?? audioBaseName;
const wavPath = path.join(outputDir, `${outputPrefix}-recognition.wav`);

const sharedWhisperCache = process.env.WHISPER_CACHE_DIR
  ? path.resolve(process.env.WHISPER_CACHE_DIR)
  : path.join(os.homedir(), ".cache", "remotion-music-video-director");
const whisperPath = process.env.WHISPER_CPP_DIR
  ? path.resolve(process.env.WHISPER_CPP_DIR)
  : path.join(sharedWhisperCache, "whisper.cpp");

const wordsOutputPath = path.join(outputDir, `${outputPrefix}-words.json`);
const linesOutputPath = path.join(outputDir, `${outputPrefix}-lines.json`);
const captionsOutputPath = path.join(outputDir, `${outputPrefix}-captions.json`);
const rawOutputPath = path.join(outputDir, `${outputPrefix}-whisper-raw.json`);
const textOutputPath = path.join(outputDir, `${outputPrefix}-transcript.txt`);

const whisperCppVersion = process.env.WHISPER_CPP_VERSION ?? "1.5.5";
const model = process.env.WHISPER_MODEL ?? "small.en";
const lyricOffsetMs = Number(process.env.LYRIC_OFFSET_MS ?? 0);

const lineRules = {
  maxWords: Number(process.env.LYRIC_MAX_WORDS ?? 7),
  maxChars: Number(process.env.LYRIC_MAX_CHARS ?? 42),
  minLineMs: Number(process.env.LYRIC_MIN_LINE_MS ?? 650),
  maxLineMs: Number(process.env.LYRIC_MAX_LINE_MS ?? 3200),
  softGapMs: Number(process.env.LYRIC_SOFT_GAP_MS ?? 260),
  hardGapMs: Number(process.env.LYRIC_HARD_GAP_MS ?? 650),
};

const nonLyricTokens = new Set([
  "[music]",
  "(music)",
  "[applause]",
  "(applause)",
  "[instrumental]",
  "(instrumental)",
]);

const weakStarts = new Set([
  "and",
  "but",
  "or",
  "so",
  "cause",
  "cuz",
  "because",
  "of",
  "to",
  "in",
  "on",
  "at",
  "the",
  "a",
  "an",
  "my",
  "your",
]);

const weakEnds = new Set([
  "and",
  "but",
  "or",
  "so",
  "of",
  "to",
  "in",
  "on",
  "at",
  "the",
  "a",
  "an",
  "my",
  "your",
]);

const run = (command, args) => {
  const result = spawnSync(command, args, {
    cwd: root,
    encoding: "utf8",
    stdio: "inherit",
  });

  if (result.status !== 0) {
    throw new Error(`${command} ${args.join(" ")} failed`);
  }
};

const commandExists = (command) =>
  spawnSync(command, ["-version"], { stdio: "ignore" }).status === 0;

const stripWord = (text) =>
  text.toLowerCase().replace(/^[^a-z0-9']+|[^a-z0-9']+$/g, "");

const isNonLyric = (text) => nonLyricTokens.has(text.trim().toLowerCase());

const numberOrNull = (value) => {
  const number = Number(value);
  return Number.isFinite(number) ? number : null;
};

const normalizeCaptionTiming = (caption) => {
  const startMs =
    numberOrNull(caption.startMs) ??
    numberOrNull(caption.fromMs) ??
    numberOrNull(caption.timestampMs) ??
    0;
  const endMs =
    numberOrNull(caption.endMs) ??
    numberOrNull(caption.toMs) ??
    Math.max(startMs + 250, startMs);

  return {
    startMs: Math.max(0, startMs + lyricOffsetMs),
    endMs: Math.max(0, endMs + lyricOffsetMs),
  };
};

const splitCaptionIntoWords = (caption, captionIndex) => {
  const text = String(caption.text ?? "").replace(/\s+/g, " ").trim();
  if (!text || isNonLyric(text)) {
    return [];
  }

  const parts = text.split(" ").filter(Boolean);
  const { startMs, endMs } = normalizeCaptionTiming(caption);
  const duration = Math.max(80, endMs - startMs);

  return parts
    .map((part, partIndex) => {
      const wordStart = startMs + (duration * partIndex) / parts.length;
      const wordEnd = startMs + (duration * (partIndex + 1)) / parts.length;

      return {
        id: `${captionIndex}-${partIndex}`,
        text: part,
        startMs: Math.round(wordStart),
        endMs: Math.round(Math.max(wordStart + 80, wordEnd)),
      };
    })
    .filter((word) => !isNonLyric(word.text));
};

const captionFromWord = (word, index) => ({
  text: `${index === 0 ? "" : " "}${word.text}`,
  startMs: word.startMs,
  endMs: word.endMs,
  timestampMs: word.startMs,
});

const makeLine = (lineWords, id) => {
  const first = lineWords[0];
  const last = lineWords[lineWords.length - 1];

  return {
    id,
    text: lineWords.map((word) => word.text).join(" "),
    startMs: first.startMs,
    endMs: last.endMs,
    words: lineWords.map((word, index) => ({
      ...word,
      textWithSpace: `${index === 0 ? "" : " "}${word.text}`,
    })),
  };
};

const shouldBreakBefore = (lineWords, nextWord, rules) => {
  if (lineWords.length === 0) {
    return false;
  }

  const first = lineWords[0];
  const previous = lineWords[lineWords.length - 1];
  const gapMs = nextWord.startMs - previous.endMs;
  const candidateText = [...lineWords, nextWord]
    .map((word) => word.text)
    .join(" ");
  const candidateDuration = nextWord.endMs - first.startMs;
  const previousEndsPhrase = /[.!?;:]$/.test(previous.text);
  const nextStartsWeak = weakStarts.has(stripWord(nextWord.text));

  if (gapMs >= rules.hardGapMs) {
    return true;
  }

  if (nextStartsWeak && gapMs < rules.hardGapMs) {
    return false;
  }

  if (previousEndsPhrase && candidateDuration >= rules.minLineMs) {
    return true;
  }

  if (gapMs >= rules.softGapMs && lineWords.length >= 3) {
    return true;
  }

  if (lineWords.length >= rules.maxWords) {
    return true;
  }

  if (candidateText.length > rules.maxChars && lineWords.length >= 3) {
    return true;
  }

  if (candidateDuration > rules.maxLineMs && lineWords.length >= 3) {
    return true;
  }

  return false;
};

const polishLines = (lines, rules) => {
  const polished = [];

  for (const line of lines) {
    const previous = polished[polished.length - 1];

    if (!previous) {
      polished.push(line);
      continue;
    }

    const gapMs = line.startMs - previous.endMs;
    const previousLast = stripWord(previous.words[previous.words.length - 1].text);
    const currentFirst = stripWord(line.words[0].text);
    const combinedText = `${previous.text} ${line.text}`;
    const shouldMerge =
      gapMs < rules.hardGapMs &&
      combinedText.length <= rules.maxChars + 14 &&
      (previous.words.length <= 2 ||
        weakEnds.has(previousLast) ||
        weakStarts.has(currentFirst));

    if (shouldMerge) {
      const mergedWords = [...previous.words, ...line.words].map(
        ({ textWithSpace, ...word }) => word,
      );
      polished[polished.length - 1] = makeLine(
        mergedWords,
        previous.id,
      );
    } else {
      polished.push(line);
    }
  }

  return polished.map((line, index) =>
    makeLine(
      line.words.map(({ textWithSpace, ...word }) => word),
      index,
    ),
  );
};

const groupWordsIntoLines = (words, rules) => {
  const lines = [];
  let lineWords = [];

  for (const word of words) {
    if (shouldBreakBefore(lineWords, word, rules)) {
      lines.push(makeLine(lineWords, lines.length));
      lineWords = [];
    }

    lineWords.push(word);
  }

  if (lineWords.length > 0) {
    lines.push(makeLine(lineWords, lines.length));
  }

  return polishLines(lines, rules);
};

if (!fs.existsSync(audioPath)) {
  throw new Error(`Missing audio file: ${audioPath}`);
}

fs.mkdirSync(outputDir, { recursive: true });
fs.mkdirSync(path.dirname(whisperPath), { recursive: true });

console.log("Preparing 16 kHz mono WAV for word-level lyric recognition...");
const ffmpegArgs = [
  "-y",
  "-i",
  audioPath,
  "-af",
  "pan=mono|c0=0.5*c0+0.5*c1,highpass=f=120,lowpass=f=4200,loudnorm=I=-16:TP=-1.5:LRA=11",
  "-ar",
  "16000",
  "-ac",
  "1",
  wavPath,
];

if (commandExists("ffmpeg")) {
  run("ffmpeg", ffmpegArgs);
} else {
  run("npx", ["remotion", "ffmpeg", ...ffmpegArgs]);
}

console.log(`Using shared Whisper.cpp directory: ${whisperPath}`);
console.log(`Installing Whisper.cpp ${whisperCppVersion} if needed...`);
await installWhisperCpp({
  to: whisperPath,
  version: whisperCppVersion,
  printOutput: true,
});

console.log(`Downloading Whisper model ${model} if needed...`);
await downloadWhisperModel({
  model,
  folder: whisperPath,
  printOutput: true,
});

console.log("Transcribing lyrics with token-level timestamps...");
const whisperCppOutput = await transcribe({
  inputPath: wavPath,
  whisperPath,
  whisperCppVersion,
  model,
  tokenLevelTimestamps: true,
  language: "en",
  splitOnWord: true,
  printOutput: true,
  onProgress: (progress) => {
    const percent = Math.round(progress * 100);
    process.stdout.write(`\rWhisper progress: ${percent}%`);
  },
});

process.stdout.write("\n");

const { captions } = toCaptions({ whisperCppOutput });
const words = captions.flatMap(splitCaptionIntoWords).sort((a, b) => {
  return a.startMs - b.startMs || a.endMs - b.endMs;
});

const wordCaptions = words.map(captionFromWord);
const lines = groupWordsIntoLines(words, lineRules);
const transcript = words.map((word) => word.text).join(" ").replace(/\s+/g, " ");

fs.writeFileSync(rawOutputPath, JSON.stringify(whisperCppOutput, null, 2));
fs.writeFileSync(wordsOutputPath, JSON.stringify(words, null, 2));
fs.writeFileSync(linesOutputPath, JSON.stringify(lines, null, 2));
fs.writeFileSync(captionsOutputPath, JSON.stringify(wordCaptions, null, 2));
fs.writeFileSync(textOutputPath, transcript);

console.log(`Wrote ${words.length} word tokens to ${wordsOutputPath}`);
console.log(`Wrote ${lines.length} lyric lines to ${linesOutputPath}`);
console.log(`Wrote Remotion caption tokens to ${captionsOutputPath}`);
console.log(`Wrote plain transcript to ${textOutputPath}`);
```

## Output Files

All outputs are written next to the source audio file and share the audio's basename as their prefix, so lyrics, music, and other assets stay co-located. For `public/song.mp3`:

- `public/song-words.json`: word-level tokens with `text`, `startMs`, and `endMs`. Use this for precise highlighting.
- `public/song-lines.json`: readable lyric lines with nested word tokens. Use this for the primary lyric overlay.
- `public/song-captions.json`: word-level `Caption[]` data for `@remotion/captions` compatibility.
- `public/song-transcript.txt`: plain draft transcript to confirm with the user.
- `public/song-whisper-raw.json`: raw recognizer output for debugging.
- `public/song-recognition.wav`: 16 kHz mono WAV used during recognition; safe to delete after sync is verified.

Use `LYRICS_OUTPUT_PREFIX` to change the basename (for example, when you want `lyrics-` instead of the song name) and `LYRICS_OUTPUT_DIR` to write outputs to a different folder.

## Shared Whisper Cache

Do not put Whisper.cpp inside each Remotion project unless the user explicitly needs a project-local checkout. It is large and will be duplicated across projects.

Default shared location:

```text
~/.cache/remotion-music-video-director/whisper.cpp
```

Override locations when needed:

```bash
WHISPER_CACHE_DIR="$HOME/.cache/remotion-shared-whisper" \
node scripts/transcribe-lyrics.mjs public/song.mp3

WHISPER_CPP_DIR="/Volumes/FastDisk/whisper.cpp" \
node scripts/transcribe-lyrics.mjs public/song.mp3
```

Use `WHISPER_CACHE_DIR` to change the parent cache folder. Use `WHISPER_CPP_DIR` to point at a specific existing Whisper.cpp checkout. Keep the model consistent across projects when comparing sync quality; changing `WHISPER_MODEL` may download an additional model into the shared checkout.

## Line-Breaking Rules

Prefer the generated `*-lines.json` file (next to the song) over automatic TikTok-style grouping when the lyric phrasing matters.

Good lyric lines:

- Keep short phrases together unless there is a clear vocal pause.
- Avoid starting a new line with weak connector words like "and", "but", "of", "to", "the", or "my" unless there is a long silence.
- Avoid ending a line with weak connector words.
- Use word count, character count, punctuation, and vocal gaps together.
- Keep repeated hooks visually consistent across choruses when the lyrics repeat.

Tune grouping with environment variables:

```bash
LYRIC_MAX_WORDS=6 \
LYRIC_MAX_CHARS=36 \
LYRIC_SOFT_GAP_MS=240 \
LYRIC_HARD_GAP_MS=620 \
node scripts/transcribe-lyrics.mjs public/song.mp3
```

## Lyric Typography Design

Use lyric overlays as part of the animated world, not as captions pasted over finished footage.

- Preserve readability first: high contrast, stable phrase grouping, enough dwell time, and no motion that obscures the lyric during the vocal.
- Match typography to tone: soft drifting type for vulnerable lines, bold snap or scale hits for energetic hooks, distorted or fractured type for darker dramatic moments.
- Let important words react to delivery with frame-driven scale, opacity, blur, glow, rotation, or position changes.
- Embed text into the scene when it helps the concept: projected on walls, reflected in glass, attached to objects, floating as holograms, or moving through foreground and background layers.
- Keep choruses visually stronger than verses through larger type, brighter contrast, wider layouts, faster entrances, or stronger environmental interaction.
- Use one primary lyric type system across the video. Add contrast only for a purposeful bridge, breakdown, or narrative shift.

## Remotion Overlay Pattern

Do not rely on line-level captions for timing. Render each line as a page, then highlight each word from its own `startMs` and `endMs`.

Because lyrics files live next to the audio (for example `public/song.mp3` → `public/song-lines.json`), pass the lines path to the component as a prop so the same component works for any song. The path is relative to `public/`, exactly like the matching `staticFile()` lookup.

```tsx
import { useEffect, useState } from "react";
import {
  AbsoluteFill,
  interpolate,
  staticFile,
  useCurrentFrame,
  useVideoConfig,
} from "remotion";

type LyricWord = {
  text: string;
  textWithSpace: string;
  startMs: number;
  endMs: number;
};

type LyricLine = {
  id: number;
  text: string;
  startMs: number;
  endMs: number;
  words: LyricWord[];
};

type LyricOverlayProps = {
  // Path to the lines JSON inside `public/`, e.g. "song-lines.json" or "songs/track-1-lines.json".
  linesFile: string;
};

export const LyricOverlay = ({ linesFile }: LyricOverlayProps) => {
  const [lines, setLines] = useState<LyricLine[]>([]);
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();
  const timeMs = (frame / fps) * 1000;

  useEffect(() => {
    fetch(staticFile(linesFile))
      .then((response) => response.json())
      .then(setLines);
  }, [linesFile]);

  const activeLine = lines.find((line) => {
    return timeMs >= line.startMs - 140 && timeMs <= line.endMs + 260;
  });

  if (!activeLine) {
    return null;
  }

  const lineOpacity = interpolate(
    timeMs,
    [
      activeLine.startMs - 140,
      activeLine.startMs + 80,
      activeLine.endMs + 80,
      activeLine.endMs + 260,
    ],
    [0, 1, 1, 0],
    { extrapolateLeft: "clamp", extrapolateRight: "clamp" },
  );

  return (
    <AbsoluteFill style={{ justifyContent: "flex-end", alignItems: "center" }}>
      <div
        style={{
          marginBottom: 110,
          maxWidth: 980,
          textAlign: "center",
          fontSize: 64,
          lineHeight: 1.06,
          fontWeight: 900,
          whiteSpace: "pre-wrap",
          opacity: lineOpacity,
          color: "white",
          textShadow: "0 4px 28px rgba(0,0,0,0.65)",
        }}
      >
        {activeLine.words.map((word) => {
          const active = timeMs >= word.startMs && timeMs <= word.endMs;
          const wordLift = interpolate(
            timeMs,
            [word.startMs - 80, word.startMs, word.endMs],
            [0, 1, 0],
            { extrapolateLeft: "clamp", extrapolateRight: "clamp" },
          );

          return (
            <span
              key={`${word.startMs}-${word.text}`}
              style={{
                color: active ? "#ffffff" : "rgba(255,255,255,0.58)",
                transform: `translateY(${-8 * wordLift}px) scale(${
                  active ? 1.06 : 1
                })`,
                display: "inline-block",
                textShadow: active
                  ? "0 0 18px rgba(255,255,255,0.85)"
                  : "0 4px 28px rgba(0,0,0,0.65)",
              }}
            >
              {word.textWithSpace}
            </span>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

Use frame-driven `interpolate()` calculations only. Do not use CSS transitions or CSS animations for lyric motion because they will not render correctly in Remotion.

Final renders are gated on explicit user approval (see SKILL.md "Preview Before Render"). Serve the composition in the live Remotion/browser preview, check at least one verse and one chorus for lyric readability, word timing, line breaks, contrast, and asset loading, hand the user the preview URL, and wait for an explicit "render it" before starting the slow final render.

## Sync Calibration

If every word appears early or late by the same amount, rerun the script with `LYRIC_OFFSET_MS`.

Examples:

```bash
LYRIC_OFFSET_MS=-120 node scripts/transcribe-lyrics.mjs public/song.mp3
LYRIC_OFFSET_MS=180 node scripts/transcribe-lyrics.mjs public/song.mp3
```

Use a negative value when lyrics appear late and need to start earlier. Use a positive value when lyrics appear too early and need to start later.

If only some sections drift, split the song into sections or manually adjust the affected word ranges in the `*-words.json` file next to the song and regenerate lines from the corrected words.
