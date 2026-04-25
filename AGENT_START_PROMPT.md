# Agent Start Prompt

Paste the prompt below into Codex, Claude, or another coding agent after opening
an empty project.

```text
You are working on a new AI music video from scratch.

First read:
- ai-music-video-pipeline-kit/PIPELINE.md
- ai-music-video-pipeline-kit/README.md

Treat those files as the production contract.

Goal:
Build the music-video project from zero: audio analysis, storyboard, asset
bible, shot list, preview.html, and then shot rendering through fal.ai.

Hard rules:
- final video is generated through fal.ai, not Atlas Cloud;
- storyboard images are only for story planning and preview.html placeholders;
- never use storyboard images as reference_images for fal.ai;
- files under examples/ are human-facing demos only and must never be used as
  source footage, asset-bible items, continuity frames, or render references;
- final renders may use only the asset bible, approved source footage created
  inside the active project, and approved continuity frames;
- after an MP3 is added, always start with audio analysis: BPM, beat grid,
  song sections, vocal regions, cue sheet, and recommended cut points;
- every render must save the full *_source_full.mp4 plus the active trimmed *.mp4;
- before every render, make a continuity decision: inspect the previous shot,
  and if it is an intercut or different location, find the latest related shot;
- preview.html is the center of the workflow from the start and must have
  `Video Preview` and `Storyboard` tabs.

First task:
1. Create the project directory structure.
2. Copy the templates from ai-music-video-pipeline-kit/templates into the project root.
3. If there is no MP3 yet, ask me for the audio file.
4. Once the MP3 is available, start with audio analysis and save the results in audio/.
5. Only then move on to the treatment, storyboard, and asset bible.
```
