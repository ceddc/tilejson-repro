# TileJSON Repro

Minimal repro for loading vector tiles with ArcGIS Maps SDK for JavaScript `5.0`.

Modes:
- `ASIT remote style`
- `CH basemap style`
- `CH basemap Pro lite`
- `ASIT TileJSON`

URLs used:
- ASIT style: `https://vt.asit-asso.ch/styles/asit.color/style.json`
- ASIT TileJSON: `https://vt.asit-asso.ch/tiles/asit/v0.1/3857/tiles.json`
- swisstopo style: `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- swisstopo TileJSON shown in the panel: `https://vectortiles.geo.admin.ch/tiles/ch.swisstopo.base.vt/v1.0.0/tiles.json`
- local Pro-lite style: `./swisstopo.basemap.pro-lite.style.json`

`style.json` is the useful cartographic repro. `TileJSON` is the raw source-only repro.

## CH basemap Pro lite

`CH basemap Pro lite` is a checked-in swisstopo derivative style for ArcGIS Pro testing. It keeps only the base TileJSON source, removes relief, removes sprite icons, and keeps text labels only.

## Run

Open `index.html` directly, or serve the folder:

```powershell
cd C:\data\tilejson-repro
& 'C:\data\env\python.exe' -m http.server 8080
```

Then open `http://localhost:8080/index.html`.

Test `CH basemap Pro lite` over `http://localhost`, not `file:///`, because it loads a local JSON style file.

ArcGIS Pro test URL:

`http://localhost:8080/swisstopo.basemap.pro-lite.style.json`

## Expected result

- the selected mode reaches `layerview-ok`
- the panel shows the active primary URL and TileJSON URL
- the console logs `LAYER_OK` and `LAYERVIEW_OK`
