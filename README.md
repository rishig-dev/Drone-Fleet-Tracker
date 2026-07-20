# 🛰️ Drone Fleet Tracker — Tactical Ops Dashboard

A real-time, map-based drone fleet monitoring dashboard with a dark tactical UI.
Built with **Dash**, **dash-leaflet**, and **Plotly**, and designed to run directly
in Google Colab with zero external hosting.

![status](https://img.shields.io/badge/status-active-brightgreen)
![python](https://img.shields.io/badge/python-3.10%2B-blue)
![license](https://img.shields.io/badge/license-Proprietary-red)

---

## Features

- **Live tactical map** (dark CartoDB basemap) with animated drone markers rotated
  to true heading
- **Per-drone flight path trails** showing the last N positions
- **Color-coded status rings** — green (nominal) / amber (caution) / red (critical)
  based on battery and link quality
- **Click-to-inspect telemetry panel** — altitude, battery, speed, link quality,
  live heading compass, and lat/lon
- **Fleet roster list** synced with the map, click either to select an asset
- **Simulated real-time telemetry stream**, refreshing every 1.5 seconds —
  designed to be swapped for a real WebSocket / MQTT / MAVLink / ADS-B feed
- Fully self-contained, runs inline inside a Colab notebook

---

## Preview

The dashboard renders inline as an iframe once the notebook is run — a dark
command-center layout with the map on the left and roster + telemetry panel
on the right.

---

## Getting Started

### Option A — Google Colab (recommended)
1. Upload `drone_fleet_tracker.ipynb` to [Google Colab](https://colab.research.google.com/)
2. Run all cells top to bottom (`Runtime → Run all`)
3. The dashboard renders inline at the bottom of the notebook

### Option B — Local machine
```bash
git clone <this-repo>
cd drone-fleet-tracker
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py                 # if you extract the notebook code into app.py
```
Then open the URL Dash prints in your terminal (defaults to `http://127.0.0.1:8050`).

> Note: if running locally, change `app.run(jupyter_mode="inline", ...)` to
> `app.run(debug=True)` since `jupyter_mode` is a Colab/Jupyter-only argument.

---

## Project Structure

```
.
├── drone_fleet_tracker.ipynb   # Main notebook — simulation + Dash app
├── requirements.txt            # Python dependencies
├── .gitignore
├── LICENSE
└── README.md
```

---

## How It Works

- **`Drone` / `Fleet` classes** simulate a squadron of UAS assets patrolling a
  configurable area of interest, with smoothly evolving position, altitude,
  speed, heading, battery, and link quality.
- **`dl.Map`** (dash-leaflet) renders the tactical map, with custom rotated SVG
  `divIcon` markers and `dl.Polyline` flight trails.
- **`dcc.Interval`** drives the simulation tick and UI refresh every 1.5s.
- **Pattern-matching callbacks** (`dash.ALL`) let you click any marker or
  roster row to select a drone and populate the telemetry panel.

To go from simulation to a live system, replace `Fleet.tick()` with a client
that pulls real telemetry (WebSocket, MQTT, MAVLink, ADS-B, or a REST feed) —
the rest of the app requires no changes.

---

## Roadmap / Ideas

- [ ] Real telemetry ingestion (WebSocket/MQTT/MAVLink)
- [ ] Geofencing — draw no-fly zones, flag breaches in red
- [ ] Multi-sector view with a dropdown to switch AOIs/fleets
- [ ] Alerts ticker logging status transitions
- [ ] Persistence layer (SQLite/Postgres) for flight-path playback and
      after-action review

---

## License

This project is **proprietary** and **not open source**. All rights are
reserved by the copyright holder — no permission is granted to use, copy,
modify, or redistribute this code. See [`LICENSE`](./LICENSE) for full terms.

---

## Disclaimer

This is a simulation/demo project intended for portfolio and educational
purposes. It does not connect to, control, or interface with any real
unmanned aerial system, military asset, or airspace authority.
