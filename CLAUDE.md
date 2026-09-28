# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Browser-based GeoGuessr clone on Google Street View. Plain static HTML with no build step, package manager or tests: each game is one self-contained `.html` file with inline CSS and JS. The site is published via GitHub Pages at https://mksdc.github.io/geoguess/, and `index.html` only redirects to `geoguess.html`.

UI text, comments and the README are in **German**. Keep new strings and comments in German too.

## Running

Open `geoguess.html` directly in a browser, or serve the folder (e.g. `python3 -m http.server`). Playing needs a Google Maps API key with the "Maps JavaScript API" enabled and billing set up. You can't verify gameplay end-to-end without one.

## Two variants kept in sync

- `geoguess.html` is the published version. The user types the key on the start screen, and it is stored in `localStorage` under `gg_key`.
- `geoguess_api.html` has the key hardcoded in `const API_KEY`. When a real key is set, the input is hidden. It is for local hosting only, never commit a real key. The README cites the line number of `API_KEY` (currently 157–158), so update the README if that line moves.

The two files are otherwise identical: `diff geoguess.html geoguess_api.html` should show only the key-handling differences. **Apply every gameplay/UI change to both files.**

## Game architecture (inside the `<script>` block)

- The Maps JS API is loaded dynamically on "Spiel starten", with `callback=initGame`. `gm_authFailure` catches rejected keys.
- Location picking: `randomPoint()` does a weighted-random pick from `REGIONS` (`[minLat, maxLat, minLng, maxLng, weight]`, areas with good Street View coverage), then `findRandomPanorama()` calls `StreetViewService.getPanorama` with a 50 km radius and only official Google imagery. It makes up to 40 tries.
- The panorama hides the address and road labels (`addressControl`, `showRoadLabels: false`). Keep this, or the game is trivial.
- Scoring: `MAX_POINTS * exp(-km / 2000)` per round, using the haversine distance, over `ROUNDS` = 5 rounds.
- Screens/layers (`#start`, `#game` → `#streetLayer`/`#mapLayer`, `#end`) are switched with the `.hidden` class (visibility, not display, so the Maps containers keep their size).
- Colors are CSS variables in `:root` (`--guess` yellow, `--target` green, `--line` red). `pin()` and the polyline repeat the hex values, so change both places together.
