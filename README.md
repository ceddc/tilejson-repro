# TileJSON Repro

ArcGIS Maps SDK for JavaScript 5.0 repro for ASIT vector tiles, with remote style, local style, local no-hillshade, and raw TileJSON modes.

Small standalone repro for loading the ASIT vector tiles with ArcGIS Maps SDK for JavaScript `5.0`, with switches for the remote style, a checked-in local style copy, a local no-hillshade variant, and the raw TileJSON source.

Style under test:

`https://vt.asit-asso.ch/styles/asit.color/style.json`

Underlying TileJSON source:

`https://vt.asit-asso.ch/tiles/asit/v0.1/3857/tiles.json`

## What it does

- loads ArcGIS JS API `5.0` from the official CDN
- creates a `VectorTileLayer` from the remote ASIT `style.json`, a checked-in local style copy, a checked-in local no-hillshade style copy, or the raw `TileJSON` URL
- lets you switch between all four modes with buttons in the panel
- shows the underlying `TileJSON` URL used by the style
- opens a `MapView` centered on the ASIT coverage area
- shows a small status panel with the current mode, current URL, a short mode note, and exact error text when something fails
- logs `LAYER_OK`, `LAYERVIEW_OK`, `LAYER_FAIL`, `VIEW_FAIL`, and `LAYERVIEW_FAIL` in the browser console

## Local style files

The folder includes:

- `asit.color.style.json`: a checked-in local copy of the ASIT color style
- `asit.color.no-hillshade.style.json`: the same style with the `Hillshade` layer and its `relief` source removed
- `local-styles-data.js`: a local JS data file generated from those checked-in JSON files so the local modes also work when `index.html` is opened directly from disk

## Why style first

The `style.json` is the useful real-world repro because it carries the cartography: colors, labels, layer order, and raster overlays.

The `TileJSON` sits underneath that style and describes the vector tile source itself. In other words:

- `TileJSON` = where the vector tiles come from
- `style.json` = how those tiles should be drawn

This repro loads the remote style by default, but you can switch to a local style copy, a local no-hillshade style, or `TileJSON only` to isolate the raw source.

## Run

Open `index.html` directly in a browser, or serve the folder with any static file server.

The local modes are backed by the checked-in JSON files and mirrored through `local-styles-data.js`, so they also work when `index.html` is opened directly from disk. Serving over HTTP is still fine if you prefer it.

Example with Python:

```powershell
cd C:\data\holistic-tools\arcgis-jsapi5-tilejson-repro
python -m http.server 8080
```

Then open:

`http://localhost:8080/index.html`

## Expected result

- the map draws the ASIT styled vector tiles
- the local style copy also loads against the same TileJSON source
- the local no-hillshade mode removes the hillshade layer from the style
- switching to `TileJSON only` still loads the source successfully, but without the full ASIT cartography
- the status panel ends at `layerview-ok`
- the console includes `LAYER_OK` and `LAYERVIEW_OK`

## Useful for bug reports

Capture:

- a screenshot of the page
- the final status text shown in the panel
- the browser console output
- the exact primary URL and TileJSON URL shown in the page
