# ArcGIS JS API 5.0 Style Repro

Minimal swisstopo repro for ArcGIS Maps SDK for JavaScript `5.0`.

The default page now focuses on three modes:

- `CH basemap style`
- `CH data check`
- `CH Pro compatible`

Files used by the default page:

- swisstopo style: `https://vectortiles.geo.admin.ch/styles/ch.swisstopo.basemap.vt/style.json`
- swisstopo TileJSON shown in the panel: `https://vectortiles.geo.admin.ch/tiles/ch.swisstopo.base.vt/v1.0.0/tiles.json`
- public data-check style: `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.data-check.style.json`
- public Pro-compatible style: `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.pro-compatible.style.json`

`CH data check` is the simple local diagnostic style with flat fills and lines only, no labels, no icons, no filters, and no expressions.

`CH Pro compatible` keeps the same swisstopo base TileJSON source, but rewrites the style into a flatter ArcGIS Pro-friendly shape:

- one source only
- no relief, no fill patterns, no sprite icons
- more explicit swisstopo color buckets for landcover, landuse, water, boundaries, and roads
- constant values or classic `stops` instead of nested expressions
- text-only labels for `place`, `water_name`, `waterway`, `park`, `boundary`, `transportation_name`, `area_name`, `poi`, and `aerodrome_label`
- plain token fields such as `{name}`, `{adm2_l}`, and `{adm4_r}` to avoid the previous `name:latin` compatibility path
- elevation-first transport ordering: `surface transport -> buildings -> l1 transport -> l2plus transport -> labels`
- hosted swisstopo transport label palette kept: neutral road labels, warm route-road labels, red rail/aerialway labels, orange drag-lift labels

### Technical Adaptations In `CH Pro compatible`

This style keeps the hosted swisstopo data source and glyphs, then rewrites the style into a shape ArcGIS Pro accepts more reliably.

What changed from hosted swisstopo to the Pro-compatible version:

- kept the same `base_v1.0.0` TileJSON source and `glyphs`
- removed relief, hillshade, `sprite`, `fill-pattern`, and other extras that are not needed for the compatibility test
- replaced expression-heavy styling with many explicit fill/line layers plus constant values or classic `stops`
- kept the main swisstopo colors by splitting landcover, landuse, water, boundary, building, and road classes into separate layers
- simplified labels to text-only symbol layers with plain token fields such as `{name}`, `{adm2_l}`, and `{adm4_r}`
- switched to the simplified transport stack `surface transport -> buildings -> l1 transport -> l2plus transport -> labels`
- split rail/transit into `railway-surface-line`, `railway-l1-line`, and `railway-l2plus-line`
- kept aerialways only as `aerialway-l2plus-line` and drag lifts only as `drag-lift-l2plus-line`
- kept the hosted swisstopo transport label palette: default road labels use `rgb(40, 40, 40)` with a white halo, route-road labels use `rgb(55, 43, 22)` with a beige halo, rail/transit and aerialway labels use `rgb(183, 57, 57)` with a white halo, and drag-lift labels use `rgb(180, 111, 14)` with a white halo
- left out icons, shields, road-number boxes, computed icon/text logic, variable anchors, and full tunnel/bridge parity on purpose

Current shape of the local style:

- `1` vector source
- `92` layers total
- no `sprite`
- no `fill-pattern`
- text labels preserved in simplified form, including cities, selected POI families, and transport labels
- colors preserved through split layers instead of dynamic expressions

Additional test style JSON files are still checked in beside the page for direct ArcGIS Pro tests, but they are no longer linked from the default `index.html`.

## Public URLs

- website: `https://ceddc.github.io/tilejson-repro/`
- data-check style: `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.data-check.style.json`
- Pro-compatible style: `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.pro-compatible.style.json`

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

ArcGIS Pro test URL on the public website:

- `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.data-check.style.json`
- `https://ceddc.github.io/tilejson-repro/swisstopo.basemap.pro-compatible.style.json`

## Expected result

- the selected mode reaches `layerview-ok`
- the panel shows the active primary URL and TileJSON URL
- the console logs `LAYER_OK` and `LAYERVIEW_OK`
