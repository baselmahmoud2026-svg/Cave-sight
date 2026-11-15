# Cave-sight
**CAVE-SIGHT ** is an open-source, multi-sensor planetary screening platform built for the **2026 NASA Space Apps Challenge** (*"Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars"*). It fuses open NASA Earth and planetary datasets to discover, score, and rank terrestrial basaltic lava tubes and collapse skylights as natural subsurface shelters for permanent astronaut habitats on the Moon and Mars.

- **Live Web Platform:** https://cave-sight.vercel.app/
- **Challenge Track:** Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars

---

## 1. Core Scientific Synthesis & Analog Limits

Permanent human bases on the Moon and Mars require natural subsurface shielding against cosmic radiation, micrometeorite impacts, and extreme diurnal temperature swings. CAVE-SIGHT is anchored on a geometric cross-calibration linking two independent 2024 peer-reviewed discoveries that converged on the exact same subsurface conduit width:

1. **Lunar Benchmark — Carrer & Bruzzone (2024):** Analyzed NASA Lunar Reconnaissance Orbiter (LRO) Mini-RF radar data to reveal an accessible subsurface volcanic conduit **45 meters wide** beneath the **Mare Tranquillitatis Pit** (`8.3355°N, 33.2220°E`).
2. **Terrestrial Calibration Anchor — Stewart et al. (2024):** Surveyed the **Umm Jirsan Lava Tube** in Harrat Khaybar, Saudi Arabia (`25.5888°N, 39.7570°E`; total length `1,481 m`), documenting accessible basaltic passages reaching **45 meters wide**.

### Cave-Geometry Match Matrix (Explicit Analog Boundaries)
In alignment with NASA's analog evaluation guidelines, CAVE-SIGHT explicitly documents both physical matches and planetary scaling boundaries:
- **What Matches:** Subsurface conduit width (`45 m`) and mafic basaltic rock composition.
- **What Differs:** Overburden roof depth (`130–170 m` on the Moon vs. shallow terrestrial roofs) and gravitational regime—under lunar gravity (`1.62 m/s²`, roughly 1/6th of Earth's `9.81 m/s²`), a `45 m` lunar basalt roof span scales structurally to approximately `7.4 m` on Earth.

---

## 2. Three-Target Planetary Screening Architecture

CAVE-SIGHT evaluates terrestrial locations simultaneously against **three extraterrestrial reference benchmarks**:

1. **Mare Tranquillitatis Pit (Moon):** Primary lunar mare basalt and `45 m` subsurface conduit benchmark (`8.3355°N, 33.2220°E`).
2. **Lunar South Pole (Moon):** Polar and cold-context lunar benchmark paired with a sub-zero (`< 0°C`) cold-context check for extreme cold-dry terrestrial analogs (e.g., *Haughton Crater* and *McMurdo Dry Valleys*).
3. **Arsia Mons Skylights (Mars):** Martian volcanic collapse pit benchmark evaluated under Martian gravity (`3.71 m/s²`).

---

## 3. Key Platform Features

- **Three-World Split Geospatial Explorer:** Pairs an interactive terrestrial map (**Esri World Imagery** satellite tiles & **Esri Topographic** relief tiles via `Leaflet.js`) with a synchronized planetary target map integrated with **NASA Moon Trek** (`https://trek.nasa.gov/moon/`) and **NASA Mars Trek** (`https://trek.nasa.gov/mars/`). Clicking any terrestrial marker automatically flies the planetary viewer to its highest-matching extraterrestrial target.
- **Global Registry & 60-Point Discovery Atlas:** Evaluates **16 terrestrial registry sites** (an 8-site blind-test cohort—1 calibration anchor at *Umm Jirsan*, 5 global volcanic candidates, and 2 non-volcanic quartz sand controls—plus 8 established benchmark analogs including *Death Valley*, *Haughton Crater*, and *McMurdo Dry Valleys*) alongside **60 exploratory discovery scan points** across 10 volcanic regions (`cavesight_discovery_scan.csv`).
- **Toggleable Multi-Sensor Heatmaps (`Leaflet.heat`):** Visualizes spatial intensity across **Analog Similarity**, **Basalt Regolith Darkness**, and **Diurnal Thermal Swing**.
- **5-Axis Polar Radar Comparator (`Chart.js`):** Compares any selected terrestrial site against planetary benchmarks across **Basalt Regolith Darkness**, **Seasonal Albedo Stability**, **Hyper-Aridity**, **Topographic Rim Relief**, and **Solar Irradiance Flux**, accompanied by automated physical-driver diagnostics.
- **Live Coordinate Screening Simulator:** Interactive sandbox allowing mission planners to adjust bare-earth albedo, local DEM relief, diurnal temperature swing, and precipitation to compute real-time analog similarity scores.

---

## 4. Unbiased White-Box Methodology & Statistical Audits

To prevent confirmation bias, label leakage, or black-box opacity, CAVE-SIGHT executes a transparent statistical pipeline:

1. **Blind Coordinate Ingestion:** The pipeline ingests raw coordinates `(site_id, name, lat, lon)` with zero geological role labels (`Calibration_Anchor`, `Volcanic_Candidate`, `Negative_Control`) and zero manual feature weights exposed during scoring.
2. **Remote-Sensing Hygiene (Ocean-Pixel & Snow-Bias Filtering):**
   - Uses inland continental pixels to avoid $0.5^\circ \times 0.625^\circ$ (~55 km) ocean-grid contamination.
   - Extracts minimum monthly surface albedo (`Bare_Earth_Albedo`) and 12-month standard deviation (`Albedo_Monthly_SD`) from **NASA POWER** so winter snow cover on high-latitude lava fields (*Craters of the Moon*, *Lava Beds*) does not mimic bright quartz sand.
3. **3×3 SRTM Topographic Morphometry:** Queries a `3 × 3` elevation grid (`0.00135°` / ~150 m spacing) from **NASA SRTM**-backed DEM data to compute local relief ($\Delta Z = Z_{\max} - Z_{\min}$) and rim slope gradients.
4. **Pillar-Balanced Z-Score Euclidean Distance:** Applies `StandardScaler` against planetary reference vectors and computes a **Geometric Mean** across three orthogonal pillars—**Regolith Optical Match**, **Topographic Morphometry**, and **Hyper-Arid Environment**—preventing climate variables from outvoting geology.
5. **Post-Hoc & Monte Carlo Robustness Audits:**
   - Ground-truth classifications are joined strictly *after* ranking. **Umm Jirsan (`SITE_01`)** blindly ranks **#1 (`70.84%` )**, outranking all continental volcanic candidates and isolating non-volcanic quartz sand controls (*Algodones Sand Dunes* and *Simpson Desert*) at the bottom (`Mann-Whitney U` $p < 0.05$, with `0.0%` missing measurements).
   - **Monte Carlo** sensitivity testing confirms top-rank stability under random sensor noise (`P(rank 1) = 100%`) and across `82.8%` of 232 analytical configurations.

---
├── cavesight_final_ranked.csv      # Ranked terrestrial registry dataset (0.0% missing data)
├── cavesight_discovery_scan.csv    # 60-point global volcanic discovery scan dataset
└── README.md                       # Project documentation
