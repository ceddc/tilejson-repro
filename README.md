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

### Technical Adaptations In `CH Pro compatible`

The goal of this variant is not visual parity with the hosted swisstopo style. The goal is to keep the same data source, preserve recognizable swisstopo colors and labels, and encode them in a style shape that is closer to what ArcGIS Pro appears to accept.

Source and top-level changes:

- keeps the same `base_v1.0.0` TileJSON source
- keeps `version: 8`
- keeps `glyphs`
- removes the relief source and all relief-backed layers
- removes `sprite`
- removes `fill-pattern`
- removes nonessential top-level extras such as `transition`

Fill and line styling changes:

- starts from the working `CH data check` idea: one source and simple geometry rendering
- rebuilds color classes by splitting the map into many explicit layers instead of using one expression-heavy layer
- uses separate layers for glacier, forest, park-green landcover, sand, residential landuse, industrial landuse, utility landuse, pitch areas, lake/other water, bathymetry, construct, aeroway, and building fills
- uses separate transportation layers for motorway/trunk, route overlays, primary/secondary, tertiary/minor/service, path/track, railway, and aerialway
- uses separate boundary band and line layers for country, region, and local admin levels
- uses classic zoom `stops` for widths, sizes, and a few opacity ramps
- avoids nested `match`, `case`, `interpolate`, `step`, `%`, `to-number`, and `has` logic in paint/layout

Label changes:

- labels are text-only symbol layers, with no icon+text combo layers
- label families are split into explicit layers:
  - `place-country`, `place-city`, `place-town`, `place-village`, `place-other`
  - `water-name`
  - `waterway`
  - `park`
  - `boundary-country-left/right`, `boundary-region-left/right`
  - `transportation-road`, `transportation-rail`
  - `area-name-landform`, `area-name-glacier`, `area-name-field`
- `poi-transport`, `poi-nature`, `poi-history`, `poi-public`, `poi-motorway`
- `aerodrome`
- label fields are simple token strings such as `{name}`, `{adm2_l}`, `{adm2_r}`, `{adm4_l}`, and `{adm4_r}`
- label sizes use constant values or `stops`
- label paint is fixed per layer: text color, halo color, halo width, and halo blur are all simple values or `stops`

Deliberately not carried over from the hosted swisstopo style:

- relief and hillshade rendering
- sprite-driven icons and shields
- road-number boxes
- computed `icon-image`
- computed `text-field`
- computed `text-size`
- computed `text-opacity`
- computed `symbol-sort-key`
- `text-variable-anchor`
- width-, population-, size-, rank-, elevation-, or depth-driven expression logic

Current shape of the local style:

- `1` vector source
- `62` layers total
- no `sprite`
- no `fill-pattern`
- text labels preserved in simplified form, including cities and selected POI families
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
