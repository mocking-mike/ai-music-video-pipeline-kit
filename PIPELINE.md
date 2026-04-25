# Pipeline Contract

Ten dokument jest kontraktem pracy dla agenta. Agent powinien przeczytac go
przed rozpoczeciem projektu i trzymac sie zasad przez caly proces.

## 0. Projekt startuje od zera

Zakladamy, ze uzytkownik ma tylko:

- plik MP3,
- pomysl na teledysk albo krotki opis klimatu,
- ewentualnie przyklady wizualne, ktore trzeba dopiero przepisac na asset bible.

Nie zakladamy istnienia zadnego starego repo, outputow, storyboardu ani assetow.

## 1. Kolejnosc pracy

1. Utworz strukture projektu.
2. Skopiuj template'y: `project_manifest.json`, `prompts.json`,
   `asset_bible.json`, `storyboard.json`, `preview.html`.
3. Umiesc MP3 w `audio/source.mp3`.
4. Zrob audio analysis.
5. Zrob treatment i podzial na sekcje muzyczne.
6. Zrob storyboard jako opis fabuly i/lub obrazki placeholderowe.
7. Zbuduj asset bible.
8. Zrob shot list i `prompts.json`.
9. Od razu uruchom `preview.html` na storyboard placeholderach.
10. Generuj ujecia przez fal.ai.
11. Po kazdym renderze aktualizuj preview.
12. Eksportuj review MP4 z aktualnego preview.

## 2. Audio analysis przed wszystkim

Po dodaniu MP3 agent musi przygotowac:

```text
audio/
  analysis.json
  beat_grid.csv
  cue_sheet.md
```

`analysis.json`:

- duration,
- bpm,
- bpm_confidence,
- beat_grid,
- bars,
- downbeats,
- energy_curve,
- vocal_sections,
- lyric_phrases, jesli mozliwe,
- section_markers,
- recommended_cuts,
- moments_to_hold,
- moments_for_fast_cutting.

`cue_sheet.md` ma byc czytelny dla czlowieka:

```markdown
| Time | Cue | What happens in music | Editing use |
| --- | --- | --- | --- |
| 00:00.00 | intro | Ambient start | Establish world |
| 00:11.25 | vocal_in | First vocal phrase | Start act 1 |
| 01:02.80 | drop | Strong reset | Hard cut / black frame |
```

Shot timings musza wynikac z audio analysis, nie z recznego zgadywania.

## 3. Storyboard rules

Storyboard sluzy tylko do:

- fabuly,
- blocking,
- emocji sceny,
- placeholderow w `preview.html`,
- drugiej zakladki `Storyboard`.

Storyboard nie moze byc uzyty jako:

- reference image do fal.ai,
- start frame finalnego renderu,
- continuity frame,
- source image do image-to-video,
- image edit base.

Kazdy prompt moze zawierac "storyboard intent", ale nie moze przekazac
storyboard image jako visual reference.

## 4. Asset bible rules

Asset bible jest jedynym zrodlem tozsamosci postaci, propsow, marek i UI.

Asset bible powinna zawierac:

- glowne postacie,
- warianty postaci: front, back, closeup, hands, wardrobe,
- branding,
- produkty,
- stale propsy,
- UI plates,
- style references,
- forbidden drift notes.

Lokacje mozna budowac tekstem w modelu video, a potem utrwalac approved
continuity frames. Nie trzeba generowac kazdej lokacji jako osobnego assetu.

## 5. Video generation

Aktywny provider: fal.ai.

Domyslny model:

```text
bytedance/seedance-2.0/reference-to-video
```

Do przejsc i mostkow:

```text
bytedance/seedance-2.0/image-to-video
```

Kazdy generator shota musi:

- brac referencje tylko z asset bible lub approved refs,
- walidowac, ze zaden path ze storyboardu nie trafia do `reference_images`,
- zapisac pelny source clip,
- zapisac aktywny clip montazowy,
- wyciagnac preview, review i transition frames,
- zaktualizowac `prompts.json`,
- zaktualizowac `preview.html`.

## 6. Source clip policy

Kazdy render zapisujemy tak:

```text
outputs/<act>/shot_<id>/
  shot_<id>_<version>_source_full.mp4
  shot_<id>_<version>.mp4
  shot_<id>_<version>_source_image.png
  shot_<id>_preview_<version>.png
```

Pelny `*_source_full.mp4` nigdy nie jest nadpisywany przez trim. Aktywny
montazowy `*.mp4` moze byc dociety, ale timeline w `preview.html` powinien
umiec pracowac na `source_video`, `sourceStart` i `playbackRate`.

## 7. Continuity resolver

Przed renderem agent wykonuje decyzje continuity.

Nie wystarczy "wez poprzedni shot". Agent ma:

1. sprawdzic shot N-1,
2. ocenic, czy jest ta sama przestrzen/scena/obiekt,
3. jesli N-1 to intercut, montaz albo inna przestrzen, przeszukac starsze shoty,
4. znalezc ostatni logicznie powiazany shot,
5. zapisac decyzje w `prompts.json`.

Tryby:

- `new_scene`,
- `same_scene_next_shot`,
- `return_to_previous_scene`,
- `match_cut`,
- `montage_insert`,
- `storyboard_placeholder_only`.

## 8. Preview-first workflow

`preview.html` istnieje od poczatku i ma dwie zakladki:

- `Video Preview`,
- `Storyboard`.

Na poczatku `Video Preview` moze pokazywac storyboard images jako placeholdery.
Po renderze segment zostaje przepiety na video. Storyboard zostaje w drugiej
zakladce tylko jako mapa fabuly.

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

Statusy:

- `storyboard`,
- `generated`,
- `needs_rerender`,
- `approved`,
- `manual`.

## 9. Prompt format

Kazdy prompt do video powinien zawierac:

- role kazdego `@Image`,
- storyboard intent jako tekst, jesli potrzebny,
- action,
- camera,
- environment,
- continuity,
- constraints,
- next-shot usefulness.

Dobry wzor:

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

Zly wzor:

```text
Animate this storyboard image.
```

## 10. Definition of done

Projekt jest gotowy do review, gdy:

- `audio/analysis.json`, `beat_grid.csv`, `cue_sheet.md` istnieja,
- `preview.html` odtwarza caly timeline,
- kazdy shot ma status inny niz pusty,
- storyboard images nie sa uzyte jako render references,
- kazdy wygenerowany shot ma `source_full`,
- registry wskazuje aktualne video i source video,
- da sie wyeksportowac MP4 review z aktualnego preview.

