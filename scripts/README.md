# Scripts To Create In A New Project

This kit intentionally ships script specs instead of project-specific code.
Ask Codex or Claude to implement these scripts for the new project after it
reads `PIPELINE.md`.

## Required scripts

```text
scripts/analyze_audio.py
```

Input: `audio/source.mp3`

Outputs:

- `audio/analysis.json`
- `audio/beat_grid.csv`
- `audio/cue_sheet.md`

Must detect BPM, beat grid, downbeats, energy curve, sections, vocal regions,
strong transitions and recommended cut points.

```text
scripts/generate_video.py
```

Shared fal.ai helper. Must support:

- reference-to-video,
- image-to-video for transition shots,
- reference upload,
- provider logs,
- saving `*_source_full.mp4`,
- trimming active clip,
- extracting preview/review/transition frames,
- registry update,
- preview update.

```text
scripts/validate_refs.py
```

Must block storyboard images and human-facing example media from final render
refs. In particular, paths under `storyboard/frames/` and `examples/` must never
be passed to fal.ai as `reference_images`, source videos, continuity frames, or
image-to-video inputs.

```text
scripts/resolve_continuity.py
```

Must choose the best continuity source: previous shot if connected, otherwise
the latest older shot with matching location, object, scene, or narrative beat.

```text
scripts/sync_preview.py
```

Must rewrite `preview.html` raw segments from `prompts.json`.

```text
scripts/export_preview_mp4.py
```

Must render current `preview.html` timeline to a review MP4.
