# ISIS RCS Synchrotron — Optics Modeling & EPICS Archiver Bridge

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Three.js](https://img.shields.io/badge/Three.js-WebGL_3D-black?logo=three.js&logoColor=white)](https://threejs.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-Live_Dashboard-FF4B4B?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![EPICS](https://img.shields.io/badge/EPICS-Archiver_REST_API-4A90E2)](https://epics-controls.org/)
[![MAD-X](https://img.shields.io/badge/MAD--X-cpymad_Optics-darkgreen)](https://mad.web.cern.ch/mad/)

> **Interactive 3D particle accelerator visualization, real-time EPICS archiver data pipeline, and accelerator optics backend for the ISIS Rapid Cycling Synchrotron (800 MeV proton synchrotron).**

![ISIS Synchrotron 3D Hero Render](assets/screenshots/isis_synchrotron_3d_hero.png)

---

## Placement Context

This project encompasses key engineering deliverables developed during a **1-week Work Experience Placement** with the **Accelerator Controls / EPICS Archiver Team** at the **STFC ISIS Neutron and Muon Source** ([Rutherford Appleton Laboratory](https://www.isis.stfc.ac.uk/), July 2026).

The work developed client bridges for live EPICS archiver data, optimized search tools for 40,000+ process variables, and implemented a 3D WebGL model of the synchrotron lattice based on MAD-X dipole geometry.

---

## Featured Deliverables & Contributions

### 1. Live EPICS Archiver REST Bridge (`src/optics_gui/io/`)
* **Live Ingestion:** Python client modules connecting to EPICS archiver endpoints (`/glob`, `/getPVStatus`, `/data`) to query machine state parameters across the 10 ms acceleration ramp.
* **Physics & Magnet Ingestion:** Automated retrieval and parsing of trim quadrupole currents, horizontal/vertical corrector dipole currents, BPM geometry, and harmonic tune programmes.
* **Endpoint Verification:** Audited endpoint behaviors against the live archiver, documented real response schemas, and implemented fallback logic for missing channels (e.g. unarchived test-rig signals or absent F9 harmonics).

```python
from optics_gui.io import (
    archiver_fetch_value,
    get_corrector_settings,
    get_harmonic_tunes,
    get_requested_tune,
)

# Pull live corrector settings and tune programme for a cycle time
corrector_data = get_corrector_settings(cycle_time_ms=5.0)
harmonic_tunes = get_harmonic_tunes(cycle_time_ms=5.0)
```

---

### 2. 40,000+ Process Variable Search Engine (`web/pv_search.html`)
* **Bottleneck:** An initial process variable lookup over 40,000+ records took ~5 seconds per keystroke due to unvectorised per-row regex execution.
* **Optimization:** Replaced iterative row matching with vectorised regex extraction in Pandas/C layer, debounced input events, and cached normalized records.
* **Result:** Reduced query time from ~5,000 ms to <1 ms, enabling real-time search across all 40,000+ PVs without UI freeze.

![ISIS PV Search Tool](assets/screenshots/isis_pv_search_live.png)

---

### 3. Interactive 3D WebGL Synchrotron Simulator (`web/isis_synchrotron_3d.html`)
* **Lattice-Accurate Geometry:** Built a Three.js / WebGL 3D model of the synchrotron generated from **real MAD-X lattice dipole bend angles** rather than an idealized circular ring.
* **Features:**
  * Interactive camera orbits and superperiod inspection (Superperiods 0 through 9).
  * Real-time beam path trajectory and animated particles synchronized to the 50 Hz machine cycle.
  * Standalone, zero-install HTML5 application capable of running locally in any modern browser.

![ISIS Synchrotron 3D Dashboard](assets/screenshots/isis_synchrotron_dashboard.png)

---

### 4. Live Streamlit Archiver Bridge (`apps/archiver_live_app.py`)
* **Live EPICS Pipeline & Status Ledger:** Directly integrates the REST bridge (`epics_archiver_client.py` and `epics_live.py`) to stream real telemetry into the optics computation core.
* **Working Tabs & Interactive Diagnostics:**
  * **Architecture & Status:** Visualises real-time data movement from Athena archiver endpoints through to closed-orbit/tune GUIs.
  * **PV Search:** Performs instant regex filtering, magnet family parsing, and CSV exporting across thousands of PV records.
  * **BPM Geometry & Orbit:** Integrates MAD-X TWISS lattice execution across all 36 beam position monitors.

| Tune & Harmonics Readouts (A3) | Corrector Current Ingestion (A1) |
|:---:|:---:|
| ![Tune & Harmonics](assets/screenshots/archiver_tab_tune_live.png) | ![Corrector Settings](assets/screenshots/archiver_tab_correctors_live.png) |

| Architecture & Data Pipeline Ledger | MAD-X TWISS 36-BPM Geometry (A2) |
|:---:|:---:|
| ![Archiver Data Pipeline](assets/screenshots/archiver_tab_status_active.png) | ![BPM Geometry Table](assets/screenshots/archiver_tab_bpm_preview.png) |

---

## Accelerator Optics Architecture

The repository provides a modular backend for modeling the beam dynamics and optics of the ISIS Rapid Cycling Synchrotron:

```text
isis-synchrotron-optics/
├── src/
│   └── optics_gui/
│       ├── io/                    # EPICS Archiver REST client & data ingestion
│       │   ├── epics_archiver_client.py
│       │   ├── epics_live.py
│       │   ├── measurements.py
│       │   └── archives.py
│       ├── cycle_time.py          # Relativistic beam ramp & kinematics (0-10ms)
│       ├── machine_state.py       # Machine state dataclasses & magnetic overrides
│       ├── madx_model.py          # MAD-X / cpymad lattice execution wrapper
│       ├── orbit_correction.py    # Closed-orbit correction (COCU-style algorithm)
│       ├── tune_matching.py       # Tune matching & harmonic quadrupole tuning
│       ├── envelope.py            # Beam envelope & RMS emittance calculation
│       ├── aperture.py            # Mechanical aperture clearance margins
│       └── snapshot.py            # Whole-system machine state snapshot integration
├── web/
│   ├── isis_synchrotron_3d.html   # 3D WebGL Synchrotron simulation (Three.js)
│   └── pv_search.html             # 40,000+ Process Variable search tool
├── apps/
│   └── archiver_live_app.py       # Streamlit live archiver bridge application
├── assets/
│   └── screenshots/               # High-res UI & 3D renders
└── tutorials/                     # Step-by-step interactive Jupyter notebooks
```

---

## Visual Gallery

| 3D Synchrotron Ring | Superperiod Layout |
|:---:|:---:|
| ![3D Ring](assets/screenshots/isis_3d_canvas.png) | ![Superperiod View](assets/screenshots/isis_synchrotron_superperiod_view.png) |

| Live Operator UI | 3D Interactive Canvas |
|:---:|:---:|
| ![Operator UI](assets/screenshots/isis_synchrotron_ui.png) | ![3D Clean View](assets/screenshots/isis_synchrotron_3d_clean.png) |

---

## Quickstart Guide

### 1. Running the 3D WebGL Synchrotron Simulator
No dependencies required — open directly in any browser:
```bash
# Linux
xdg-open web/isis_synchrotron_3d.html

# macOS
open web/isis_synchrotron_3d.html

# Windows
start web/isis_synchrotron_3d.html
```

### 2. Running the 40k+ PV Search Tool
```bash
xdg-open web/pv_search.html
```

### 3. Running the Streamlit Archiver Bridge
```bash
# Create and activate virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
pip install -e .

# Launch the app
streamlit run apps/archiver_live_app.py
```

### 4. Running the Offline Demonstration Suite
Verify all deliverables end-to-end without requiring access to the internal STFC network:
```bash
python3 scripts/demo_run.py
```
This suite automatically validates:
1. **EPICS Archiver REST Ingestion:** Correctors (HD/VD) and tune setpoints ($Q_x = 4.331, Q_y = 3.731$) mapped from authentic ISIS snapshots.
2. **PV Search Engine:** Sub-millisecond regex queries over thousands of process variables.
3. **MAD-X Lattice & Kinematics:** Full relativistic beam ramp calculation ($70 \to 800\text{ MeV}$) and TWISS optics execution across 36 beam position monitors.

---

## Technical Stack
* **Languages & Runtimes:** Python 3.10+, JavaScript (ES6+), HTML5/CSS3
* **Graphics & Visualization:** Three.js, WebGL, Matplotlib, Plotly
* **Scientific Computing:** NumPy, Pandas, cpymad (CERN MAD-X interface)
* **Web & UI Frameworks:** Streamlit
* **Controls Systems:** EPICS (Experimental Physics and Industrial Control System) Archiver Appliance REST API
