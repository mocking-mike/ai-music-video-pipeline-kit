# Pipeline Contract

This document is the production contract for the coding agent. The agent should
read it before starting work and follow it throughout the project.

## 0. The Project Starts From Zero

Assume the user has only:

- an MP3 file,
- a music-video idea or a short mood description,
- optional visual examples that still need to be translated into an asset bible.

Do not assume an existing repository, previous renders, storyboard, or asset pack.

## 1. Work Order

1. Create the project directory structure.
2. Copy the templates: `project_manifest.json`, `prompts.json`,
   `asset_bible.json`, `storyboard.json`, and `preview.html`.
3. Place the MP3 at `audio/source.mp3`.
4. Run audio analysis.
5. Create the treatment and map it to the musical sections.
6. Create the storyboard as story text and/or placeholder images.
7. Build the asset bible.
8. Create the shot list and `prompts.json`.
9. Start working in `preview.html` immediately, using storyboard placeholders at first.
10. Generate shots through fal.ai.
11. Update the preview after every render.
12. Export review MP4s from the current preview timeline.

## 2. Audio Analysis Comes First

After the MP3 is added, the agent must create:

```text
audio/
  analysis.json
  beat_grid.csv
  cue_sheet.md
```

`analysis.json` should include:

- duration,
- BPM,
- BPM confidence,
- beat grid,
- bars,
- downbeats,
- energy curve,
- vocal sections,
- lyric phrases when possible,
- section markers,
- recommended cut points,
- moments that should hold longer,
- moments that invite faster cutting.

`cue_sheet.md` should be readable by a director or editor:

```markdown
| Time | Cue | What happens in the music | Editing use |
| --- | --- | --- | --- |
| 00:00.00 | intro | Ambient start | Establish the world |
| 00:11.25 | vocal_in | First vocal phrase | Start act 1 |
| 01:02.80 | drop | Strong reset | Hard cut / black frame |
```

Shot timing must come from the audio analysis, not from guesswork.

## 3. Storyboard Rules

The storyboard is only for:

- story structure,
- blocking,
- scene emotion,
- placeholders in `preview.html`,
- the separate `Storyboard` tab.

The storyboard must never be used as:

- a fal.ai reference image,
- the start frame for a final render,
- a continuity frame,
- a source image for image-to-video,
- an image-edit base.

Prompts may include "storyboard intent" as text, but they must not pass the
storyboard image as a visual reference.

## 4. Human Example Media Rules

The repository may include finished example outputs for humans who want to
understand what the pipeline can produce. These live under `examples/`.

Example media is documentation, not production material. It must never be used
as:

- source footage,
- a fal.ai reference,
- a continuity frame,
- an asset-bible item,
- a storyboard frame,
- a target frame,
- an image-to-video or video-to-video input,
- training data for a new project.

When starting a new project from this kit, copy the templates and docs, but do
not copy `examples/media/` into the active project workspace unless the user
explicitly wants to keep it as documentation. Even then, validators must treat
`examples/` as a forbidden render-reference root.

## 5. Asset Bible Rules

The asset bible is the source of truth for characters, props, brands, UI, and
visual identity.

The asset bible should include:

- main characters,
- character variants: front, back, close-up, hands, wardrobe,
- brand assets,
- products,
- recurring props,
- UI plates,
- style references,
- forbidden drift notes.

Locations can be built from text inside the video model and then locked in with
approved continuity frames. You do not need to generate every location as a
separate static asset.

## 6. Video Generation

Active provider: fal.ai.

Default model:

```text
bytedance/seedance-2.0/reference-to-video
```

For transitions and bridge shots:

```text
bytedance/seedance-2.0/image-to-video
```

Every shot generator must:

- use references only from the asset bible or approved refs,
- validate that no storyboard path is passed to `reference_images`,
- validate that no file under `examples/` is passed to `reference_images`,
- save the full source clip,
- save the active edited clip,
- extract preview, review, and transition frames,
- update `prompts.json`,
- update `preview.html`.

## 7. Source Clip Policy

Every render is saved like this:

```text
outputs/<act>/shot_<id>/
  shot_<id>_<version>_source_full.mp4
  shot_<id>_<version>.mp4
  shot_<id>_<version>_source_image.png
  shot_<id>_preview_<version>.png
```

The full `*_source_full.mp4` is never overwritten by a trim. The active edited
`*.mp4` may be trimmed, but the timeline in `preview.html` should be able to
work from `source_video`, `sourceStart`, and `playbackRate`.

## 8. Continuity Resolver

Before rendering, the agent makes a continuity decision.

It is not enough to blindly use the previous shot. The agent must:

1. inspect shot N-1,
2. decide whether it shares the same space, scene, object, or story beat,
3. if N-1 is an intercut, montage insert, or different location, search older shots,
4. find the most recent logically connected shot,
5. record the decision in `prompts.json`.

Modes:

- `new_scene`,
- `same_scene_next_shot`,
- `return_to_previous_scene`,
- `match_cut`,
- `montage_insert`,
- `storyboard_placeholder_only`.

Example:

```json
{
  "mode": "return_to_previous_scene",
  "checked_previous_shots": ["019", "018"],
  "source_shot": "018",
  "source_frame": "refs/act1/transitions/shot_018_end_frame.png",
  "reason": "Shot 019 is a surreal phone-world intercut. Shot 020 returns to the bank office.",
  "inherit": ["room geometry", "light direction", "desk relationship"],
  "do_not_inherit": ["exact pose", "temporary gesture", "background extras"]
}
```

## 9. Preview-First Workflow

`preview.html` exists from the beginning and has two tabs:

- `Video Preview`,
- `Storyboard`.

At first, `Video Preview` may show storyboard images as placeholders. After a
render exists, the segment is switched to video. The storyboard remains in the
second tab only as a story map.

Segment format:

```js
[
  shot,
  start,
  end,
  section,
  title,
  video,
  sourceStart,
  playbackRate,
  storyboardImage,
  status
]
```

Statuses:

- `storyboard`,
- `generated`,
- `needs_rerender`,
- `approved`,
- `manual`.

## 10. Prompt Format

Every video prompt should include:

- the role of each `@Image`,
- storyboard intent as text, if useful,
- action,
- camera,
- environment,
- continuity,
- constraints,
- next-shot usefulness.

Good pattern:

```text
Storyboard intent only: the hero notices the rival billboard and stops.
Use @Image1 only for hero face identity.
Use @Image2 only for hero full-body wardrobe and silhouette.
Use @Image3 only for rival poster branding.
Build the street from text and asset cues, not from the storyboard still.
Keep the final frame useful for the next shot: the hero should end under the
billboard, looking away from it toward the city.
Constraints: no storyboard image inheritance, no extra hero duplicate, no
readable city names, no landmark skyline.
```

Bad pattern:

```text
Animate this storyboard image.
```

## 11. Definition Of Done

The project is ready for review when:

- `audio/analysis.json`, `beat_grid.csv`, and `cue_sheet.md` exist,
- `preview.html` plays the full timeline,
- every shot has a non-empty status,
- storyboard images are not used as render references,
- example media is not used as source footage or render references,
- every generated shot has a `source_full`,
- the registry points to the current video and source video,
- a review MP4 can be exported from the current preview.
