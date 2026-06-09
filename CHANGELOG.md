# AROME + ECMWF + ICON + GFS Wind Analysis — Changelog

## Version 1.3 — June 2026

Tag: `v1.3` · revert with `git checkout v1.3` (or `git reset --hard v1.3`).

A large round of upgrades turning the app from a 3-model wind tool into a full
multi-model wind + sounding workbench. Still a single self-contained `index.html`,
no build step, auto-deployed to <https://weather.wvsailing.co.uk> via Vercel.

### Models & data
- **ICON added** as a third surface model (`icon_seamless`, DWD) alongside AROME
  (Météo-France 1.5 km) and ECMWF IFS. All three are fetched up front per location.
- **Model toggle** (AROME / ECMWF / ICON) drives the hourly tables, location
  summaries, wind-speed comparison and vertical profile, each using that model's
  own forecast heights. PBL height stays GFS-only.
- **Model checkboxes** in the green box: tick which models to fetch/use (all on by
  default). Unticked models are skipped (faster); after an analysis, models with no
  data at your points are greyed out. GFS is always on (PBL + Skew-T source).

### Model Comparison pane
- New pane comparing all models over the full 48 h for one location:
  **10 m wind speed**, **100 m wind speed** (ICON interpolated 80↔120 m), and
  **10 m wind direction**, with a per-location selector.
- **High-res / regional models added** (shown where they have coverage):
  - **DMI Harmonie-AROME** (2 km, NW/N Europe, 10 m + 100 m)
  - **ItaliaMeteo ICON-2I** (2 km, Italy + W Med, 10 m)
  - **ARPEGE Europe** (~11 km, all-Europe incl. Spain, 10 m)
  - *(meteoapi.eu was evaluated but its free tier is 10 m only and Alps/Balkans-
    focused; no free high-res model exists for Spain.)*

### Atmospheric Sounding — Skew-T Log-P (new)
- Custom D3-built **Skew-T log-P** diagram, axis capped at **500 hPa**, with skewed
  isotherms, isobars, light dry adiabats, and wind barbs (kt).
- **Source selector** GFS / ICON / ECMWF (default **GFS**, densest at 25 hPa → 21
  levels; ICON 10, ECMWF 6). Dew point derived from temperature + RH.
- **Interactive crosshair**: hover shows a guide line, temperature/dew-point markers,
  and a right-edge readout box (pressure, height, T, Td, wind, surface value).
- **Lifted parcel** (green dashed): anchored at the cursor — dry adiabat down to the
  surface, moist/pseudo-adiabat up from the cursor.
- **Surface line** (black): the temperature-line (isotherm) projection to the surface.
- **Convective indices** under the chart: **TCON** (convective temperature),
  **CCL** (convective condensation level), **LCL** (lifting condensation level).
- **Location-picker map** beside the diagram: auto-zooms to the 3 analysis points;
  click to pick a 4th point → its sounding is fetched and added to the dropdown as
  **"Selected sounding position"** while the 3 analysis soundings stay available.

### Interaction
- **All graphs dynamically zoomable**: Plotly charts get mouse-wheel zoom (plus the
  existing drag-box zoom / toolbar); the Skew-T gets d3 zoom + pan with double-click
  to reset.

### Tooling / workflow
- Set up the repo locally and a Cowork → GitHub push workflow (fine-grained PAT);
  every change above was committed and pushed to `main` (Vercel auto-deploys).

---

*Earlier baseline: V1.2 (smart AROME→ECMWF fallback, theoretical log profile,
time-selectable vertical profile).*
