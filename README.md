# National Prescribed Fire Burn Planner (EXPERIMENTAL)

> **Experimental:** this page is under development. Do not use it to make operational decisions.
> If you need assistance with tactical planning, please reach out to your local Predictive Services forecaster.

A free web page, hosted on GitHub Pages, for prescribed fire burn-window planning (smoke dispersion,
fire weather, temperature, humidity, wind and precipitation) anywhere in the United States, built for
Predictive Services at every GACC.

The sidebar has two tabs: **Planner** (the controls) and **Page information** (a detailed guide to every
layer, the ventilation scale, overnight settling, burn-window criteria, overlays, times and limitations).

Live site (once set up): **https://swccfirewx.github.io/prescribed-fire-planner/**

## What it shows

**Forecast (NOAA National Blend of Models, NBM)** on four grids: CONUS (2.5 km), Alaska (3 km,
including the Aleutians across the date line), Hawaii, and Puerto Rico / USVI. Out to **7 days**:
hourly to 48 h, 3-hourly to 4 days, 6-hourly to 7 days (when the NBM run reaches that far).
Smooth, interpolated maps at about 10–14 km.

| Map layer | What it is |
|---|---|
| Ventilation rate | Mixing height × transport wind, in categories (default NWS 40k/60k/100k/150k kt-ft; editable) |
| Burn-window hours (selected day) | Hours meeting *your* criteria, in each location's own local time |
| Meets burn criteria (this hour) | Yes / no at the selected time |
| Confidence: ventilation / wind | Chance ventilation meets your minimum, or surface wind stays under your max, from NBM's ensemble spread |
| Overnight smoke settling risk | Screening index for the night starting that evening (see below) |
| Near-surface smoke / vertically integrated smoke | NOAA HRRR smoke (CONUS, first 48 h) |
| Temperature (hourly), Daily high, Daily low | NBM; daily high/low use each cell's own local midnight–midnight day |
| Chance of lightning, chance of wetting rain (≥ 0.10 in), sky cover | NBM |
| Daily minimum RH, overnight RH recovery (max RH) | NBM, per local day / night |
| Hot-Dry-Windy Index, hourly and daily max | Surface approximation: vapor pressure deficit × 10-m wind, from NBM |
| Mixing height, transport wind, precip chance, 24-h precip, surface wind, gusts, RH | NBM (wind at 10 m, or 20 ft = 10-m × 0.87) |

**Burn-window criteria** (editable, each with an on/off checkbox plus **All on / All off**): ventilation,
max precip chance, max wetting rain chance, max lightning chance, max wind and gust (10 m or 20 ft), min RH,
max RH, min/max temperature, max HRRR smoke, max Hot-Dry-Windy Index, burn hours, and excluding Red Flag
Warning / Fire Weather Watch hours. Unchecked criteria are ignored everywhere.

**ⓘ button** next to *Experimental*: a quick overview of the page's purpose: planning to find the best
prescribed burn windows, not specifics or go/no-go decisions. It opens automatically on a person's first visit.

**Precipitation** runs the full 7 days. Where NBM only provides 6- or 12-hour precipitation periods, the
script uses the shortest period covering each forecast step (amounts spread evenly so totals add up). The run
log prints `precip: PoP for N/N times` to confirm.

**Overlays**
- **NWS Red Flag Warnings and Fire Weather Watches.** Checked live from NWS when the page opens, with a
  cached copy as backup. Alerts in effect at the selected time are drawn bold, others faint. By default,
  hours under a warning or watch are excluded from burn windows.
- **AirNow PM2.5 monitors** (latest hourly value, colored by EPA PM2.5 breakpoints; not the 24-hour AQI)
- **PM2.5 nonattainment areas** (EPA)
- **PSAs colored by ERC percentile** (NIFC ERC/BI percentiles by PSA)
- GACCs, PSAs, current wildfire and prescribed-fire perimeters (NIFC)

**Click any point** for the forecast at that exact spot (blended from the 4 nearest grid points):
- **Burn-day calendar**: days × hours, colored by how many criteria each hour fails; click a cell to jump there
- charts (dispersion with NBM spread; precip, wetting rain and lightning chances with wind; temperature & humidity; HRRR smoke)
- burn windows in local time + UTC + your main time
- **Spot-style forecast text** (copy / download), laid out like a NWS spot forecast and clearly labeled as automated guidance, not a spot forecast
- **wind shifts** (45°+ within ~3 h; "possible front" with a temperature drop or RH jump), on the wind chart and in window flags
- sunrise / sunset

