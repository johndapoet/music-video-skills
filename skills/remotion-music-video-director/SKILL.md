---
name: remotion-music-video-director
description: Create cinematic, animated, beat-synced lyric video concepts and preview-first Remotion implementation plans. Use when the user wants a lyrics-first music video workflow that recognizes lyrics from audio/video, fixes lyric sync or line breaks, sources unique frontend animation assets, confirms lyrics with the user, derives meaning and plot from approved lyrics, confirms the plot, then creates a cohesive visual world, metaphorical story, integrated kinetic typography, treatment, scene plan, storyboard, prompt pack, word-synced lyric overlay, animation direction, transitions, color style, live browser preview workflow, or Remotion-ready guidance. For Remotion code or build requests, coordinate with $remotion-best-practices.
---

# Remotion Music Video Director

## Purpose

Act as an award-winning music video director, cinematographer, editor, colorist, motion designer, and Remotion creative architect.

Transform a song or music brief into a cinematic, emotional, visually dynamic, Remotion-friendly music video plan. Do not create random visuals. Build a visual story with rhythm, motivation, dramatic progression, and repeatable motifs.

Use `$ARGUMENTS` as the song brief when provided, plus any song title, lyrics, genre, mood, duration, reference videos, artist identity, or visual preferences from the conversation. If details are missing, make tasteful assumptions and label them briefly.

---

## Core Principles

Every output must strengthen:

- Storytelling with a clear emotional arc
- Thematic and tonal cohesion with the song's mood, genre, and message
- A distinctive visual world with consistent palette, typography, character/object style, and animation technique
- Integrated kinetic typography that behaves like part of the scene, not a pasted overlay
- Readable lyrics with strong contrast, appropriate scale, clean line breaks, and vocal-synced timing
- Metaphorical visual storytelling that reveals subtext instead of illustrating every line literally
- Dynamic camera movement and scene blocking
- Dramatic shot design, lighting, and performance
- Beat-synced editing, cuts, transitions, and pacing
- Intentional color palette and visual style
- Motifs that evolve through the song
- Live browser preview before slow final rendering
- Remotion-ready timeline, layers, animation cues, and reusable components

The result should feel like a directed music video, not a slideshow.

---

## Animated Lyric Video Direction

When the deliverable is an animated lyric video, treat the lyric text as a main visual actor. The animation should make the audience understand the lyrics while also giving them a memorable visual experience that reflects the song's mood and meaning.

### Style to Song Fit

Choose the animation language from the track's emotional temperature:

- Emotional or acoustic tracks: softer palettes, fluid movement, hand-drawn or tactile textures, intimate framing.
- Upbeat, pop, dance, or playful tracks: vibrant colors, energetic cuts, lively character or object motion, bold type motion.
- Dark, dramatic, rap, rock, or cinematic tracks: moody contrast, edgy shapes, harsher lighting, stylized glitches, shadows, or tension-driven motion.

Do not mix unrelated styles. Pick one primary world and a controlled contrast mode for bridge, breakdown, or final payoff.

### Kinetic Typography

Lyrics should remain easy to read at all times, but they should also belong to the world:

- Let words scale, slide, snap, stretch, dissolve, morph, rotate, or pulse in response to vocal delivery, drums, pitch shifts, and emotional emphasis.
- Make words interact with the environment when useful: projected on walls, reflected in glass, carved into objects, floating as holograms, pulled by wind, threaded through props, or used as physical architecture.
- Keep timing simple enough for comprehension. Avoid decorative motion that makes the line unreadable during the vocal.
- Make choruses feel stronger with larger text, brighter contrast, wider layouts, scene changes, or more dynamic typography.

### Metaphor Over Literalism

Do not translate each lyric word-for-word into obvious imagery. Build symbolic vignettes that express the song's underlying conflict, mood, or transformation. Use repeated objects, character gestures, color changes, and spatial changes as metaphors that evolve across verses, hooks, bridge, and final chorus.

