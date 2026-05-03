# Remotion Music Video Director Skill

Publish-ready Agent Skill for creating cinematic, beat-synced Remotion music video treatments, storyboards, prompt packs, and implementation plans.

For code or build requests, the skill instructs agents to coordinate with `$remotion-best-practices` so the creative plan turns into valid Remotion React code, timing, media handling, and render guidance.

## Install From GitHub

Replace `OWNER/REPO` with your GitHub repo:

```bash
npx skills add OWNER/REPO --skill remotion-music-video-director -a codex -g -y
```

Full URL form:

```bash
npx skills add https://github.com/OWNER/REPO --skill remotion-music-video-director -a codex -g -y
```

Restart Codex after installing so it can pick up the skill.

## Test Locally

From this repo root:

```bash
npx skills add . --skill remotion-music-video-director -a codex -g -y
```

## Skill Layout

```text
skills/remotion-music-video-director/
├── SKILL.md
└── agents/
    └── openai.yaml
```