Flags for:
- Red Flag Warnings / Fire Weather Watches at or near the window
- a wind shift during or soon after the window
- smoke drifting toward a PM2.5 nonattainment area within 40 miles (from the window's mean transport wind)
- the PSA's ERC and BI percentiles
- the nearest AirNow monitor

There's a daily table (wetting rain, lightning, sky, high/low temperature, min RH, overnight RH recovery,
Hot-Dry-Windy, overnight settling, sunrise/sunset) and a **CSV download** of the full point forecast.

**Other tools**
- **Last-updated badge** (top of the page): "Updated Mon 9:40 AM MST · 17 min ago". It turns **red and says
  OUT OF DATE** if the data is more than 6 hours old, the NBM run is more than 14 hours old, or the latest
  *Update data* run failed. A failed run publishes nothing, so the page checks GitHub's public Actions API
  (cached 10 minutes per visitor); if that can't be reached, the age checks still apply. Click the badge
  for details and a link to the run.
- **Point panel**: location and key facts, then the potential burn windows (click one to jump to it), then
  tabs: Calendar, Charts, Daily summary, Hourly table, Spot-style text. Charts use brighter colors in dark mode.
- **Sidebar sections** fold open and closed (the page remembers which).
- **Area menu**: Lower 48, any GACC, Alaska, Hawaii, Puerto Rico. Sets that area's time zone.
- **Clocks** (top right), all for the selected forecast time:
  1. **Local time**: the point you clicked on the map, or with no point open, the selected area's or GACC's time zone.
  2. **Your time**: the time zone your own computer or phone is set to.
  3. **UTC**.
- **Experimental notice**: a permanent banner and badge say the page is under development and not for
  operational decisions; the notice also prints and appears at the top of every CSV.
- **Copy link**: the URL captures area, layer, time, clicked point, criteria, ventilation scale and units.
- **Print / PDF**: a landscape page with title, times, criteria and legend (use "Save as PDF" in the print dialog).
- **My burn units**: save units with their own prescriptions; each shows its next window. Export/import
  as a JSON file to share with others; download all units' windows as CSV.
- **Burn unit outlines**: upload KML, KMZ, GeoJSON or a zipped shapefile (projected shapefiles are converted
  using the .prj via proj4js, loaded only when needed). The forecast is sampled across each unit, and a window
  needs every sampled point to meet the criteria. Files are read in the browser and never uploaded.
- **Display**: ventilation in kt-ft, m²/s or mph-ft; wind in mph, kt or m/s; surface wind height 10 m or
  20 ft; °F/°C; ft/m; in/mm.
- **Light / dark mode**: the 🌙 / ☀️ button at the top right switches between them. Under **Display → Theme**
  choose Light, Dark, or Match my device. Dark mode uses a dark basemap; printing is always light.
- **Colorblind-safe colors** and **Lite mode** (less data; on automatically on phones).
- On phones, the controls open from **☰ Controls**.

### Overnight smoke settling risk (this site's screening index)
After sunset a surface (radiation) inversion forms and acts like a lid: smoke can't mix upward, so it
stays near the ground, drifts downslope and pools in valleys and low spots (sometimes with fog) until
mid-morning heating mixes it out. For each night (6 PM–8 AM local), from the NBM forecast:
- **Moderate:** lowest mixing height under 1,500 ft *and* lowest transport wind under 10 kt
- **High:** under 1,000 ft *and* under 6 kt
- **Very high:** High plus RH ≥ 85% (smoke + fog)

This is not LVORI. Lavdas ADI and LVORI can be added once their published lookup tables are
provided (they are only published as chart images).

## How it works
A GitHub Action (`.github/workflows/update-data.yml`) runs every 3 hours. It downloads only the needed
fields from NOAA's public NBM and HRRR data on AWS, plus NWS alerts, AirNow, EPA and NIFC layers. It finds
each map cell's time zone, packages everything with `index.html`, and publishes to GitHub Pages.
Nothing is committed to the repository.

| File | What it does |
|---|---|
| `index.html` | The web page |
| `fetch_data.py` | Downloads and processes all data into `data/` |
| `.github/workflows/update-data.yml` | Runs the script every 3 hours and publishes the site |

---

## Step-by-step setup (about 15 minutes, all in a web browser)

1. **Sign in** at https://github.com as **SWCCFireWx**.
2. **Create a repository:** **+ → New repository**, name `prescribed-fire-planner`, **Public**,
   check **Add a README file** → **Create repository**.
3. **Upload files:** **Add file → Upload files**, drag in `index.html`, `fetch_data.py`, `README.md`
   (the files, not the zip) → **Commit changes**.
4. **Workflow file:** **Add file → Create new file**, name it exactly `.github/workflows/update-data.yml`,
   paste in the contents of `update-data.yml` → **Commit changes**.
