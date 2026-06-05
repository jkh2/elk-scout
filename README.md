# ELK SCOUT v1.3 — Colorado Field Intelligence
**Real sighting data. Transparent scoring. AI-powered field analysis. Less guesswork.**

A browser-based elk scouting intelligence tool for Colorado hunters, built on verified citizen science data from iNaturalist and official CPW Game Management Unit boundaries. ELK SCOUT analyzes thousands of research-grade elk observations to surface where animals are actually showing up — broken down by elevation, cover, season, and road pressure — then lets you interrogate the data with an embedded AI hunting analyst.

**Live demo:** [jkh2.github.io/elk-scout](https://jkh2.github.io/elk-scout)

---

## What It Does

ELK SCOUT pulls live, verified elk sighting data from the iNaturalist API, applies a five-factor scoring model to every grid cell across Colorado, and renders the results as an interactive map with ranked hot zones. An embedded AI Field Intelligence analyst — powered by your choice of provider — can interpret your results, explain the scoring, and give you tactical hunting advice calibrated to the actual data on screen.

No guessing, no outdated paper maps — pattern intelligence derived from real observations, with an AI that knows what the numbers mean.

---

## Five-Factor Scoring Model

Every 0.25° grid cell — roughly 17 miles north/south by 13–14 miles east/west at Colorado latitudes (~220–240 sq mi) — is scored 0–100 based on:

| Factor | Weight | What It Measures |
|---|---|---|
| Sighting Density | 40 pts | Log-scaled observation count — more sightings, higher signal |
| Season Match | 20 pts | How closely observed months align with your selected hunting season |
| Elevation Zone | 20 pts | Alpine, mid-mountain, or transition — calibrated to elk habitat preference |
| Cover Proxy | 10 pts | Terrain and region type correlated with cover availability |
| Road Pressure | 10 pts | Distance from high-traffic corridors — backcountry scores higher |

Scores combine into a single 0–100 Intelligence Score per zone. Zones are ranked, color-coded, and plotted on the map.

### Score Legend

| Color | Score | Classification |
|---|---|---|
| 🔴 Red | ≥ 80 | PRIME ZONE |
| 🟡 Amber | 60–79 | HOT ZONE |
| 🟢 Green | 40–59 | ACTIVE |
| ⬛ Dark | < 40 | SPARSE |

---

## Features

### Live Data
- Pulls from the iNaturalist API — research-grade observations only
- Up to 1,600 verified elk sightings per run (8 pages × 200 results)
- Transparent page-cap warning when total available sightings exceed the fetch limit
- Colorado bounding box: 36.99°N–41.0°N, 109.06°W–102.04°W

### Map Layers (Toggle in header)
- **GRID** — Scored zone rectangles with individual sighting dots in hot areas; click any zone for full intelligence breakdown
- **HEAT** — Score-weighted density heatmap; hotspots glow brighter based on composite score, not just raw observation count
- **BOTH** — Heat layer underneath grid at reduced opacity — maximum information density

### Base Layer (MAP | SAT | TOPO toggle)
Switch between three base tile sources at any time — before or after a scout run:
- **MAP** — OpenStreetMap street layer with dark tactical filter applied
- **SAT** — ESRI World Imagery satellite view (same source as CPW's own hunting atlas) — true terrain colors, no filter. See timber stands, open parks, drainages, and ridgelines as they actually look.
- **TOPO** — USGS National Map topographic layer — official USGS topo quads rendered as web tiles. Contour lines, elevation labels, named peaks, drainages, and land cover. The same data source hunters have trusted for decades, now overlaid with your scored elk zones.

All data layers — grid zones, heat map, GMU boundaries — remain visible on top of whichever base layer is active. No API key required for any base layer.

### CPW GMU Overlay
- Official Colorado Parks & Wildlife Game Management Unit boundaries
- Lazy-loaded on demand (not fetched until you click **GMU UNITS**)
- Each unit shows: GMU number, Elk DAU designation, county, area (sq mi), elk sighting count, and sightings-per-100-sq-mi density
- GMU labels color-coded by sighting density (amber = high activity, green = moderate, dim = sparse)
- Hover any unit to highlight it; click for full popup
- **Sighting counts update automatically** when you change filters and re-run the scout

### ⬡ Field Intelligence AI
An embedded AI hunting analyst lives in the right-side drawer. It has full context of your current session — active filters, sighting counts, top-ranked zones with coordinates — and answers tactical questions about your specific data.

**Supported providers:**

| Provider | Model | Cost | Get Key |
|---|---|---|---|
| **Groq** | Llama 3.3 70B | **Free tier** | [console.groq.com](https://console.groq.com) |
| Claude | claude-haiku-4-5 | Paid | [console.anthropic.com](https://console.anthropic.com) |
| OpenAI | GPT-5.4 Mini | Paid | [platform.openai.com](https://platform.openai.com/api-keys) |
| Grok (xAI) | grok-4.3 | Paid | [console.x.ai](https://console.x.ai) |

> **Free path:** Create a Groq account (email only, no credit card), generate an API key, paste it in. Under two minutes.

API keys are stored in browser `sessionStorage` only — never transmitted anywhere except directly to the provider you select. Keys are cleared when you close the tab.

The AI analyst knows:
- Exact scoring weights for all five factors
- Colorado elk behavior by season (pre-rut through late season)
- Elevation migration patterns across the state
- Tactical principles — north-facing aspects, saddle corridors, west slope vs. east slope
- iNaturalist data bias and how to account for it
- Your live session state: current filters, zone scores, coordinates, and peak months

Quick-prompt buttons cover the most common questions; the chat supports full multi-turn conversation.

### Mission Parameters (Filters)
- **Season / Month Range** — Archery/Early Rifle (Aug–Oct), Rut Peak (Sep–Nov), Late Season (Nov–Jan), Summer Scouting (Jun–Aug), or Full Year
- **Years of Data** — Last 2, 5, or 10 years
- **Road Pressure Weight** — Ignore, moderate, or high weighting for backcountry penalty
- **Elevation Zone** — Filter to Low (<8,000 ft), Mid (8,000–10,500 ft), High (>10,500 ft), or All

### Hot Zones Panel
- Ranked list of top 25 scoring zones
- Click any zone card to fly the map to that location — smooth animated fly-to with status update
- Each card shows: composite score, sighting count, elevation band, peak activity month

### Mission Brief
After each scout run, a tactical summary panel generates automatically below Signal Summary:
- Top score, prime zone count, hot zone count, lead terrain type, peak month, lead coordinates
- Tactical recommendation that adjusts based on signal strength — tells you what to do, not just what the numbers mean
- Updates on every re-scan with new filters

### Score Breakdown in Popups
Click any zone rectangle on the map to see the full five-factor breakdown — Density/Season/Elevation/Cover/Pressure shown individually, with proxy factors labeled as estimates.

### UI & Map Polish (v1.1)
- **Dynamic scan grid** — a subtle tactical grid rendered via `L.GridLayer`, moves and scales with the map as you zoom and pan
- **Map vignette** — soft radial darkening at map edges for field-console depth
- **Layer toggle preserves view** — switching GRID/HEAT/BOTH no longer resets the map to full Colorado
- **Glass/depth panel styling** — gradient panels, depth shadows, hover animations on zone cards
- **Collapsible panels** — sidebar collapses to icon rail via `‹/›` tab; header controls collapse via `⊟/⊞` button; FIELD AI always accessible
- **Mobile responsive** — sidebar starts collapsed on mobile, AI drawer goes full-width, touch-friendly tap targets

---

## How It Works

### Architecture

ELK SCOUT is a single-file HTML application — no build step, no backend, no database. Open the file in any modern browser and it runs. All computation happens client-side.

```
index.html
├── CSS         — Dark tactical UI, CSS variables, responsive layout
├── Leaflet     — Map rendering (OSM tiles, GeoJSON, heatmap)
├── JavaScript
│   ├── iNaturalist fetch loop     — Paginated API calls with rate limiting
│   ├── Client-side filters        — Month, elevation applied post-fetch
│   ├── Grid cell builder          — 0.25° cells, sighting aggregation
│   ├── Five-factor scorer         — Density + season + elevation + cover + pressure
│   ├── renderMap()                — Grid rectangles + heat layer + sighting dots
│   ├── renderMissionBrief()       — Tactical summary with signal-strength recommendation
│   ├── GMU loader                 — Lazy fetch, L.layerGroup wrapper, label markers
│   ├── refreshGMUCounts()         — Rebuilds GMU counts after each new scout run
│   ├── Point-in-polygon           — Ray casting for GMU sighting counts
│   ├── ScanGrid (L.GridLayer)     — Dynamic canvas grid, moves with map zoom/pan
│   ├── setBaseLayer()             — Swaps MAP/SAT tile layers, toggles dark filter
│   └── Field Intelligence AI      — Multi-provider BYOK chat with live session context
└── External dependencies (CDN)
    ├── leaflet@1.9.4
    ├── leaflet.heat@0.2.0
    ├── IBM Plex Mono / IBM Plex Sans / Bebas Neue (Google Fonts)
    └── OpenStreetMap tiles
```

### Data Flow

```
User clicks RUN SCOUT
        │
        ▼
iNaturalist API (paginated, 200/page, up to 8 pages)
  → research-grade observations only
  → Colorado bounding box
  → date range based on years filter
        │
        ▼
Client-side filter pass
  → month filter (season selection)
  → elevation zone filter
        │
        ▼
buildGridCells()
  → bin sightings into 0.25° grid
  → aggregate by month distribution
  → score each cell (5-factor model)
  → sort by score descending
        │
        ▼
renderMap()            →  GRID / HEAT / BOTH layer (view preserved on layer toggle)
renderStats()          →  summary panel
renderMissionBrief()   →  tactical brief with recommendation
renderZoneList()       →  ranked sidebar
refreshGMUCounts()     →  rebuild GMU overlay if active
        │
        ▼
[Optional] GMU UNITS button
  → fetch CPW boundary GeoJSON (Hub API v3)
  → countSightingsPerGMU() (ray casting)
  → L.geoJSON + label markers → L.layerGroup

[Optional] FIELD AI button
  → select provider (Groq / Claude / OpenAI / Grok)
  → enter API key (session only)
  → chat with full live session context injected
```

---

## Data Sources

| Source | Data | License |
|---|---|---|
| iNaturalist API | Research-grade elk observations with GPS coordinates, dates, observer info | CC BY-NC (observations) |
| CPW Open Data / ArcGIS Hub | Game Management Unit boundaries (Big Game) | Public domain — Colorado state government |
| OpenStreetMap | Street/topo base map tiles | ODbL |
| ESRI World Imagery | Satellite base map tiles | Esri, Maxar, Earthstar Geographics |
| USGS National Map | Topographic base map tiles | U.S. Geological Survey |

### API Details

**iNaturalist endpoint:**
```
GET https://api.inaturalist.org/v1/observations
  ?taxon_id=204114         ← Cervus canadensis (elk/wapiti)
  &quality_grade=research  ← verified observations only
  &geo=true                ← GPS coordinates required
  &nelat=41.0&nelng=-102.04&swlat=36.99&swlng=-109.06  ← Colorado bbox
  &d1={startDate}&d2={endDate}
  &per_page=200&page={n}
```

**CPW GMU endpoint:**
```
GET https://opendata.arcgis.com/api/v3/datasets/
    168fccb0583f42f1afe57de6c9ce846d_6/downloads/data
  ?format=geojson
  &spatialRefId=4326
```
Falls back to the Colorado GeoLibrary dataset if the primary is unavailable.

---

## Running Locally

No installation required.

```bash
git clone https://github.com/jkh2/elk-scout.git
cd elk-scout
open index.html   # macOS
# or
start index.html  # Windows
# or just drag the file into your browser
```

The app makes live API calls to iNaturalist on load. An internet connection is required.

---

## Limitations & Honest Notes

**iNaturalist data is citizen science.** Sightings come from hikers, photographers, and naturalists — not hunters. Distribution patterns are real but skewed toward areas with foot traffic. Remote roadless wilderness may be underrepresented despite holding elk.

**Elevation, cover, and road pressure are proxies.** The scoring model uses geographic heuristics — not actual DEM elevation data or USFS road layers. These upgrade paths are planned.

**Page cap at 1,600 sightings.** The app fetches up to 8 pages × 200 results. With 10 years of data selected, total available sightings may exceed this. A warning is shown when the cap is reached.

**GMU sighting counts use ray casting.** Point-in-polygon computation runs in the browser's main thread. With large sighting counts this may take a second on slower devices.

**Research-grade only.** The iNaturalist API is filtered to `quality_grade=research` — observations that have been community-verified. Casual or unverified sightings are excluded.

**AI Field Intelligence requires your own API key.** The AI chat is BYOK — bring your own key. Groq offers a free tier with no credit card required. Keys are never stored beyond your browser session.

**Not legal hunting advice.** ELK SCOUT shows where elk have been observed — it does not guarantee elk presence, land access, or legal hunting permission. Always verify current CPW regulations, season dates, bag limits, and land ownership before hunting. Trespassing and out-of-season harvest are your responsibility to avoid.

---

## Roadmap

**Shipped in v1.3:**
- [x] USGS topographic base layer (TOPO button, MAP/SAT/TOPO toggle)

**Shipped in v1.2:**
- [x] Satellite imagery base layer (ESRI World Imagery, MAP/SAT toggle)

**Shipped in v1.1:**
- [x] Mobile-friendly responsive layout with collapsible panels
- [x] Mission Brief tactical summary panel
- [x] Score factor breakdown in zone popups
- [x] Dynamic scan grid (L.GridLayer, moves with map)
- [x] Layer toggle preserves map view
- [x] Glass/depth UI polish
- [x] FIELD AI always accessible (outside collapsible header)

**Planned:**
- [ ] Real elevation data in scoring model via OpenTopoData / USGS 3DEP API (topo tiles added in v1.3; scoring model elevation upgrade is next)
- [ ] USFS road layer for true pressure-distance scoring
- [ ] User-submitted sightings as a second data flywheel
- [ ] Species toggle (mule deer, pronghorn)
- [ ] Multi-state support
- [ ] Offline mode with cached sighting data
- [ ] 3D terrain mode via MapLibre GL (v2 map engine)

---

## License

Copyright © 2026 James Keith Harwood II / Sentinel AI Systems
Contact: jameskharwood2@gmail.com
GitHub: github.com/jkh2

This software is licensed under the **Sentinel Source Available License v1.0** (see LICENSE).

**You are free to:**
- Use this software for personal, non-commercial scouting and hunting
- Study and modify the code for personal use
- Share the unmodified source with attribution

**You may not:**
- Sell, sublicense, or commercially distribute this software or any derivative
- Incorporate this software into a paid product or service without a commercial license
- Remove or alter copyright notices or license terms

For commercial licensing inquiries, contact: jameskharwood2@gmail.com

---

## About

Built by James Keith Harwood II and Claude Sentinel (Anthropic) under the SIDLF framework — a research initiative exploring symbiotic human-AI partnership.

ELK SCOUT is part of a broader portfolio of field intelligence tools developed under Sentinel AI Systems.

*"The best hunters don't just walk with a rifle. They scout with data."*