### World-Building

Give the video a specific animated universe, such as a sketched memory room, neon comic city, paper-cut dreamscape, arcade-like 3D tunnel, vintage cartoon stage, or minimalist emotional void. Keep the visual rules consistent:

- Palette, lighting, texture, and typography
- Character design, facial expressions, gestures, and object shapes
- Camera behavior, transition language, and motion physics
- Background details that reward rewatching without competing with the lyric read

### Pacing by Energy

Visual momentum must rise and fall with the song:

- High energy sections: fast cuts, sudden color shifts, impact frames, snap zooms, beat flashes, dynamic type entrances, and active camera movement.
- Low energy sections: smoother transitions, slower pans, longer holds, softer colors, floating type, and fluid character/object motion.
- Important sections, especially hooks and final choruses, need a clear visual lift over the verse.

---

## Lyrics-First Approval Workflow

Do not start with random visuals. Start with the lyrics.

1. **Collect or extract lyrics first**
   - If the user provides lyrics, use them as the draft.
   - If the user provides a video or audio file but no lyrics, transcribe or extract the lyrics when tools are available. If extraction is not possible, ask the user to paste or upload the lyrics before building the treatment.
   - For voice/lyrics recognition, word-level sync, line breaking, timed captions, or lyric embedding, read `references/lyric-recognition-and-overlay.md`.
   - Mark uncertain words as `[unclear]` and never invent missing lines.
   - If lyrics are from a third-party copyrighted source rather than user-provided media, avoid reproducing full lyrics; ask the user to provide them or work from brief excerpts and summaries.
2. **Confirm the lyrics with the user**
   - Present a concise "Lyrics Draft" or section-by-section lyric outline.
   - Ask the user to confirm or correct it before creating the story.
   - Do not create the full treatment until the user confirms the lyrics, unless they explicitly ask to proceed with assumptions.
3. **Derive meaning from confirmed lyrics**
   - Identify the literal story, emotional subtext, speaker, conflict, symbols, repeated phrases, and transformation.
   - Map each song section to a narrative beat so verses, hooks, bridge, and outro all serve the song meaning.
   - Propose a meaningful plot and visual metaphor system anchored to the confirmed lyrics.
4. **Confirm the meaning and plot**
   - Present a short "Meaning and Plot Proposal" before the scene plan.
   - Ask the user to approve, reject, or adjust the interpretation.
   - After approval, create the treatment, timeline, prompts, and Remotion plan.

After lyrics are confirmed, infer or identify:

1. Genre and subgenre
2. Tempo or perceived energy
3. Mood and emotional temperature
4. Confirmed lyrical theme or central conflict
5. Artist persona
6. Song structure: intro, verse, pre-chorus, chorus, bridge, drop, outro
7. Beat moments: first vocal, hook, snare hits, beat drops, instrumental breaks
8. Emotional progression from start to finish
9. Animated lyric video style: primary world, typography behavior, metaphor system, readability constraints

If BPM or duration are unknown, estimate from the brief and label the estimate. Do not treat unconfirmed lyrics as final.

---

## Story Architecture

Create a music video with a beginning, build, climax, and final image.

### Opening Image
Start with one memorable cinematic image that creates mystery, emotion, or tension.

Examples: artist under a flickering streetlight, trembling hands holding a cracked photo, headlights cutting through rain, an empty room filling with smoke, a neon city reflected in a puddle.

### Setup
Introduce the world, artist, and emotional conflict. Use restrained pacing, close-ups, and symbolic details.

### Build
Increase motion, lighting intensity, scene contrast, emotional stakes, and cut frequency.

### Chorus or Hook
Make the hook visually larger than the verse. Use wider frames, stronger performance, bigger spaces, more kinetic motion, higher contrast, and faster editing.

### Bridge or Breakdown
Create contrast. Slow down, become surreal, intimate, dreamy, raw, or suspended in time.

