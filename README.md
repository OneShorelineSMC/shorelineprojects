# OneShoreline Flood Adaptation Viewer (Beta)

A one-page web map of coastal flood hazards and adaptation projects in San Mateo County. It reads regionally published data live from its sources and clips it to the county in the browser, so nothing has to be clipped or copied in GIS.

**Beta, for exploration only.** The viewer brings regionally published data together in one place so users can explore data coverage and quality to support adaptation planning. It is not an official flood map and does not replace the source data or site-specific analysis. Questions or corrections: info@oneshoreline.org

## What's in the viewer

**Flood hazards (BCDC RSAP Coastal Flood Hazards, live)**
- Four sea level rise scenarios: 0.8 ft (2050), 3.1 ft, 4.9 ft and 6.6 ft (2100). Opens on 4.9 ft (2100 Intermediate-High).
- Layer toggles: BCDC's combined hazard map, tidal inundation (MHHW), 100-year storm surge, and groundwater (emergent and 0–3, 3–6, 6–9 ft below ground).
- Storm surge overlaps the MHHW areas rather than adding to them, so it draws underneath tidal inundation and groundwater.

**FEMA flood zones (National Flood Hazard Layer, live)**
- 100-year (1% annual chance) and 500-year (0.2% annual chance) floodplains.
- "Land only" option (on by default) hides FEMA zones over the Bay, using BCDC's "Existing Bay" polygon as the shoreline.

**Regional priorities (BCDC RSAP Strategic Regional Priorities, live)**
- Transportation and transit: airports, ferry terminals, freight and passenger rail, highways, seaports, and Bay Plan priority use areas.
- Critical infrastructure: wastewater facilities, power plants, healthcare facilities, emergency operations centers, and the Water-Related Industry priority use area.
- OneShoreline layer (ArcGIS Online item `defe3431d7644a8bb0c676d937f3aaeb`), drawn in bright green. The item must be shared publicly to appear.

**EcoAtlas projects:** see the section below.

**Map tools**
- Clip-out toggles for Foster City (Census city limits) and SFO (Caltrans airport footprint).
- Basemaps: light gray, gray shaded relief with place names, and satellite imagery.
- Legend in the top-right corner that updates with whatever is switched on.
- Export: downloads the current hazard scenario as GeoJSON, clipped to the county (and any clip-outs), with acreage by hazard type.

## EcoAtlas projects

### Source
Project boundaries come from SFEI's EcoAtlas **California Habitat Projects** dataset, downloaded on **October 29, 2026**. They are stored in this repo as `ecoatlas_san_mateo.geojson` rather than read live, because the download is the only form of the data we could filter and tag. EcoAtlas updates daily, so this is a snapshot.

### How the San Mateo set was built
- The statewide download (4,747 project sites) has no county field, so each site was assigned to a county by its location.
- **123 projects** (174 sites) have at least one site in San Mateo County. Sites within 1 km of a county line were checked by hand. Three projects that straddle the line but sit mostly in a neighboring county were left out: South Bay Salt Ponds Cargill Acquisition, San Francisquito Creek Flood Reduction (SF Bay to Highway 101), and South San Francisco Bay Shoreline Project Phase II.
- Site outlines were lightly simplified (about 2 m) to keep the file small.

### Coastside vs. Bayside
Each project is assigned by which side of the Skyline ridge (the Bay–Pacific drainage divide) it sits on, using an approximate ridge line. Pacifica, Half Moon Bay, Pescadero, Gazos Creek and Año Nuevo are Coastside. San Andreas Reservoir, Webb Ranch and the Colma Creek projects are Bayside.

### Public/Private and OneShoreline tags: preliminary
**These tags are a preliminary staff review by OneShoreline (October 31, 2026). They have not been confirmed with project proponents and should be treated as draft.**

- Each of the 123 projects was reviewed one at a time against its EcoAtlas contacts and project group.
- **Public / Private:** based on the lead proponent. Government agencies, special districts and public universities are Public. Private developers, landowners, companies, investor-owned utilities and nonprofit organizations (such as Peninsula Open Space Trust and Save The Bay) are Private.
- **OneShoreline-related:** projects OneShoreline leads, partners on, or tracks as related to its work. Five projects list OneShoreline staff as EcoAtlas contacts; the rest were identified by staff judgment.
- The full list of tags, including completed projects, is in `ecoatlas_project_tags.csv`.

| | Public | Private | Total |
|---|---|---|---|
| Coastside | 30 (3 OneShoreline-related) | 4 | 34 |
| Bayside | 67 (19 OneShoreline-related) | 22 (1 OneShoreline-related) | 89 |
| **All projects** | **97** | **26** | **123** |

### What the viewer shows
- **Completed projects (53) are hidden.** The viewer shows the **70 active projects** (planning, proposed, permitting, in progress or unknown status). To show completed projects too, change `hideStatuses: ["Completed"]` to `hideStatuses: []` in `index.html`.
- The project list sits at the bottom of the sidebar in a collapsible section, grouped **Coastside / Bayside → Public / Private → OneShoreline-related / Not OneShoreline**. Every group has Show all / Hide all, and each project has its own checkbox and a ⌖ button that zooms to it.
- Clicking a project on the map shows its details, with **Cost** first. The EcoAtlas download has no cost field, so Cost currently reads "Not reported in EcoAtlas" for every project.

### Updating the project data
1. Download a fresh Habitat Projects shapefile from EcoAtlas.
2. Re-run the San Mateo filter, Coastside/Bayside assignment and tags (new projects will need tagging).
3. Replace `ecoatlas_san_mateo.geojson` in the repo.
4. In `index.html`, change `version:` in the `projects` settings to the new date, so browsers download the new file instead of reusing an old copy.

## Files in this repo
| File | What it is |
|---|---|
| `index.html` | The whole viewer. Adjustable settings are in the `CONFIG` block near the top of the script. |
| `ecoatlas_san_mateo.geojson` | San Mateo EcoAtlas project boundaries with Region, Public/Private and OneShoreline tags. |
| `ecoatlas_project_tags.csv` | Record of the preliminary tags for all 123 projects (optional; the viewer doesn't read it). |
| `README.md` | This file. |

## Publishing with GitHub Pages
The site is published from the `main` branch, root folder (Settings → Pages). After uploading changed files, wait for the green check on the Actions tab (usually 1–2 minutes), then hard-refresh the page (Ctrl+Shift+R, or Cmd+Shift+R on a Mac).

## Data sources
- BCDC RSAP Coastal Flood Hazards (BCDC, USGS CoSMoS, ART), 2024 California Sea Level Rise Guidance scenarios
- BCDC RSAP Strategic Regional Priorities: Transportation and Transit; Critical Infrastructure
- FEMA National Flood Hazard Layer (effective FIRM)
- SFEI EcoAtlas, California Habitat Projects (download of October 29, 2026)
- OneShoreline ArcGIS Online layer
- U.S. Census TIGERweb (county and city boundaries); Caltrans airports (SFO footprint)
- Esri basemaps (Light Gray Canvas, World Hillshade, World Imagery)

Planning-level data, not for project design. Built with the ArcGIS Maps SDK for JavaScript 4.33.
