# ArcGIS JS API 5.0 Style Repro

Minimal swisstopo repro for ArcGIS Maps SDK for JavaScript `5.0`.

The default page now focuses on three modes:

- `CH basemap style`
- `CH data check`
- `CH Pro compatible`

Files used by the default page:

- swisstopo style: `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- swisstopo TileJSON shown in the panel: `https://vectortiles.geo.admin.ch/tiles/ch.swisstopo.base.vt/v1.0.0/tiles.json`
- local data-check style: `./swisstopo.basemap.data-check.style.json`
- local Pro-compatible style: `./swisstopo.basemap.pro-compatible.style.json`

`CH data check` is the simple local diagnostic style with flat fills and lines only, no labels, no icons, no filters, and no expressions.

`CH Pro compatible` keeps the same swisstopo base TileJSON source, but rewrites the style into a flatter ArcGIS Pro-friendly shape:

- one source only
- no relief, no fill patterns, no sprite icons
- many simple fill and line layers with explicit filters
- constant values or classic `stops` instead of nested expressions
- text-only labels for `place`, `water_name`, `waterway`, `boundary`, `transportation_name`, and `area_name`
- token-based label fields such as `{name:latin}`, `{adm2_l}`, and `{adm4_r}`

Additional test style JSON files are still checked in beside the page for direct ArcGIS Pro tests, but they are no longer linked from the default `index.html`.

## Run

```powershell
cd C:\data\tilejson-repro
& 'C:\data\env\python.exe' -m http.server 8080
```

Then open:

- `http://localhost:8080/index.html`

ArcGIS Pro test URL for the simple local style:

- `http://localhost:8080/swisstopo.basemap.data-check.style.json`

ArcGIS Pro test URL for the Pro-compatible style:

- `http://localhost:8080/swisstopo.basemap.pro-compatible.style.json`

## Expected result

- the selected mode reaches `layerview-ok`
- the panel shows the active primary URL and TileJSON URL
- the console logs `LAYER_OK` and `LAYERVIEW_OK`