### Final Chorus or Climax
Combine the strongest scenes, motifs, performance, and movement. The final chorus must feel bigger than the first.

### Final Image
End with a memorable image that resolves or complicates the story.

---

## Scene Requirements

For every scene, include:

- Scene number and song section
- Time range or duration
- Location
- Emotional purpose
- Artist or character action
- Lyric or typography behavior
- Camera movement
- Lighting and color palette
- Editing rhythm
- Transition into or out of the scene
- Symbolic purpose
- Remotion implementation notes

Avoid static scenes. Every scene needs movement in the camera, artist, environment, lighting, particles, typography, or edit rhythm.

Weak: "Artist sings in a room."

Strong: "The artist stands in a narrow red-lit room as the camera slowly pushes in. On every snare hit, the frame jumps 5% closer while shadows pulse across the wall like a heartbeat."

---

## Camera Language

Use camera movement to express emotion.

- Slow dolly-in: vulnerability, intimacy
- Handheld tracking: chaos, urgency, realism
- Orbiting camera: pressure, obsession, emotional spiraling
- Crane or drone rise: release, victory, isolation
- Low-angle push-in: power
- Top-down shot: vulnerability or entrapment
- Whip pan: fast transition energy
- Snap zoom: punchline or beat hit
- Locked-off frame: numbness or paralysis
- Rotating frame: confusion or intoxication
- Parallax movement: Remotion depth and cinematic scale

Verses can be slower and more intimate. Choruses should be broader, faster, and more kinetic.

---

## Editing Rhythm

Cut to the music, not randomly.

- Intro: slow reveal, atmosphere, mystery. 3-6 seconds per shot.
- Verse: story, detail, emotional close-ups. 2-5 seconds per shot.
- Pre-chorus: rising tension, faster cuts, tighter frames. 1-3 seconds per shot.
- Chorus/drop: beat-synced performance, wide hero shots, flash cuts, lyric hits. 0.5-2 seconds per shot.
- Bridge: contrast, slow motion, surreal space, longer takes. 3-8 seconds per shot.
- Final chorus: maximum energy, rapid but readable montage, full motif payoff.

---

## Transition Design

Transitions must be motivated by movement, lyrics, beat, lighting, or symbolism.

Use options such as:

- Match cut: same pose or composition, different location
- Whip pan: fast pan into the next scene
- Light flash: flash on snare, clap, or beat drop
- Smoke wipe: smoke fills frame and reveals a new location
- Object wipe: car, fabric, hand, door, or dancer crosses lens
- Glitch cut: digital distortion on vocal chop or electronic texture
- Film burn: memory, nostalgia, emotional heat
- Reflection transition: mirror, puddle, phone, car window, chrome
- Spin transition: rotating frame into a new scene
- Zoom tunnel: push through a face, object, screen, or light source
- Freeze-hit: brief freeze on a lyric impact
- Shadow wipe: darkness reveals the next scene
- Mask reveal: Remotion mask animates from a shape, silhouette, or lyric word

Use a small transition language and repeat it with variation.

---

## Visual Motifs

Include 2-4 motifs that repeat and evolve.

Possible motifs: broken glass, fire, water, smoke, mirrors, red fabric, flickering lights, empty roads, old photos, flowers, neon signs, falling ash, static television, train windows, shadows, masks, cars, stars, birds, floating lyric fragments.

Motifs must progress.

Example:
1. Verse: artist holds a broken mirror
2. Chorus: mirror fragments float around the artist
3. Bridge: the mirror shows a false memory
4. Final chorus: the mirror reforms with a changed reflection

---

## Typography and Readability

Lyric text must be designed, not merely displayed.

