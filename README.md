# Motion library skill

An agent skill for Claude Code and OpenAI Codex. It teaches an agent to study why great motion design works, then invent original moves for a brief, instead of copying the references.

The skill itself is the `motion-library/` folder. That folder is a normal Agent Skill: a `SKILL.md` with `name` and `description`, plus `references/` and `scripts/`.

## Install

```bash
git clone https://github.com/ronkenx9/motion-library-skill.git
cd motion-library-skill
```

Then put the `motion-library` folder on the skill path your tool reads. A symlink is enough; both tools follow it. The commands below assume you are in the cloned repo.

### Claude Code

For every project on this machine:

```bash
mkdir -p ~/.claude/skills
ln -s "$PWD/motion-library" ~/.claude/skills/motion-library
```

Or copy the folder instead of linking it:

```bash
cp -R motion-library ~/.claude/skills/motion-library
```

For one repository only, use the same folder at `.claude/skills/motion-library`.

Start a new Claude Code session and try `/motion-library`.

### Codex

Current OpenAI docs load user skills from `~/.agents/skills`, and repository skills from `.agents/skills` (Codex walks from the working directory up to the repo root). Symlinks work.

```bash
mkdir -p ~/.agents/skills
ln -s "$PWD/motion-library" ~/.agents/skills/motion-library
```

For one repository only, use `.agents/skills/motion-library`.

Some older Codex builds still read `$CODEX_HOME/skills` or `~/.codex/skills`. If the skill does not show up under `/skills`, also link it there:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
ln -s "$PWD/motion-library" "${CODEX_HOME:-$HOME/.codex}/skills/motion-library"
```

Restart Codex if it does not pick the skill up. You can mention it with `$motion-library`.

If your agent does not load skills automatically, point it at `AGENTS.md` in this repo, or tell it to read `motion-library/SKILL.md` first.

## Requirements

Needed by the bundled scripts:

- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [ffmpeg](https://ffmpeg.org/) (including ffprobe)
- Python 3

Optional, for sound analysis (`scripts/analyze_sound.py`):

```bash
pip install -r motion-library/scripts/requirements.txt
```

That install is numpy and librosa. ffmpeg still does the loudness measurement in `extract.sh`.

## Usage

Once the skill is installed, ask things like:

- "Break down this video: https://..."
- "Storyboard a 30s product film from this brief."
- "Grow my motion library from these official brand links."

The agent should fetch and measure the video, write a breakdown, pick technique families and remix them into something original, then run the pre-flight checklist before any storyboard or render.

Your working library (downloads, frames, breakdowns, lessons) is a separate folder from this skill. Pass it with `--library` or `MOTION_LIBRARY`. The default is `./motion-library`.

```bash
./motion-library/scripts/fetch.sh --library ./my-library my-slug 'https://www.youtube.com/watch?v=...'
./motion-library/scripts/extract.sh --library ./my-library my-slug
python3 ./motion-library/scripts/analyze_sound.py --library ./my-library my-slug
```

After frames and the contact sheet exist, delete the raw video and keep the source URL in the breakdown so it can be re-fetched.

## What is in the skill

- `motion-library/SKILL.md` — the workflow
- `motion-library/references/principles.md` — ten principles and remix operators
- `motion-library/references/technique-families.md` — 45 families grouped by use, with recipes and public reference links
- `motion-library/references/premium-sound.md` — how brand mixes measure, and how to build sound that holds up
- `motion-library/references/preflight-checklist.md` — rules, a checklist, and a lessons-file template
- `motion-library/references/breakdown-template.md` — the breakdown format
- `motion-library/scripts/` — fetch, extract, and optional sound analysis

## Companion skill: launch-storyboard

`launch-storyboard/` is a second skill in this repo. It storyboards a product launch film the way a web designer builds a site:
- the brief and target customer;
- a hook (provocation, bold promise or story) and a retention arc;
- Map, Cut, Colorize: the agent finds and downloads the product's real UI library online (brand and press kits, design-system or Storybook sites, GitHub, npm, official Figma Community files, or a live capture), logs every source and licence in `ui/UI-SOURCES.md`, and cuts it into bite-sized UI pieces;
- 4–6 poster-grade 1920×1080 frames with a creative direction for pacing and transitions.

Use it before `motion-library` section C on any product launch. Install it the same way as `motion-library`, linking `launch-storyboard` next to it:

```bash
ln -s "$PWD/launch-storyboard" ~/.claude/skills/launch-storyboard
ln -s "$PWD/launch-storyboard" ~/.agents/skills/launch-storyboard
```

## Licence

MIT