5. **Turn on Pages:** **Settings → Pages → Source: GitHub Actions**.
6. **First run:** **Actions → Update data → Run workflow**. Wait for **build** and **deploy** to show
   green checks (about 25–45 minutes the first time).
7. **Open** https://swccfirewx.github.io/prescribed-fire-planner/ and share the link.

**Updating an existing copy:** upload the new `index.html`, `fetch_data.py` and `README.md` over the old
ones, then open `.github/workflows/update-data.yml`, click the pencil, replace everything with the new
version, and commit. The commit starts a new run automatically.

## Renaming the repository (changes the web address)
The site address is always `https://swccfirewx.github.io/<repository name>/`.
1. Open the repository → **Settings** (top row, gear icon).
2. Under **General**, in **Repository name**, type the new name (e.g. `prescribed-fire-planner`) → **Rename**.
3. **Actions → Update data → Run workflow** to republish at the new address (green checks on build and deploy).
4. Open `https://swccfirewx.github.io/<new name>/` and hard refresh (Ctrl+Shift+R / Cmd+Shift+R).

GitHub forwards the old *repository* address, but **not the old site address**: old bookmarks and shared
links stop working, so send everyone the new link. Saved criteria and burn units carry over (they're stored per
`swccfirewx.github.io`, not per repository).

## Checking the first real run
Open the run log (**Actions → the run → build → "Build site and data"**) and look for:
- `precip: PoP for N/N times, … wetting rain for N/N, lightning for N/N, sky cover for N/N`: each should
  match the total. If wetting rain or lightning shows 0, send the line plus the `windows found` and any
  `no thunder records matched; example TSTM line:` line so the match can be adjusted.
- `NBM spread fields found: …`: which spread fields NBM provided. If `none`, the two confidence layers
  will say spread isn't available.
- `features from PSA_ERC_and_BI_Percentiles_and_Trends …; fields: …`: the ERC/BI field names found.
  If the page doesn't color PSAs by ERC, send these lines so the field match can be adjusted.
- `HRRR smoke from …`: the HRRR run used.
- Any line starting with `!!`: that source failed this run. Every source except NBM CONUS is optional,
  and the site still publishes without it.

## Limitations
- NBM and HRRR are guidance. They under-resolve valley inversions and drainage flow; overnight smoke in
  valleys can be worse than shown. **Always get a NWS spot forecast** and follow the burn plan and smoke permit.
- Confidence uses NBM's ensemble standard deviation with a normal approximation.
- HRRR smoke covers CONUS only, about 48 hours.
- AirNow values are the latest hour, not NowCast or 24-hour AQI.
- Default ventilation categories are the common NWS scale; set your state's breakpoints on the page.
- GitHub Pages has a soft limit of about 100 GB of downloads a month (thousands of visits). For heavy
  agency-wide use, host the same files on an agency web server.

## Troubleshooting
| Problem | Fix |
|---|---|
| Site shows 404 | Settings → Pages → Source must be **GitHub Actions**, then run the workflow |
| "No forecast data yet" | Run the workflow and wait for both green checks, then reload |
| Red ✗ on a run | Open the failed step and read the last lines; NOAA is sometimes briefly unavailable. The previous site stays up |
| Header says "not in this run: Alaska" (etc.) | That grid failed this run (log line `!! ak failed`); usually fixes itself |
| Legend says alerts are "as of the last data update" | The live NWS check didn't respond; the cached copy (up to 3 h old) is shown |
| Header shows "⚠ … h old" | Check the Actions tab; rerun **Update data** |
| Run cancelled at 120 minutes | NOAA downloads slow; rerun. If frequent, raise `timeout-minutes` |
| Scheduled runs stopped | GitHub pauses schedules after 60 days with no repo activity: **Actions → Update data → Enable workflow** |

## Data sources
- NOAA National Blend of Models (core; CONUS, Alaska, Hawaii, Puerto Rico), NOAA Open Data on AWS `noaa-nbm-grib2-pds`
- NOAA HRRR smoke (MASSDEN 8 m, COLMD), NOAA Open Data on AWS `noaa-hrrr-bdp-pds`
- NWS watches/warnings map service (Red Flag Warnings, Fire Weather Watches)
- AirNow public hourly data files (files.airnowtech.org)
- EPA Nonattainment Areas (PM2.5 24-hr 2006, annual 2012)
- NIFC: WFIGS current perimeters, GACC boundaries, PSA boundaries, ERC/BI percentiles by PSA
- Time zones: timezone-boundary-builder via the `timezonefinder` Python package