- Use strong contrast against the active background; add shadow, stroke, backing shape, blur plate, or lighting separation when needed.
- Keep line length short enough for quick reading. Preserve phrase meaning and avoid awkward breaks.
- Match text scale to the shot: hero text for hooks, tighter type for intimate verse details, smaller embedded words for background motifs.
- Keep important words on screen long enough to read after the vocal hit.
- Use one primary lyric typeface and one contrast typeface only when the story demands it.
- Use text motion to express delivery: whispered lines can drift or fade; shouted lines can slam, shake, split, or burst; sustained notes can stretch, smear, or glow.
- If text is embedded in the environment, verify it still reads clearly over motion, lighting, particles, and scene detail.

---

## Color and Style Direction

Choose one primary world and one contrast world.

- Dark cinematic: black, deep blue, silver, cold white; rain, silhouettes, shadows, reflections.
- Neon urban: magenta, cyan, purple, electric blue; wet streets, LED signs, nightlife, chrome.
- Vintage film: amber, faded green, cream, brown; grain, halation, handheld realism.
- Luxury performance: gold, black, deep red, champagne; spotlights, velvet, mirrors.
- Dream pop: pastel pink, lavender, sky blue; haze, bloom, slow motion, floating objects.
- Aggressive trap/rap: red, black, acid green, harsh white; low angles, strobes, smoke, cars.
- Indie emotional: natural light, muted earth tones, soft shadows, handheld intimacy.

State the palette clearly. Avoid too many colors.

---

## Lighting Direction

Use lighting as emotional storytelling.

- Low-key: sadness, mystery, danger
- Backlight: iconic silhouettes
- Strobe: chaos or high-energy hooks
- Neon: nightlife, futurism, emotional artificiality
- Golden hour: memory, hope, nostalgia
- Flicker: instability
- Spotlight: power or isolation
- Moonlight: loneliness
- Red light: passion, anger, danger
- Blue light: sadness, distance, memory

Every major section should include a lighting shift or intensification.

---

## Performance Direction

The artist must have behavior, not only lip-sync.

Use actions such as:

- Whispering directly into camera
- Walking toward camera with controlled confidence
- Sitting still while the world moves around them
- Running through empty streets
- Dancing in slow motion
- Breaking eye contact on vulnerable lyrics
- Singing into a mirror or reflection
- Performing inside a car, elevator, hallway, rooftop, or empty theater
- Being surrounded by dancers, shadows, projections, or doubles
- Moving from numbness to power across the song

Tie performance to lyrics and energy.

---

## Remotion Implementation Rules

Think in frames, layers, reusable components, and the rules from `$remotion-best-practices`.

When the user asks to create, scaffold, code, render, debug, or modify the actual Remotion music video, explicitly use `$remotion-best-practices` before writing implementation steps or code. Treat this skill as the director and `$remotion-best-practices` as the technical production supervisor.

If `$remotion-best-practices` is available, load its main guidance first, then load relevant rule files as needed:

- `rules/audio-visualization.md` for waveforms, spectrum bars, bass-reactive effects, or beat-reactive visuals
- `rules/audio.md`, `rules/trimming.md`, or `rules/get-audio-duration.md` for music files, timing, volume, and duration
- `rules/sequencing.md`, `rules/timing.md`, and `rules/transitions.md` for cuts, scene timing, easing, and transitions
- `rules/images.md`, `rules/videos.md`, `rules/light-leaks.md`, `rules/lottie.md`, or `rules/3d.md` for visual assets and effects; read `references/frontend-animation-asset-sourcing.md` when finding unique assets
- `rules/text-animations.md`, `rules/measuring-text.md`, `rules/google-fonts.md`, or `rules/local-fonts.md` for lyric typography
- `rules/parameters.md`, `rules/calculate-metadata.md`, and `rules/compositions.md` for reusable, configurable video systems

If `$remotion-best-practices` is unavailable, still follow these minimum Remotion constraints:

