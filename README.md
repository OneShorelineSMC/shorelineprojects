# San Mateo County – Coastal Flood Hazards Viewer

A one-page web map that reads BCDC's live **RSAP Coastal Flood Hazards** service and shows only San Mateo County. Nothing is clipped or copied in GIS. The page clips the layers in the browser, so the map always reflects BCDC's current data.

## What it does

- Four sea level rise scenarios (0.8 ft / 2050, 3.1 ft, 4.9 ft, 6.6 ft / 2100), one at a time
- Hazards are visible only inside the San Mateo County boundary; the basemap stays visible everywhere
- Popups on click, with no popups outside the county
- Legend, opacity slider, toggles for "Existing Bay" and the county outline
- **Download San Mateo GeoJSON**: queries the current scenario, clips it to the county, adds an acres field, and downloads a `.geojson` you can open in ArcGIS Online, QGIS, or Pro. It also shows an acreage table by hazard type.

## Put it on GitHub Pages (about 5 minutes, no software needed)

1. Sign in at github.com and click **New repository**. Name it, e.g. `san-mateo-flood-hazards`, set it to **Public**, and create it.
2. On the repo page, click **Add file → Upload files**, drag in `index.html` (and this `README.md`), then click **Commit changes**.
3. Go to **Settings → Pages**. Under *Build and deployment*, set **Source** to *Deploy from a branch*, choose branch **main** and folder **/ (root)**, then **Save**.
4. After a minute or two, the site is live at `https://<your-username>.github.io/san-mateo-flood-hazards/`.

To change something later, open `index.html` on GitHub, click the pencil icon, edit, and commit. The site updates in a minute or so.

## How the San Mateo filter works

The BCDC layers have no county field, so the filter is spatial:

- **Map display**: the hazard layers sit in a group with a county-shaped mask set to the `destination-in` blend mode. Only the pixels under the county polygon are drawn.
- **Export**: the page asks BCDC's service for features that intersect the county, clips each one to the exact boundary, then builds the GeoJSON.
- **Boundary**: San Mateo County (FIPS 06081) from the U.S. Census TIGERweb service. This is the legal boundary, so it includes San Mateo's portion of the Bay.

## Customizing

Everything adjustable is in the `CONFIG` block near the top of the `<script>`:

- **Use your own boundary** (a city, planning area, or a different county line): export it as GeoJSON in WGS84, upload it to the repo (e.g. `data/boundary.geojson`), and set `geojsonUrl: "data/boundary.geojson"`.
- **Exclude areas**: the "Clip out Foster City" toggle cuts Foster City's limits out of the map, popups, and export (the export filename gets `_excl_foster_city`). To add another city, copy its entry in `exclusions` and change `id`, `label`, `file`, and the place GEOID (`"06"` + the city's 5-digit Census place code). Set `startOn: true` to have it clipped by default.
- **Another county**: change `where` to that county's FIPS code, e.g. `"GEOID = '06001'"` for Alameda.
- **Basemaps**: change `basemap` / `altBasemap` (e.g. `"topo-vector"`, `"satellite"`, `"osm"`).

## Notes

- Data: BCDC RSAP Coastal Flood Hazards (BCDC, USGS CoSMoS, ART). Planning-level data, not for project design.
- Built with the ArcGIS Maps SDK for JavaScript 4.33, loaded from Esri's CDN. No API key or ArcGIS license is needed for public services.
- The RSAP data is built from Bay shoreline mapping, so check how much of San Mateo's Pacific coast it covers before relying on it there.
