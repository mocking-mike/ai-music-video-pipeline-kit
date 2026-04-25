# AI Music Video Pipeline Kit

Samowystarczalny starter-kit do zrobienia AI video od zera razem z Codexem,
Claude'em albo innym agentem kodowym. Glowny przyklad uzycia: teledysk
generowany i montowany pod konkretny plik MP3.

Ten folder nie zaklada, ze masz jakikolwiek istniejacy projekt. Mozesz go
skopiowac do pustego katalogu, dodac MP3 i zaczac prace od podstaw.

## Co jest w srodku

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
```

## Jak tego uzyc

1. Skopiuj caly folder `ai-music-video-pipeline-kit` do nowego pustego katalogu.
2. Zmien nazwe folderu projektu, np. na `my-music-video`.
3. Wloz plik MP3 do `audio/source.mp3` albo popros agenta, zeby utworzyl strukture.
4. Otworz projekt w Codexie albo Claude Code.
5. Wklej agentowi zawartosc `AGENT_START_PROMPT.md`.
6. Agent powinien zaczac od analizy audio, a dopiero potem robic storyboard,
   asset bible, shot list, preview i generowanie video.

## Najwazniejsze zasady

- Video generation idzie przez fal.ai.
- Storyboard jest tylko mapa fabuly i placeholderem w `preview.html`.
- Storyboard images nie moga byc uzywane jako `reference_images` do renderow.
- Finalne renderowanie uzywa tylko asset bible, approved footage i approved
  continuity frames.
- Kazdy render zapisuje pelne `*_source_full.mp4` plus aktywny montazowy `*.mp4`.
- Po dodaniu MP3 najpierw powstaje analiza: BPM, beat grid, sekcje, wokal i cue sheet.
- `preview.html` jest centrum pracy od pierwszego dnia.

## Minimalny start projektu

Docelowa struktura pustego projektu:

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

Folder `templates/` zawiera pliki startowe, ktore agent moze skopiowac do
roota nowego projektu i wypelnic danymi konkretnego klipu.
