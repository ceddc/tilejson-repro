# ArcGIS JS API 5.0 Style Repro

Minimal repro for loading vector tiles with ArcGIS Maps SDK for JavaScript `5.0`.

Modes:
- `ASIT remote style`
- `ASIT no relief`
- `ASIT no relief + TileJSON 2`
- `CH basemap style`
- `CH basemap Pro lite`
- `CH data check`
- `ASIT TileJSON`

URLs used:
- ASIT style: `https://vt.asit-asso.ch/styles/asit.color/style.json`
- ASIT TileJSON: `https://vt.asit-asso.ch/tiles/asit/v0.1/3857/tiles.json`
- local ASIT no-relief style: `./asit.color.no-relief.style.json`
- local ASIT TileJSON 2 wrapper: `./asit.tiles.tilejson-2.0.json`
- local ASIT no-relief TileJSON 2 style: `./asit.color.no-relief.tilejson-2.0.style.json`
- swisstopo style: `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- swisstopo TileJSON shown in the panel: `https://vectortiles.geo.admin.ch/tiles/ch.swisstopo.base.vt/v1.0.0/tiles.json`
- local Pro-lite style: `./swisstopo.basemap.pro-lite.style.json`
- local data-check style: `./swisstopo.basemap.data-check.style.json`

`style.json` is the useful cartographic repro. `TileJSON` is the raw source-only repro.

## ASIT no relief

`ASIT no relief` is a checked-in local copy of the ASIT color style with only two changes:

- removed the `relief` raster source
- removed the `Hillshade` raster layer that used that source

Everything else is left as-is. This is the closest possible ASIT comparison when testing whether ArcGIS Pro rejects the style because of the secondary raster source.

## ASIT TileJSON 2 wrapper

`ASIT no relief + TileJSON 2` uses the same no-relief ASIT style, but points it at a local TileJSON wrapper.

This wrapper changes only one thing:

- `tilejson` is advertised as `2.0.0` instead of `3.0.0`

The actual tile URLs, vector layers, bounds, min/max zoom, and styling stay the same. This isolates the TileJSON version metadata as a possible ArcGIS Pro compatibility issue.

## CH basemap Pro lite

`CH basemap Pro lite` is a checked-in swisstopo derivative style for ArcGIS Pro testing. It keeps only the base TileJSON source, removes relief, removes sprite icons, and keeps text labels only.

Adaptations made in the simple style:

- derived from `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- kept only the `base_v1.0.0` source and removed the `relief_v1.0.0` source
- removed all layers that depended on the relief source
- removed the top-level `sprite` reference
- removed all `fill-pattern` usage
- kept `background`, `fill`, and `line` layers from the base source only
- trimmed `fill` paint to `fill-color`, `fill-opacity`, and `fill-antialias`
- trimmed `line` paint to `line-color`, `line-width`, `line-opacity`, `line-dasharray`, and `line-offset`
- trimmed `line` layout to `line-cap`, `line-join`, and `visibility`
- kept only text-oriented symbol layers from `place`, `area_name`, `boundary`, `transportation_name`, `water_name`, and `waterway`
- dropped symbol categories such as `poi`, `aerodrome_label`, `mountain_peak`, `spot_elevation`, `contour_line_pt`, `landcover_pt`, `park`, and `housenumber`
- removed all `icon-*` properties from kept symbol layers
- kept only text label properties such as `text-field`, `text-font`, `text-size`, `text-anchor`, `text-offset`, and halo/color settings
- preserved layer `filter`, `minzoom`, and `maxzoom` where they existed

## CH data check

`CH data check` is an even simpler diagnostic style. It uses only the base TileJSON source and a handful of flat-color `fill` and `line` layers with no labels, icons, filters, or expressions.

Layers included in the data-check style:

- `landcover`
- `landuse`
- `water`
- `bathymetry`
- `construct`
- `aeroway`
- `building`
- `building_ln`
- `transportation`
- `waterway`
- `boundary`
- `contour_line`

## Run

Open `index.html` directly, or serve the folder:

```powershell
cd C:\data\tilejson-repro
& 'C:\data\env\python.exe' -m http.server 8080
```

Then open `http://localhost:8080/index.html`.

Test `CH basemap Pro lite` over `http://localhost`, not `file:///`, because it loads a local JSON style file.

`CH data check` should also be tested over `http://localhost`.

`ASIT no relief` should also be tested over `http://localhost`.

`ASIT no relief + TileJSON 2` should also be tested over `http://localhost`.

ASIT ArcGIS Pro test URL:

`http://localhost:8080/asit.color.no-relief.style.json`

ASIT TileJSON 2 wrapper URL:

`http://localhost:8080/asit.tiles.tilejson-2.0.json`

ASIT TileJSON 2 style URL:

`http://localhost:8080/asit.color.no-relief.tilejson-2.0.style.json`

ArcGIS Pro test URL:

`http://localhost:8080/swisstopo.basemap.pro-lite.style.json`

Diagnostic ArcGIS Pro test URL:

`http://localhost:8080/swisstopo.basemap.data-check.style.json`

## Expected result

- the selected mode reaches `layerview-ok`
- the panel shows the active primary URL and TileJSON URL
- the console logs `LAYER_OK` and `LAYERVIEW_OK`
