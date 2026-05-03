# Lyric Recognition and Overlay

Use this reference when the user asks to recognize lyrics from audio/video, create timed lyric captions, or embed lyrics into a Remotion music video.

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

3. Add the script below to the target Remotion project as `scripts/transcribe-lyrics.mjs`.
4. Update `audioPath` for the user's song file in `public/`.
5. Run `node scripts/transcribe-lyrics.mjs`.
6. Review `public/lyrics-transcript.txt` with the user before treating lyrics as confirmed.
7. Use `public/lyrics-captions.json` for timed lyric overlays in Remotion.

If network access fails while installing packages, cloning Whisper.cpp, or downloading a model, rerun the command with user approval for network access. If the first Whisper.cpp checkout is incomplete, remove the incomplete checkout before retrying.

## Transcription Script

```js
import { spawnSync } from "node:child_process";
import fs from "node:fs";
import path from "node:path";
import {
  downloadWhisperModel,
  installWhisperCpp,
  toCaptions,
  transcribe,
} from "@remotion/install-whisper-cpp";

const root = process.cwd();
const audioPath = path.join(root, "public", "frozen-behind-the-shine.mp3");
const wavPath = path.join(root, "public", "frozen-behind-the-shine-lyrics.wav");
const whisperPath = path.join(root, "whisper.cpp");
const outputPath = path.join(root, "public", "lyrics-captions.json");
const rawOutputPath = path.join(root, "public", "lyrics-whisper-raw.json");
const textOutputPath = path.join(root, "public", "lyrics-transcript.txt");

const whisperCppVersion = "1.5.5";
const model = process.env.WHISPER_MODEL ?? "small.en";

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

if (!fs.existsSync(audioPath)) {
  throw new Error(`Missing audio file: ${audioPath}`);
}

console.log("Preparing 16 kHz mono WAV for lyric recognition...");
run("ffmpeg", [
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
]);

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

console.log("Transcribing lyrics...");
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
const cleaned = captions
  .map((caption) => ({
    ...caption,
    text: caption.text.replace(/\s+/g, " "),
  }))
  .filter((caption) => caption.text.trim().length > 0)
  .filter((caption) => {
    const text = caption.text.trim().toLowerCase();
    return !["[music]", "(music)", "[applause]", "(applause)"].includes(text);
  });

fs.writeFileSync(rawOutputPath, JSON.stringify(whisperCppOutput, null, 2));
fs.writeFileSync(outputPath, JSON.stringify(cleaned, null, 2));
fs.writeFileSync(
  textOutputPath,
  cleaned.map((caption) => caption.text.trim()).join(" ").replace(/\s+/g, " "),
);

console.log(`Wrote ${cleaned.length} timed lyric captions to ${outputPath}`);
console.log(`Wrote plain transcript to ${textOutputPath}`);
```

## Remotion Integration Notes

- Treat `lyrics-transcript.txt` as a draft. Ask the user to confirm/correct it before deriving song meaning or final scenes.
- Treat `lyrics-captions.json` as timed caption data for lyric overlays.
- Use `$remotion-best-practices`, especially `rules/transcribe-captions.md`, `rules/display-captions.md`, `rules/text-animations.md`, `rules/audio.md`, and `rules/timing.md`.
- Animate lyrics with Remotion frame logic, not CSS animations or CSS transitions.
- Sync lyric opacity, scale, position, glow, and color accents to caption start/end times and major beat moments.
- Use the confirmed lyric meaning to decide which lines deserve hero typography, repeated motifs, or scene transitions.
