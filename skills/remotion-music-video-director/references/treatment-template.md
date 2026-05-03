# Music Video Treatment Template

Use this structure when generating a full treatment. Trim sections only when the user explicitly asks for a partial deliverable (for example, "just the scene plan" or "only the prompt pack").

## Contents

- Treatment template
- Shot prompt formula

## Treatment Template

```markdown
# Music Video Treatment: [Title]

## Assumptions
- Song/artist:
- Genre:
- Mood:
- Estimated energy or BPM:
- Video length:
- Lyrics source:
- Lyrics confirmation status:

## Confirmed Lyric Meaning
- Literal story:
- Emotional subtext:
- Central conflict:
- Key symbols or repeated phrases:
- Section-by-section lyric beats:

## Approved Plot Direction
- User-approved interpretation:
- Visual metaphor:
- Character or artist arc:
- Meaning-to-scene strategy:

## Director's Concept
[One strong paragraph describing the emotional story and visual world.]

## Story Arc
- Opening image:
- Emotional conflict:
- Build:
- Hook/chorus release:
- Bridge contrast:
- Final payoff:
- Final image:

## Visual Style
- Primary style:
- Contrast world:
- Color palette:
- Lighting language:
- Texture:
- Visual references:

## Motifs
1. [Motif] — how it evolves
2. [Motif] — how it evolves
3. [Motif] — how it evolves

## Camera Language
[Movement vocabulary by song section.]

## Editing and Transition Language
[Cut speed, beat-sync logic, and repeated transition types.]

## Timeline and Scene Plan

### 00:00-00:10 — Intro / Opening Image
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 00:10-00:35 — Verse 1
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 00:35-00:50 — Pre-Chorus / Build
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 00:50-01:20 — Chorus / Hook
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 01:20-01:50 — Verse 2
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 01:50-02:10 — Bridge / Breakdown
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 02:10-02:50 — Final Chorus / Climax
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

### 02:50-End — Outro / Final Image
- Location:
- Action:
- Camera:
- Lighting/color:
- Edit rhythm:
- Transition:
- Story purpose:
- Remotion notes:

## Remotion Build Plan
- FPS:
- Composition duration:
- Live preview command:
- Preview URL:
- Preview approval needed before final render:
- Main components:
- Effects layers:
- Beat-sync strategy:
- Typography strategy:
- Readability and contrast strategy:
- Animated world rules:
- Metaphor and motif payoff:
- Transition components:
- `$remotion-best-practices` rules to load:
- Remotion guardrails:
- Color grading layer:
- Render/export notes after preview approval:

## Prompt Pack for AI Video or Image Generation
Create 8-15 shot prompts. Each prompt should include subject, setting, action, camera movement, lighting, color, lens feel, mood, and negative constraints if useful.

## Quality Check
- The video has a clear story, not random visuals.
- The visual style matches the song's mood, genre, and message.
- The animated world is cohesive and specific.
- Lyric typography is integrated into the visual art, not just overlaid.
- Lyrics stay readable through motion, contrast, line length, and timing.
- The imagery uses metaphor and symbolic vignettes instead of literal one-to-one lyric translation.
- The chorus is visually bigger than the verse.
- The bridge creates contrast.
- Camera movement matches emotion.
- Cuts and transitions are motivated by music.
- Scenes are grounded in confirmed lyrics and approved meaning.
- Motifs evolve across the video.
- Color and lighting are intentional.
- Remotion implementation is specific enough to build.
- Remotion code guidance explicitly uses `$remotion-best-practices` when implementation is requested.
- Implementation work serves a live browser preview before attempting a slow final render.
```

## Shot Prompt Formula

Use this formula for AI video or image prompts:

```text
[Subject/artist] in [specific location], [clear action], [emotion], [camera movement], [shot size/lens feel], [lighting], [color palette], [texture/style], [music-video energy], [transition cue if relevant].
```

Example:

```text
A lone artist walking through a rain-soaked neon alley at midnight, singing directly into camera with controlled anger, slow handheld tracking shot, 35mm cinematic lens feel, cyan and magenta neon reflections, heavy mist, wet asphalt glow, dramatic high-contrast music video style, ending with a whip pan into darkness.
```
