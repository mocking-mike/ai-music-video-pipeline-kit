# Agent Start Prompt

Wklej ponizszy prompt do Codexa, Claude'a albo innego agenta kodowego po
otwarciu pustego projektu.

```text
Pracujesz nad nowym AI music video od zera.

Najpierw przeczytaj:
- ai-music-video-pipeline-kit/PIPELINE.md
- ai-music-video-pipeline-kit/README.md

Traktuj je jako kontrakt produkcyjny.

Cel:
Zbuduj projekt teledysku od zera: audio analysis, storyboard, asset bible,
shot list, preview.html, a potem renderowanie ujec przez fal.ai.

Twarde zasady:
- finalne video generujemy przez fal.ai, nie Atlas Cloud;
- storyboard images sa tylko do fabuly i placeholderow w preview.html;
- nigdy nie uzywaj storyboard images jako reference_images do fal.ai;
- do renderow uzywaj asset bible, approved source footage i approved continuity frames;
- po dodaniu MP3 zawsze najpierw zrob audio analysis: BPM, beat grid, sekcje,
  wokal, cue sheet i rekomendowane punkty ciecia;
- kazdy render zapisuj jako pelny *_source_full.mp4 plus aktywny dociety *.mp4;
- przed kazdym renderem wykonaj continuity decision: sprawdz poprzedni shot,
  a jesli to intercut lub inna przestrzen, znajdz ostatni powiazany shot;
- preview.html jest centrum pracy od poczatku i ma miec zakladki Video Preview
  oraz Storyboard.

Pierwsze zadanie:
1. Utworz strukture katalogow projektu.
2. Skopiuj template'y z ai-music-video-pipeline-kit/templates do root projektu.
3. Jesli nie ma jeszcze MP3, popros mnie o plik audio.
4. Gdy MP3 bedzie dostepne, zacznij od audio analysis i zapisz wyniki w audio/.
5. Dopiero potem przejdz do treatmentu, storyboardu i asset bible.
```

