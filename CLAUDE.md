# Travel Map — project context for Claude

## Stack
- `index.html` — single-page app, Leaflet 1.9.4, TopoJSON 3.0.2
- `config.js` — all user data (visited countries + cities), no other files to edit
- No build step, no dependencies, open `index.html` directly or via GitHub Pages

## Architecture
- One shared map, all layers always visible
- Tab switch (Мир / Россия) changes only the sidebar list — map is never re-rendered
- World countries: world-atlas TopoJSON (countries-50m.json)
- Russian regions: codeforamerica/click_that_hood russia.geojson
- City markers: L.marker with emoji 📍 divIcon

## Key conventions

### config.js
- `visited[]` — ISO 3166-1 alpha-2 country codes
- `cities[]` — `{ name, lat, lng, region }` where `region` must exactly match GeoJSON `properties.name`
- Regions missing from GeoJSON use `_` prefix (e.g. `"_Крым"`, `"_ДНР"`) — only pin shown, no polygon

### Exact GeoJSON region names (case-sensitive)
- `"Татарстан"` not "Республика Татарстан"
- `"Башкортостан"` not "Республика Башкортостан"
- `"Бурятия"` not "Республика Бурятия"
- `"Удмуртская республика"` — lowercase р
- `"Чувашская республика"` — lowercase р
- `"Чеченская республика"` — lowercase р
- `"Кабардино-Балкарская республика"` — lowercase р
- `"Карачаево-Черкесская республика"` — lowercase р
- `"Северная Осетия - Алания"` — short hyphen, spaces around it
- `"Республика Дагестан"`, `"Республика Ингушетия"` — with "Республика"
- `"Республика Саха (Якутия)"` — with parentheses

### Hover / tooltip system
- Single GeoJSON layer per dataset (not separate hover + visited layers)
- `_hoveredLayer` global tracks current hovered layer — forcibly reset on new `mouseover`
- `countryClear` style uses `fillOpacity: 0.001` (not 0) so SVG registers mouse events
- No `bringToFront()` in hover — causes lost `mouseout` events

### Sidebar counts
- Мир: visited out of 195
- Россия: visited out of 156 (cities 100k+ in README)

## README
- Contains full copy-paste ready city list grouped by federal district
- Each entry has exact `region` string matching GeoJSON
- Also contains full country code reference (ISO alpha-2)
- When adding a city to config.js, add it to README too (same section/order)
