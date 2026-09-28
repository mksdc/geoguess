# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Browser-based GeoGuessr clone on Google Street View. Plain static HTML with no build step, package manager or tests: each game is one self-contained `.html` file with inline CSS and JS. The site is published via GitHub Pages at https://mksdc.github.io/geoguess/, and `index.html` only redirects to `geoguess.html`.

UI text, comments and the README are in **German**. Keep new strings and comments in German too.

## Running

Open `geoguess.html` directly in a browser, or serve the folder (e.g. `python3 -m http.server`). Playing needs a Google Maps API key with the "Maps JavaScript API" enabled and billing set up. You can't verify gameplay end-to-end without one.

## Two variants kept in sync

- `geoguess.html` is the published version. The user types the key on the start screen, and it is stored in `localStorage` under `gg_key`.
- `geoguess_api.html` has the key hardcoded in `const API_KEY`. When a real key is set, the input is hidden. It is for local hosting only, never commit a real key. The README cites the line number of `API_KEY` (currently 161–162), so update the README if that line moves.

The two files are otherwise identical: `diff geoguess.html geoguess_api.html` should show only the key-handling differences. **Apply every gameplay/UI change to both files.**

## Game architecture (inside the `<script>` block)

- The Maps JS API is loaded dynamically on "Spiel starten", with `callback=initGame`. `gm_authFailure` catches rejected keys.
- Location picking: `findRandomPanorama()` makes up to 40 tries. The player picks a game type from `MODES` on the start screen (`#mode`) and again on the end screen (`#endMode`). Both selects are filled from `MODES` and kept in sync by `chooseMode()`, which sets `mode` and remembers the choice in `localStorage` as `gg_mode`. On each try, `mode.share` decides between:
  - City mode: `randomCityPoint()` picks a random point within `mode.spread` km of a random entry in `CITIES`, and the panorama search radius is 1 km.
  - Region mode: `randomRegionPoint()` picks a weighted-random box from `REGIONS` and a random point inside it, and the search radius is 50 km.

  Both modes accept only official Google imagery. Region mode tends to land on rural roads, which is why cities were added. The mode select sits outside `#keyBox` in `geoguess_api.html` so it stays visible when the key is embedded.
- The panorama hides the address and road labels (`addressControl`, `showRoadLabels: false`). Keep this, or the game is trivial.
- Scoring: `MAX_POINTS * exp(-km / 2000)` per round, using the haversine distance, over `ROUNDS` = 5 rounds.
- Screens/layers (`#start`, `#game` → `#streetLayer`/`#mapLayer`, `#end`) are switched with the `.hidden` class (visibility, not display, so the Maps containers keep their size).
- Colors are CSS variables in `:root` (`--guess` yellow, `--target` green, `--line` red). `pin()` and the polyline repeat the hex values, so change both places together.
