# ArcGIS JS API 5.0 Style Repro

Minimal swisstopo repro for ArcGIS Maps SDK for JavaScript `5.0`.

The default page now focuses on just two modes:

- `CH basemap style`
- `CH data check`

Files used by the default page:

- swisstopo style: `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- swisstopo TileJSON shown in the panel: `https://vectortiles.geo.admin.ch/tiles/ch.swisstopo.base.vt/v1.0.0/tiles.json`
- local data-check style: `./swisstopo.basemap.data-check.style.json`

`CH data check` is the simple local diagnostic style with flat fills and lines only, no labels, no icons, no filters, and no expressions.

The previous multi-mode public page is preserved as:

- `./indexold.html`

## Run

```powershell
cd C:\data\tilejson-repro
& 'C:\data\env\python.exe' -m http.server 8080
```

Then open:

- `http://localhost:8080/index.html`

If you want to compare against the older public page, open:

- `http://localhost:8080/indexold.html`

ArcGIS Pro test URL for the simple local style:

- `http://localhost:8080/swisstopo.basemap.data-check.style.json`

## Expected result

- the selected mode reaches `layerview-ok`
- the panel shows the active primary URL and TileJSON URL
- the console logs `LAYER_OK` and `LAYERVIEW_OK`
