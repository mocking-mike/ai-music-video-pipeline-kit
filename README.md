# AI Music Video Pipeline Kit

A self-contained starter kit for building an AI video project from scratch with
Codex, Claude, or another coding agent. The primary example is a music video
generated and edited tightly to a specific MP3 file.

This folder does not assume you already have a project. Copy it into an empty
directory, add an MP3, and start from zero.

## What's Included

```text
ai-music-video-pipeline-kit/
  README.md
  PIPELINE.md
  AGENT_START_PROMPT.md
  templates/
    asset_bible.json
    project_manifest.json
    prompts.json
    storyboard.json
    preview.html
  scripts/
    README.md
  examples/
    first_user_message.md
    media/
      dopamina-z-linkedina.mp4
```

## Example Output

The repo includes a finished example music video:

[Dopamina z LinkedIna](examples/media/dopamina-z-linkedina.mp4)

## How To Use It

1. Copy the entire `ai-music-video-pipeline-kit` folder into a new empty project directory.
2. Rename the project folder, for example `my-music-video`.
3. Add your MP3 as `audio/source.mp3`, or ask the agent to create the project structure first.
4. Open the project in Codex, Claude Code, or a similar coding-agent environment.
5. Paste the contents of `AGENT_START_PROMPT.md` into the agent.
6. The agent should begin with audio analysis before creating the storyboard,
   asset bible, shot list, preview, or video renders.

## Core Rules

- Video generation runs through fal.ai.
- Storyboard frames are only for story planning and `preview.html` placeholders.
- Storyboard frames must never be used as `reference_images` for final renders.
- Final renders may use only the asset bible, approved footage, and approved
  continuity frames.
- Every render must save the full `*_source_full.mp4` plus the active edited `*.mp4`.
- After the MP3 is added, the first production step is audio analysis: BPM,
  beat grid, song sections, vocal regions, cue sheet, and recommended edit points.
- `preview.html` is the center of the workflow from day one.

## Minimal New Project Structure

```text
my-music-video/
  audio/
    source.mp3
    analysis.json
    beat_grid.csv
    cue_sheet.md
  assets/
    asset_bible.json
  storyboard/
    frames/
    storyboard.json
  refs/
  outputs/
  scripts/
  preview.html
  prompts.json
  project_manifest.json
```

The files in `templates/` are starter files. The agent can copy them into the
new project root and fill them with project-specific data.