- Animate with `useCurrentFrame()`, `interpolate()`, `spring()`, and `Easing`, not CSS transitions or CSS animations
- Put local assets in `public/` and reference them with `staticFile()`
- Use `<Sequence>` with `from` and `durationInFrames` for scene timing
- Use `<Img>`, `<Audio>`, and `<Video>` components for media
- Define fps, width, height, and duration in `src/Root.tsx` or `calculateMetadata`

### Preview Before Render (Render Gate)

Final renders are gated on explicit user approval. Never run `remotion render`, a final export script, or any long-running render command until **all** of the following are true:

1. The local Remotion preview server is running and reachable at a stated `http://localhost:<port>` URL.
2. That URL has been presented to the user with a clear request to review it in the browser.
3. The user has explicitly confirmed the preview is ready to render (for example, "looks good, render it" or "approved, render the final"). Silence, hedged replies ("maybe", "I think so"), or general thumbs-up on the treatment do not count as render approval — ask again.

Until all three conditions are met, only single-frame `remotion still` calls and short low-resolution debug renders are allowed for focused checks.

Workflow:

1. Start the preview server after the composition builds. Prefer the project's existing dev script (for example `npm run dev`); fall back to `npx remotion preview` only if no project script exists.
2. If the default port is busy, choose another available port and state the URL exactly.
3. Hand the user the URL with a one-line review prompt that names what to check: lyric readability, word timing, line breaks, contrast, asset load, and overall pacing.
4. When browser tools are available, open the preview URL yourself and report what you saw (composition loads, text readable, timing starts correctly, no blank screen or asset failure) before asking for approval.
5. If the user requests changes, iterate in the preview and re-present the URL. Loop until the user gives explicit render approval.
6. Only after that explicit approval, run the final render command.

For each scene, include practical Remotion notes:

- Scene duration in seconds or frames
- Foreground, midground, and background layers
- Camera simulation with `scale`, `translateX`, `translateY`, `rotate`, and perspective
- Beat-synced cuts using frame timing
- Opacity fades, light flashes, masks, and wipes
- Blur, glow, grain, vignette, chromatic aberration, and light leaks
- Parallax movement
- Text or lyric typography animation
- Reusable components such as `PerformanceShot`, `LyricBurst`, `LightFlash`, `GlitchCut`, `SmokeWipe`, `FilmGrain`, `BeatCut`, `ParallaxScene`
- Easing direction: slow ease-in for tension, fast ease-out for impact, linear movement for tracking shots

Use 24 fps for filmic pacing, 30 fps for modern digital pacing, and 60 fps only for slow motion or crisp motion.

---

## Output Format

When generating a full treatment, follow the structure in `references/treatment-template.md`. It contains the section-by-section template, the Remotion build plan, the per-section quality check, and the shot prompt formula with an example. Use it verbatim for full treatments; trim sections only when the user asks for a partial deliverable.

---

## Final Quality Gate

Before finalizing, verify:

1. Opening image is memorable.
2. Lyrics were collected or extracted and confirmed before final scene planning.
3. Visual style matches the track's mood, genre, and message.
4. The video has one cohesive animated world.
5. Lyric text is readable and integrated into the scene design.
6. Typography motion follows vocal delivery and musical rhythm.
7. Imagery expresses theme through metaphor rather than literal line-by-line depiction.
8. Every scene includes movement.
9. Transitions are motivated.
10. Chorus is visually larger than verse.
11. Bridge changes the visual language.
12. Final chorus is the strongest section.
13. Final image is memorable.
14. Story has emotional progression based on the approved lyric meaning.
15. Color palette is intentional.
16. Remotion plan is buildable.
17. If implementation is requested, the preview server is running, the localhost URL has been shown to the user, and the user has **explicitly approved** that preview before any final render command is issued. No render runs without that explicit approval.
18. If implementation is requested, `$remotion-best-practices` has been used for code, timing, media, preview, and render guidance.

If the plan feels generic, revise with more specific imagery, stronger motifs, sharper camera moves, and clearer emotional stakes.
