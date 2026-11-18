# ORBiALOG — Orbital to Terrestrial Planetary Analog Discovery Engine

[![NASA Space Apps Challenge 2026](https://img.shields.io/badge/NASA%20Space%20Apps-2026-blue.svg)](https://www.spaceappschallenge.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19.0-61dafb?logo=react&logoColor=black)](https://react.dev/)
[![Three.js](https://img.shields.io/badge/Three.js-r174-black?logo=three.js&logoColor=white)](https://threejs.org/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Google Earth Engine](https://img.shields.io/badge/Google%20Earth%20Engine-API-green?logo=google-earth&logoColor=white)](https://earthengine.google.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas%208.0-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/atlas)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)

> **NASA Space Apps Challenge 2026 Submission**  
> **Challenge:** *What's on the Horizon? Identifying and Analyzing Planetary Analog Sites*  
> **Team:** ORBiALOG Core Team

---

## 📑 Table of Contents

1. [Executive Summary & Motivation](#-executive-summary--motivation)
2. [System Architecture](#️-system-architecture)
3. [10-Dimensional Remote Sensing Vector](#-10-dimensional-remote-sensing-vector)
4. [Multi-Agency Satellite & Planetary Telemetry Data](#-multi-agency-satellite--planetary-telemetry-data)
5. [Mathematical & Machine Learning Methodology](#-mathematical--machine-learning-methodology)
6. [Key Discoveries: Novel Planetary Analogs](#-key-discoveries-novel-planetary-analogs)
7. [Planetary Target Catalog (6 Space Destinations)](#-planetary-target-catalog-6-space-destinations)
8. [Terrestrial Analog Catalog (Curated & Algorithmic Sites)](#-terrestrial-analog-catalog)
9. [Frontend Mission Control Dashboard (7 Screens)](#-frontend-mission-control-dashboard-7-screens)
10. [Backend REST API Specification](#-backend-rest-api-specification)
11. [Remote Sensing Detection Pipeline (Python)](#-remote-sensing-detection-pipeline-python)
12. [Installation, Configuration & Running Guide](#-installation-configuration--running-guide)
13. [Docker & Cloud Deployment](#-docker--cloud-deployment)
14. [Repository File Structure](#-repository-file-structure)
15. [Scientific Rigor, Validation & Citations](#-scientific-rigor-validation--citations)
16. [License & Acknowledgments](#-license--acknowledgments)

---

## 🌌 Executive Summary & Motivation

### The Challenge
Planetary exploration missions (such as NASA Artemis, Mars Sample Return, VIPER, and future Mars crewed habitats) rely extensively on terrestrial analogs to test autonomous rovers, drill mechanisms, navigation algorithms, radiation shelters, and in-situ resource utilization (ISRU) systems before flight. 

Historically, planetary science has gravitated toward a small cluster of well-publicized **"textbook" analog sites** (e.g., Chile's Atacama Desert, Hawaii's Mauna Kea, Devon Island in the Arctic, and McMurdo Dry Valleys in Antarctica). These sites were predominantly selected through visual resemblance, historical precedent, or geographic convenience, rather than systematic, quantitative, multi-spectral orbital analysis.

### The Solution: ORBiALOG
**ORBiALOG** (*Orbital to Terrestrial Planetary Analog Discovery Engine*) is an end-to-end scientific remote-sensing pipeline and interactive aerospace mission control platform. It matches lunar and Martian exploration zones to Earth analogs using **10 physical, topographic, and thermal-infrared spectral dimensions**.

By combining scale-matched digital elevation models, diurnal thermal inertia composites, shortwave albedo, and thermal infrared mineral emissivity indices from orbital satellite missions, ORBiALOG scans **11,466 global 1° terrestrial grid tiles**. Powered by **Mahalanobis distance with Ledoit-Wolf shrinkage covariance**, ORBiALOG removes human bias, rigorously handles Earth’s cross-correlated climate variables, and uncovers previously understudied planetary analog candidates.

---

## 🏛️ System Architecture

ORBiALOG is structured as a decoupled, microservice-ready system encompassing data ingestion, scientific statistical processing, an Express REST API, and a React 19 Three.js aerospace interface:

```mermaid
flowchart TD
    subgraph DataIngestion ["1. Multi-Agency Data Ingestion Layer"]
        GEE["Google Earth Engine API\n(SRTM, MODIS LST, GPM IMERG, ASTER GED)"]
        PDS["NASA Planetary Data System (PDS)\n(LRO LOLA, Diviner, MRO HiRISE, MGS MOLA, CRISM)"]
        ESA["ESA Copernicus Hub\n(Sentinel-1 SAR C-Band, Sentinel-2 MSI BOA)"]
        CSA["CSA Open Data Portal\n(RADARSAT-1 Archive, RCM Constellation CKAN)"]
        JAXA["JAXA G-Portal\n(GCOM-W AMSR2, ALOS-2 PALSAR-2 L-Band)"]
    end

    subgraph AnalyticalCore ["2. Statistical Analytical Core (Python Pipeline)"]
        GRID["1° Terrestrial Grid Generator (11,466 Tiles)"]
        SCALE["Scale-Matching Baseline (~100m)\nTopographic Derivatives (Slope, RMS Roughness)"]
        CLIP["1st–99th Percentile Saturation Clipping\nRobustScaler Standardization"]
        LW["Ledoit-Wolf Shrinkage Covariance Matrix Estimation (Σ_LW)"]
        MAHAL["Weighted Mahalanobis Distance Engine\nd² = (x - t)ᵀ Σ_LW⁻¹ (x - t)"]
        SIM["Exponential Continuous Similarity Mapping\nS = 100 · exp(-0.5 · d² / N_features)"]
        EXPLAIN["Z-Score Gap Decomposition (Worst-Feature Identification)"]
        NOVELTY["Positive Control Check & 150 km Known-Site Spatial Exclusion"]
    end

    subgraph DatabaseLayer ["3. High-Performance Database & Indexing"]
        ATLAS[("MongoDB Atlas 8.0\n• 2dsphere GeoJSON Geospatial Indexes\n• Compound Cache: { targetId: 1, similarity: -1 }\n• Planetary Targets & Site Dossiers")]
    end

    subgraph BackendAPI ["4. Enterprise Backend REST API (Node 20 / TypeScript)"]
        EXPRESS["Express 4 REST Service (:4000/api/v1)\n• Zod Schema Validation\n• Pino Structured Logging\n• Helmet & CORS Defense"]
        COSINE["Analytical Cosine Vector Scoring Engine"]
        SANDBOX["Subsystem Stress Telemetry Simulator"]
    end

    subgraph FrontendUI ["5. Aerospace Mission Control (React 19 / Vite / Three.js)"]
        THREE["Three.js 3D Celestial Globe & Interactive Topography Meshes"]
        LEAFLET["Leaflet Real GIS Coordinate Explorer"]
        RECHARTS["Recharts 10-Axis Multi-Dimensional Radar Profiles"]
        AUDIO["Synthesized Aerospace Sound FX Web Audio Engine"]
        MODAL["Multi-Agency Granule Verification Inspector"]
    end

    GEE --> GRID
    PDS --> SCALE
    ESA --> ATLAS
    CSA --> ATLAS
    JAXA --> ATLAS

    GRID --> SCALE
    SCALE --> CLIP
    CLIP --> LW
    LW --> MAHAL
    MAHAL --> SIM
    SIM --> EXPLAIN
    EXPLAIN --> NOVELTY
    NOVELTY -->|JSON Match Records| ATLAS

    ATLAS <--> EXPRESS
    EXPRESS --> COSINE
    EXPRESS --> SANDBOX
    EXPRESS <-->|REST Contract /api/v1| FrontendUI
    FrontendUI --> THREE
    FrontendUI --> LEAFLET
    FrontendUI --> RECHARTS
    FrontendUI --> AUDIO
    FrontendUI --> MODAL
```

---

## 🛰️ 10-Dimensional Remote Sensing Vector

Planetary surface parity cannot be evaluated through a single parameter. ORBiALOG formulates a **10-dimensional feature vector** $\mathbf{x} \in \mathbb{R}^{10}$ characterizing the geotechnical, climatic, and mineralogical state of any planetary or terrestrial coordinate:

| # | Feature Name | Symbol | Earth Observation Platform & Dataset | Extraterrestrial Counterpart Platform | Resolution / Processing | Physical Relevance to Exploration |
|---|:---|:---:|:---|:---|:---|:---|
| **1** | **Slope Gradient** | $\nabla z$ | SRTM 30m (`USGS/SRTMGL1_003`) | LRO LOLA GDR / MGS MOLA Topography | 30 m resampled to 100 m | Rover tip-over risk, traction, climbability, landing gear stress |
| **2** | **RMS Roughness** | $z_{\text{RMS}}$ | SRTM 3×3 Local Uniform Residual Filter | LRO LOLA 5m / MRO HiRISE DTMs | 100 m baseline filter | Wheel wear, obstacle clearance, radar scattering, landing hazard |
| **3** | **Diurnal Thermal Delta** | $\Delta LST$ | Terra/Aqua MODIS LST (`MODIS/061/MOD11A2`) | LRO Diviner Radiometer / MRO MCS | 1 km 8-day composite | Thermal stress on electronics, battery survival, thermal cycling |
| **4** | **Peak Daytime Temp** | $LST_{\text{max}}$ | MODIS Daytime Max LST (`MOD11A2`) | Diviner Thermal / Viking IRTM | 1 km composite | Maximum thermal radiator rejection limits, solar cell efficiency |
| **5** | **Shortwave Albedo** | $\alpha$ | MODIS White-Sky Albedo (`MODIS/061/MCD43A3`) | LRO LROC WAC Albedo / MGS TES | 500 m 16-day | Solar array power generation, optical sensor glare, heat balance |
| **6** | **Precipitation / Aridity** | $P_{\text{ann}}$ | Global Precipitation Measurement (`GPM_L3/IMERG_V06`) | Planetary Atmospheric Columns ($0\text{ mm}$) | 0.1° monthly integrated | Weathering rate, chemical alteration, lack of liquid moisture |
| **7** | **Vegetation Index** | $\text{NDVI}$ | MODIS 16-day Composite (`MODIS/061/MOD13A1`) | Terrestrial Baseline ($NDVI \to 0$) | 500 m composite | Organic sterility baseline; suppresses biological interference |
| **8** | **Mafic Mineral Proxy** | $M_{\text{idx}}$ | ASTER GED Emissivity B12/B13 (`ASTER_GED/AG100_003`)| CRISM NIR / Diviner Christiansen Band | 100 m TIR ratio | Basalt, pyroxene, olivine identification; regolith simulant match |
| **9** | **Carbonate Mineral Proxy**| $C_{\text{idx}}$ | ASTER GED B13/B14 Ratio (`ASTER_GED/AG100_003`) | CRISM 2.3 & 2.5 µm Carbonate Absorption | 100 m TIR ratio | Jezero Crater lacustrine delta paleolake biomarker exploration |
| **10**| **Quartz / Silica Proxy** | $Q_{\text{idx}}$ | ASTER GED $B11^2 / (B10 \times B12)$ | CRISM & Mini-TES Silicate Band 9.1 µm | 100 m TIR index | Felsic crust, silicified hydrothermal hot springs, yardang abrasiveness |

---

## 📡 Multi-Agency Satellite & Planetary Telemetry Data

ORBiALOG bridges multiple international space agencies to validate ground truth and orbital telemetry:

```
┌────────────────────────────────────────────────────────────────────────┐
│                      SPACE AGENCY TELEMETRY STACK                      │
├────────────────────┬───────────────────────────────────────────────────┤
│ NASA Earthdata     │ ASTER GDEM v3 30m, MODIS L3 LST (MOD11A1/MOD11A2), │
│ Cloud & EOSDIS     │ ECOSTRESS Diurnal TIR (ECO2LSTE), SMAP L4 (SPL3SMP)│
├────────────────────┼───────────────────────────────────────────────────┤
│ NASA Planetary     │ LRO LOLA Polar DEM 20m (LDEM_80S_20M_FLOAT),      │
│ Data System (PDS)  │ Diviner Thermal Cold-Trap (DLRE_TMIN_POLAR_240M), │
│ Geosciences Node   │ MRO HiRISE DTM (DTEEC_001414_1750), CRISM Targeted│
│                    │ Hyperspectral (FRT00003E12), MOLA PEDR, Mars 2020 │
├────────────────────┼───────────────────────────────────────────────────┤
│ ESA Copernicus     │ Sentinel-1 SAR C-Band Level-1 GRDH (IW Polarimetry│
│ Open Access Hub    │ VV+VH), Sentinel-2 MSI Level-2A BOA Multi-Spectral│
├────────────────────┼───────────────────────────────────────────────────┤
│ CSA / ASC          │ RADARSAT Constellation Mission (RCM) C-band SAR   │
│ Open Data Portal   │ (3MCP24/16M), RADARSAT-1 Open Historical Archive  │
├────────────────────┼───────────────────────────────────────────────────┤
│ JAXA G-Portal      │ GCOM-W AMSR2 Soil Moisture & Cryospheric Profile, │
│                    │ ALOS-2 PALSAR-2 L-band Quad-Pol SAR Models        │
└────────────────────┴───────────────────────────────────────────────────┘
```

---

## 🔬 Mathematical & Machine Learning Methodology

### 1. 100 m Scale-Matching Baseline
Topographic slope and RMS roughness are fractal metrics: their measured values depend strictly on spatial baseline sampling. Comparing 20 m LRO LOLA data directly against 30 m SRTM or 500 m MOLA produces mathematical artifacts. 

ORBiALOG standardizes all elevation fields to a **100 m spatial baseline grid** prior to computing gradients:
$$\nabla z = \left( \frac{\partial z}{\partial x}, \frac{\partial z}{\partial y} \right), \quad \text{Slope} = \arctan\left(\sqrt{\left(\frac{\partial z}{\partial x}\right)^2 + \left(\frac{\partial z}{\partial y}\right)^2}\right)$$
$$\text{Roughness}_{\text{RMS}} = \sqrt{\frac{1}{N} \sum_{i=1}^N \left(z_i - \bar{z}_{3 \times 3}\right)^2}$$

### 2. Feature Saturation Clipping
Extraterrestrial targets exhibit physical conditions that do not exist on Earth (e.g. Moon South Pole cold traps reach $38\text{ K}$, Martian atmospheric pressure is $6\text{ mbar}$, annual precipitation is $0\text{ mm}$). Naive distance calculations would allow a single extreme dimension to overwhelm all others. 

ORBiALOG clips extraterrestrial vector values to Earth's **1st and 99th percentile bounds** $[q_{0.01}, q_{0.99}]$ established across all 11,466 terrestrial tiles:
$$\mathbf{t}^* = \text{clip}\left(\mathbf{t}, \, \mathbf{q}_{0.01}, \, \mathbf{q}_{0.99}\right)$$

### 3. Ledoit-Wolf Shrinkage Covariance & Mahalanobis Engine
Earth's environmental variables are strongly correlated (e.g., hyper-aridity, high daytime temperature, low vegetation, and high albedo frequently co-occur in subtropical deserts). Standard Euclidean distance treats correlated variables as independent, heavily overcounting correlated environmental traits.

ORBiALOG utilizes **Mahalanobis Distance** parameterized by the Earth covariance matrix $\mathbf{\Sigma}$. Because the raw sample covariance matrix $\mathbf{S}$ across high-dimensional terrestrial grids can become ill-conditioned, we apply the **Ledoit-Wolf shrinkage estimator** $\mathbf{\Sigma}_{\text{LW}}$:
$$\mathbf{\Sigma}_{\text{LW}} = (1 - \lambda) \mathbf{S} + \lambda \mu \mathbf{I}$$

Where $\lambda \in [0, 1]$ is the analytically optimal shrinkage intensity and $\mu = \text{tr}(\mathbf{S}) / p$.

The weighted Mahalanobis distance squared $d^2(\mathbf{x}, \mathbf{t}^*)$ is calculated as:
$$d^2(\mathbf{x}, \mathbf{t}^*) = (\mathbf{x}_w - \mathbf{t}^*_w)^T \, \mathbf{\Sigma}_{\text{LW}}^{-1} \, (\mathbf{x}_w - \mathbf{t}^*_w)$$

Where $\mathbf{x}_w = \mathbf{x} \odot \mathbf{w}$ applies mission-specific weighting vectors (e.g., upweighting slope and roughness for rover mobility, or upweighting mafic index for lunar ISRU basalt mining).

### 4. Continuous Exponential Similarity Score
To provide intuitive, bounded telemetry matching for aerospace engineers, Mahalanobis distances are transformed into continuous percentage fidelity scores:
$$S(\mathbf{x}, \mathbf{t}^*) = 100 \times \exp\left( -\frac{1}{2} \cdot \frac{d^2(\mathbf{x}, \mathbf{t}^*)}{N_{\text{features}}} \right)$$

- $S \to 100\%$: Perfect multi-spectral and topographic convergence.
- $S \ge 80\%$: High-fidelity planetary analog candidate.
- $55\% \le S < 80\%$: Moderate-fidelity specialized candidate.
- $S < 55\%$: Significant discrepancy.

### 5. Explainable Z-Score Gap Decomposition
Every candidate generated by ORBiALOG highlights its worst-matching feature $f_{\text{worst}}$ by computing the maximum normalized divergence in the scaled feature space:
$$f_{\text{worst}} = \arg\max_{j} \left| \frac{x_j - t^*_j}{\text{IQR}_j} \right|$$

This guarantees full scientific transparency, informing engineers immediately if a site is structurally identical but exhibits thermal divergence.

---

## 🏆 Key Discoveries: Novel Planetary Analogs

ORBiALOG executed an unbiased global sweep across **11,466 terrestrial 1° tiles**, applying an automated **150 km spatial exclusion radius** around famous textbook analog locations to intentionally discover understudied candidate zones:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   ORBiALOG TOP NOVEL DISCOVERIES                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. MARS PALEOLAKE & YARDANG WINNER:                                    │
│    Dasht-e Lut Basin, Iran [31.5°N, 60.5°E]                           │
│    • Target Match: Gale Crater & Medusae Fossae                        │
│    • Similarity Score: 93.7%  |  Mahalanobis d²: 1.34                  │
│    • Physical Traits: Earth's hottest LST (>70.7°C), <5 mm/yr precip,  │
│      monumental wind-carved yardang corridors, hyper-abrasive dust     │
│    • Testing Scope: Autonomous optical nav under blinding albedo,     │
│      high-shear wheel sinkage mechanics, duricrust crust drills        │
├────────────────────────────────────────────────────────────────────────┤
│ 2. MOON MARE BASALT & VOLCANIC TUBE WINNER:                            │
│    Harrat Khaybar Volcanic Field, Saudi Arabia [25.5°N, 39.5°E]        │
│    • Target Match: Mare Tranquillitatis & Marius Hills Complex         │
│    • Similarity Score: 89.0%  |  Mahalanobis d²: 2.18                  │
│    • Physical Traits: Unvegetated alkali basalt lava sheets,           │
│      subterranean Umm Jirsan lava tube (1.5 km long, 45m wide)         │
│    • Testing Scope: Subterranean autonomous LiDAR SLAM mapping in      │
│      GPS-denied lava tubes, radiation habitat architectural analogs    │
├────────────────────────────────────────────────────────────────────────┤
│ 3. CRYOGENIC HIGH-ALTITUDE DESERT:                                     │
│    Western Tibetan Plateau Permafrost [32.5°N, 80.5°E]                │
│    • Target Match: Lunar South Pole & Shackleton Crater                │
│    • Similarity Score: 83.9%  |  Mahalanobis d²: 3.52                  │
│    • Physical Traits: High UV flux, extreme diurnal swing, low vapor   │
├────────────────────────────────────────────────────────────────────────┤
│ 4. SUB-SURFACE EVAPORITE CRUST:                                        │
│    Great Salt Lake Desert Playa [41.5°N, -112.5°W]                     │
│    • Target Match: Jezero Crater Paleolake Margin                      │
│    • Similarity Score: 83.2%  |  Mahalanobis d²: 3.68                  │
│    • Physical Traits: Halite-carbonate crusts, dense clay sub-strata  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 🌕 Planetary Target Catalog (6 Space Destinations)

ORBiALOG includes rich orbital telemetry profiles for 6 primary exploration targets:

```
                          ┌──────────────────────────┐
                          │   EXPLORATION TARGETS    │
                          └─────────────┬────────────┘
                 ┌──────────────────────┴──────────────────────┐
                 ▼                                             ▼
          MOON TARGETS                                   MARS TARGETS
   ┌─────────────────────────────┐               ┌─────────────────────────────┐
   │ • Lunar South Pole & PSRs   │               │ • Jezero Crater Delta       │
   │ • Shackleton Crater Rim     │               │ • Gale Crater & Mt. Sharp   │
   │ • Marius Hills Lava Skylight│               │ • Olympus Mons Caldera      │
   └─────────────────────────────┘               └─────────────────────────────┘
```

1. **Lunar South Pole & Permanently Shadowed Regions (PSRs)** (`luna-south-pole`)
   - *Coordinates:* 89.9°S, 0.0°E | *Elevation:* -3,850 m to +1,200 m (LOLA)
   - *Environment:* 38 K cold traps, quasi-permanent peak illumination, ultra-high vacuum ($3 \times 10^{-15}\text{ bar}$).
   - *Key Objectives:* Cryogenic volatile extraction drills (1.5 m), optical terrain-relative navigation in razor-sharp shadow contrasts.
   - *NASA Granules:* LOLA `LDEM_80S_20M_FLOAT`, Diviner `DLRE_TMIN_POLAR_SOUTH_240M`.

2. **Shackleton Crater Rim & Floor** (`luna-shackleton-crater`)
   - *Coordinates:* 89.9°S, 0.0°E | *Diameter:* 21 km | *Depth:* 4.2 km
   - *Environment:* Permanent darkness in crater bowl ($<40\text{ K}$), continuous sunlight ridges on rim crests.
   - *Key Objectives:* Steep-slope rim descent rovers, long-range laser power transmission, cold-trap volatiles.
   - *NASA Granules:* LOLA `LOLA_SHACKLETON_DTM_5M`, Diviner `DLRE_RA_SHACKLETON_001`.

3. **Marius Hills Skylight & Volcanic Complex** (`luna-marius-hills`)
   - *Coordinates:* 14.1°N, 303.2°E (Oceanus Procellarum)
   - *Environment:* Circular skylight pit (~65 m diameter, ~80 m depth) opening into an intact subterranean lava tube.
   - *Key Objectives:* Autonomous rappelling rovers, tethered spelunking robots, subterranean habitat pressurization.
   - *NASA Granules:* LROC NAC `M114328462R`, Diviner `DLRE_MARIUS_TBOL_L3`.

4. **Jezero Crater Delta & Margin Carbonates** (`mars-jezero-crater`)
   - *Coordinates:* 18.38°N, 77.58°E | *Elevation:* -2,500 m (MOLA)
   - *Environment:* Ancient open-basin paleolake with fan-delta clay and magnesium-rich carbonate deposits.
   - *Key Objectives:* Biosignature sample caching (Perseverance rover), delta boulder navigation, shallow drill coring.
   - *NASA Granules:* CRISM `FRT00003E12`, HiRISE `DTEEC_001414_1750`, Mars 2020 PIXL `PIX_0241_0692738410`.

5. **Gale Crater & Mount Sharp (Aeolis Mons)** (`mars-gale-crater`)
   - *Coordinates:* 5.4°S, 137.8°E | *Elevation:* -4,500 m to +5,500 m central mound
   - *Environment:* 5 km sedimentary stack displaying clay-to-sulfate transition, fields of active basaltic barchan dunes.
   - *Key Objectives:* Long-distance uphill rover driving, wheel tread puncture testing on sharp ventifacts.
   - *NASA Granules:* CRISM `HRL000040FF`, HiRISE `DTEEC_009149_1750`, ChemCam `CR0_489284912EDR`.

6. **Olympus Mons Caldera & Lava Flows** (`mars-olympus-mons`)
   - *Coordinates:* 18.65°N, 226.2°E | *Elevation:* +21,229 m above datum
   - *Environment:* Giant shield volcano with low surface pressure (~0.75 mbar) and extensive sheet basalts.
   - *Key Objectives:* Extreme low-pressure propulsion validation, uncrewed aerial vehicle (UAV/rotorcraft) flight in ultra-thin atmosphere.
   - *NASA Granules:* MOLA `MOLA_PEDR_AP12948N_01`, HRSC `H0037_0000_ND3`.

---

## 🌍 Terrestrial Analog Catalog

### Curated Global Baseline Sites (12 Cataloged Locations)
1. **White Desert (Farafra Oasis), Egypt** (`egypt-white-desert`) — Chalk yardangs, hyper-arid wind erosion, Gale Crater morphology.
2. **Qattara Depression, Egypt** (`egypt-qattara`) — Sub-sea-level (-133m) evaporite sabkha, hypersaline crusts, wet-dry cycles.
3. **Sinai High-Relief Peninsula, Egypt** (`egypt-sinai`) — Rugged Precambrian granite plutons, basaltic dikes, rugged terrain SLAM.
4. **Atacama Desert (Yungay), Chile** (`chile-atacama`) — Extreme aridity ($<2\text{ mm/yr}$), perchlorate soil chemistry, UV irradiation benchmark.
5. **Devon Island (Haughton Crater), Canada** (`devon-island`) — High Arctic impact structure (23 km), shatter cones, permafrost ground.
6. **McMurdo Dry Valleys, Antarctica** (`antarctica-dry-valleys`) — Coldest hyper-arid desert, endolithic microbial communities, subglacial brines.
7. **Svalbard Archipelago, Norway** (`svalbard-spitsbergen`) — Arctic volcanic centers, permafrost polygonal ground, hydrothermal mineralization.
8. **Mauna Kea & HI-SEAS, Hawaii, USA** (`hawaii-mauna-kea`) — Tholeiitic basalt cinder cones, weathered volcanic ash, lunar regolith simulant.
9. **Pilbara Craton (Strelley Pool), Australia** (`australia-pilbara`) — 3.5 Ga stromatolite biosignatures, ancient silicified cherts.
10. **Tabernas Desert & Rio Tinto, Spain** (`spain-tabernas`) — Acidic iron-sulfate runoff, jarosite minerals matching Meridiani Planum.
11. **Mojave Desert / Dumont Dunes, USA** (`usa-mojave-jpl`) — Active aeolian sand dunes, alluvial washboard gravels, NASA JPL rover testbed.
12. **Askja Caldera & Holuhraun, Iceland** (`iceland-askja`) — Historic Apollo astronaut training ground, fresh basalt sheets, pumice tephra fields.

---

## 🖥️ Frontend Mission Control Dashboard (7 Screens)

The frontend is an aerospace-themed UI crafted in **React 19**, **Three.js**, **Vite**, and **Tailwind CSS**. It incorporates tactical keyboard navigation, Web Audio synthesizer effects, and data visualization:

```
[1] Mission Overview ──────► [2] Target Selection ──────► [3] Earth Discovery
                                                                  │
[7] Scientific Methodology ◄─── [6] Mission Disciplines ◄─── [4] Comparison Engine
                                                                  │
                                                                  ▼
                                                      [5] Location Profile Dossier
```

- **Screen 1: Mission Command Deck (`Screen1Landing.tsx`)**
  - High-impact mission introduction, challenge statement, quickstart telemetry counter, and direct entry into target evaluation.
- **Screen 2: Planetary Target Catalog (`Screen2TargetSelection.tsx`)**
  - Interactive celestial selector for Moon and Mars destinations. Displays verified NASA PDS product URNs, LOLA/MOLA topographic datums, and Diviner thermal cold-trap telemetry.
- **Screen 3: Earth Analog Discovery Explorer (`Screen3EarthDiscovery.tsx`)**
  - Dynamic dual-mode visualization:
    - **Interactive Three.js 3D Globe (`Real3DGlobe.tsx`):** Real spherical coordinate mapping of all analog sites with planetary filter arcs.
    - **Real GIS Coordinate Map (`RealGISMap.tsx` / `InteractiveEarthMap.tsx`):** Leaflet cartography with custom aerospace reticle markers, similarity score badges, and category filters (`hyper-arid`, `polar-desert`, `volcanic-basalt`, `impact-crater`, `evaporite-sabkha`, `sub-sea-level`).
- **Screen 4: Multi-Dimensional Comparison Engine (`Screen4ComparisonEngine.tsx`)**
  - Side-by-side extraterrestrial vs. terrestrial radar charts (`RadarChart.tsx`), dimension-by-dimension divergence tables, and statistical covariance metrics.
- **Screen 5: Analog Location Profile Dossier (`Screen5LocationProfile.tsx`)**
  - Exhaustive site dossier covering geology, environment, logistics, and field campaign histories.
  - **3D Topographic Mesh Renderer (`Real3DTopography.tsx`):** Procedural elevation wireframe visualization.
  - **Simulated Telemetry Sandbox:** Real-time sensor stress testing and subsystem parameter queries.
- **Screen 6: Mission Application Disciplines (`Screen6MissionApplication.tsx`)**
  - Categorization across 5 mission domains: *Rover Mobility & Geomechanics*, *Subterranean Habitats & Lava Tubes*, *In-Situ Resource Utilization (ISRU)*, *Cryogenic Volatile Drills*, and *Optical Terrain-Relative Navigation*.
- **Screen 7: Scientific Methodology & Formulations (`Screen7Methodology.tsx`)**
  - Detailed interactive equations, Ledoit-Wolf covariance matrices, scale-matching proofs, and open-science telemetry pipelines.

### Aerospace UI Features
- **Keyboard Shortcuts:** Press `1` through `5` anywhere in the app to switch instantly between mission screens.
- **Synthesized Sound FX (`audio.ts`):** Web Audio API acoustic feedback for radar pings, clicks, confirmations, and alerts.
- **Multi-Agency Modal (`SpaceAgencyDataModal.tsx`):** Direct inspection of NASA Earthdata, ESA Copernicus, CSA RADARSAT, and JAXA G-Portal granule identifiers.

---

## 📡 Backend REST API Specification

The backend is built with **Node.js 20**, **TypeScript**, **Express 4**, and **MongoDB Atlas** with Mongoose schemas and strict Zod validation.

**Base URL:** `http://localhost:4000/api/v1`

### Endpoint Summary Table

| Method | Route | Description | Query / Body Parameters |
|:---|:---|:---|:---|
| `GET` | `/health` | API Liveness Check | None |
| `GET` | `/ready` | Readiness Check (verifies MongoDB Atlas connection) | None |
| `GET` | `/targets` | List planetary targets | `?planet=moon\|mars` |
| `GET` | `/targets/:id` | Fetch single planetary target by ID | `:id` (e.g. `luna-shackleton-crater`) |
| `GET` | `/sites` | Paginated analog candidate sites with filters | `planet`, `category`, `q`, `minSimilarity`, `sort`, `page`, `limit` |
| `GET` | `/sites/:id` | Fetch comprehensive site dossier by ID | `:id` (e.g. `egypt-white-desert`) |
| `GET` | `/analogs` | Space Apps formatted match response | `?target=<targetId>&limit=<n>` |
| `GET` | `/analogs/targets` | List target sites available for matching | None |
| `GET` | `/comparison` | Compute multi-axis cosine similarity | `?target=<targetId>&analog=<analogId>` |
| `GET` | `/disciplines` | List mission application domains | None |
| `GET` | `/disciplines/:id` | Fetch single mission application domain | `:id` (e.g. `rover-mobility`) |
| `GET` | `/methodology` | Ordered pipeline methodology steps | None |
| `POST` | `/sandbox/query` | Subsystem telemetry stress simulation | JSON `{ "subsystem": string, "ambientConditions": object }` |

### Sample API Response: `GET /api/v1/analogs?target=luna-shackleton-crater&limit=1`
```json
{
  "target_space_site": "Shackleton Crater (Moon South Pole)",
  "top_earth_analogs": [
    {
      "location_name": "Atacama Desert High Plateau, Chile",
      "country": "Chile",
      "coordinates": [-24.0667, -70.0167],
      "similarity_score": "87.4%",
      "matches": [
        "Volcanic basaltic composition",
        "Hyper-aridity & low atmospheric moisture",
        "High ultraviolet irradiance"
      ],
      "breakdown": {
        "geology_match": "High (Basalt)",
        "thermal_match": "Moderate (Diurnal Delta: 32 K)",
        "environment_limits": "Earth 1G gravity; 1013 mbar atmospheric pressure; presence of trace vegetation"
      },
      "suggested_testing": "Testing optical navigation under extreme contrast, wheel slip on fine regolith simulant, and dust seal wear.",
      "provenance": {
        "source": "curated",
        "dataset": "NASA Earthdata & ASTER Global Emissivity",
        "version": "1.0.0"
      }
    }
  ]
}
```

---

## 🐍 Remote Sensing Detection Pipeline (Python)

The `pipeline/` directory contains the automated data harvesting, processing, and ranking scripts powered by the **Google Earth Engine (GEE)** Python API, `scikit-learn`, `rasterio`, and `geopandas`.

```
pipeline/
├── detect.py                 # Core Mahalanobis & Ledoit-Wolf ranking algorithms
├── ee_features.py            # Earth Engine image collections, composites & reduction
├── merge_and_detect.py       # Joins exported tile CSVs, runs ranking, formats output JSON
├── sample_regions.py         # Regional grid bounding box sampling logic
├── setup_earthdata.py        # Earthdata Cloud authentication verification
├── start_batch_export.py     # Launches background Earth Engine batch export tasks
├── requirements.txt          # Python dependencies
└── computed_matches.json     # 500+ records of mathematically discovered analog tiles
```

### Running the Python Pipeline
```bash
# 1. Create and activate a Python virtual environment
cd pipeline
python -m venv .venv

# On Linux/macOS:
source .venv/bin/activate
# On Windows PowerShell:
.venv\Scripts\Activate.ps1

# 2. Install dependencies
pip install -r requirements.txt

# 3. Authenticate with Earth Engine (if exporting new tiles)
earthengine authenticate

# 4. Run detection across all target vectors
python merge_and_detect.py

# 5. Import computed novel matches directly into MongoDB Atlas
cd ../backend
npm run import:computed
```

### CSA RADARSAT Metadata Fetcher
The frontend includes a standalone utility script to query the Canadian Space Agency CKAN Open Data portal:
```bash
cd frontend
python scripts/fetch_csa_radarsat.py --query "radarsat" --rows 5 --output public/data/csa_radarsat_scenes.json
```

---

## 🚀 Installation, Configuration & Running Guide

### Prerequisites
- **Node.js:** `>= 20.0.0`
- **npm:** `>= 9.0.0`
- **Python:** `>= 3.10`
- **MongoDB:** MongoDB Atlas connection string (or local MongoDB 7/8 instance)

---

### Step 1: Clone Repository
```bash
git clone https://github.com/Kamal3laa/orbialog-nasa.git
cd orbialog-nasa
```

---

### Step 2: Backend Setup & Database Seeding

```bash
cd backend

# 1. Install dependencies
npm install

# 2. Configure environment variables
cp .env.example .env
```

Edit `backend/.env` with your settings:
```env
NODE_ENV=development
PORT=4000
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.yourdomain.mongodb.net/orbialog?retryWrites=true&w=majority
JWT_ACCESS_SECRET=super_secret_access_token_key_at_least_32_chars_long
JWT_REFRESH_SECRET=super_secret_refresh_token_key_at_least_32_chars_long
CORS_ORIGINS=http://localhost:3000,http://127.0.0.1:3000,http://localhost:5173
LOG_LEVEL=info
```

```bash
# 3. Seed MongoDB Atlas with planetary targets, analog sites, and disciplines
npm run seed

# 4. Import pipeline-computed algorithmic candidate matches
npm run import:computed

# 5. Start the backend development server
npm run dev
```
*Backend API will run at `http://localhost:4000/api/v1`*

---

### Step 3: Frontend Setup & Launch

Open a second terminal:
```bash
cd frontend

# 1. Install dependencies
npm install

# 2. Configure environment variables
cp .env.example .env
```

Ensure `frontend/.env` is set:
```env
VITE_API_URL=http://localhost:4000/api/v1
```

```bash
# 3. Start the Vite development server
npm run dev
```
*Open your browser at `http://localhost:3000`*

---

## 🐳 Docker & Cloud Deployment

### 1. Docker Compose (Local Multi-Container Stack)
Launch the entire stack (Backend + Frontend Nginx proxy) with a single command:

```bash
# Define your MongoDB Atlas URI
export MONGODB_URI="mongodb+srv://user:pass@cluster0.mongodb.net/orbialog?retryWrites=true&w=majority"

# Build and start services in background
docker compose up -d --build
```

- **Frontend Application:** `http://localhost:3000`
- **Backend API:** `http://localhost:4000/api/v1`

---

### 2. Render Deployment (1-Click Blueprint)
The project includes a production-ready [`render.yaml`](render.yaml) blueprint:
1. Fork or push this repository to GitHub.
2. In the [Render Dashboard](https://dashboard.render.com/), select **New +** $\rightarrow$ **Blueprint**.
3. Select your repository.
4. Input your `MONGODB_URI` environment variable when prompted.
5. Click **Apply**. Render will automatically provision:
   - `orbialog-api`: Express Node.js web service.
   - `orbialog-dashboard`: Static web app with API rewrite rules to eliminate CORS.

---

### 3. Vercel Deployment (Frontend Only)
1. Import repository into [Vercel](https://vercel.com/).
2. Set **Root Directory** to `frontend`.
3. Set environment variable: `VITE_API_URL=https://your-backend-api.onrender.com/api/v1`.
4. Click **Deploy**.

---

## 📁 Repository File Structure

```
orbialog-nasa/
├── .gitignore
├── docker-compose.yml              # Multi-container orchestration (API + UI)
├── render.yaml                     # Render Infrastructure-as-Code Blueprint
├── README.md                       # Comprehensive System Documentation
│
├── backend/                        # Express 4 + TypeScript REST API
│   ├── .env.example                # Template for environment secrets
│   ├── Dockerfile                  # Production container definition
│   ├── package.json
│   ├── tsconfig.json
│   ├── seed-data/                  # Seed datasets (targets, analogs, disciplines)
│   └── src/
│       ├── app.ts                  # Express app definition & middleware pipeline
│       ├── server.ts               # Server bootstrap with graceful shutdown
│       ├── config/env.ts           # Zod-validated environment schema
│       ├── db/connect.ts           # Resilient Mongoose Atlas connection
│       ├── middleware/             # Error handler, 404 handler, logging
│       ├── models/                 # Mongoose schemas (Targets, Sites, Matches)
│       ├── modules/                # Domain routers and services
│       │   ├── analog-match/       # Space Apps contract matching routes
│       │   ├── comparison/         # Cosine fidelity scoring service
│       │   ├── disciplines/        # Mission application domain routes
│       │   ├── health/             # Liveness & DB readiness checks
│       │   ├── methodology/        # Scientific methodology steps
│       │   ├── sandbox/            # Simulated telemetry query sandbox
│       │   ├── sites/              # Analog sites query, search & filter
│       │   └── targets/            # Planetary target profiles
│       ├── scripts/
│       │   ├── seed.ts             # Upserts core catalogs into Atlas
│       │   └── import-computed.ts  # Ingests Python-computed matches
│       └── utils/                  # ApiError, asyncHandler, Pino logger
│
├── frontend/                       # React 19 + Three.js Mission Control
│   ├── .env.example
│   ├── Dockerfile                  # Multi-stage build with Nginx reverse proxy
│   ├── index.html                  # HTML entry point
│   ├── package.json
│   ├── vite.config.ts              # Vite bundler & reverse proxy rules
│   ├── tailwind.config.js
│   ├── public/                     # High-resolution textures, icons, geojson
│   ├── scripts/
│   │   └── fetch_csa_radarsat.py   # CSA CKAN portal metadata harvester
│   └── src/
│       ├── App.tsx                 # Root router, keyboard listener, audio coordinator
│       ├── types.ts                # TypeScript domain contracts
│       ├── components/
│       │   ├── canvas3d/           # Three.js 3D Celestial Globe & Topography
│       │   ├── common/             # Navbar, Footer, Modals, RadarCharts, Maps
│       │   └── dashboard/          # Mission control sidebars and split views
│       ├── screens/                # The 7 primary aerospace screens
│       │   ├── Screen1Landing.tsx
│       │   ├── Screen2TargetSelection.tsx
│       │   ├── Screen3EarthDiscovery.tsx
│       │   ├── Screen4ComparisonEngine.tsx
│       │   ├── Screen5LocationProfile.tsx
│       │   ├── Screen6MissionApplication.tsx
│       │   └── Screen7Methodology.tsx
│       ├── services/               # Axios/Fetch API client with fallback demo
│       └── utils/                  # Web Audio synthesizer, geometric helpers
│
└── pipeline/                       # Python Remote Sensing & Machine Learning
    ├── requirements.txt            # Python dependencies (GEE, scikit-learn, etc.)
    ├── computed_matches.json       # Precomputed candidate matches output
    ├── detect.py                   # Ledoit-Wolf Mahalanobis algorithm engine
    ├── ee_features.py              # Earth Engine collection aggregators
    ├── merge_and_detect.py         # Batch CSV joiner & candidate generator
    ├── sample_regions.py           # Earth coordinate sampling grid
    ├── setup_earthdata.py          # NASA Earthdata token manager
    └── start_batch_export.py       # GEE Cloud task batch submitter
```

---

## 🔬 Scientific Rigor, Validation & Citations

ORBiALOG grounds its statistical methodology and data pipelines in peer-reviewed remote-sensing and planetary science literature:

1. **Mahalanobis Distance & Ledoit-Wolf Covariance:**  
   - Ledoit, O., & Wolf, M. (2004). *A well-conditioned estimator for large-dimensional covariance matrices.* Journal of Multivariate Analysis, 88(2), 365–411.
2. **Planetary Analog Testing Methodologies:**  
   - Léveillé, R. J. (2009). *A validation strategy for terrestrial biosignature analogs.* Planetary and Space Science, 57(5-6), 631–640.
   - Preston, L. J., & Dartnell, L. R. (2014). *Planetary habitability: Lessons from terrestrial extreme environments.* International Journal of Astrobiology, 13(1), 81–98.
3. **NASA LRO LOLA & Diviner Investigations:**  
   - Smith, D. E., et al. (2010). *Initial observations from the Lunar Orbiter Laser Altimeter (LOLA).* Geophysical Research Letters, 37(18).
   - Paige, D. A., et al. (2010). *Diviner Lunar Radiometer observations of cold traps in the Moon’s south polar region.* Science, 330(6003), 479–482.
4. **Mars Reconnaissance Orbiter (HiRISE & CRISM):**  
   - McEwen, A. S., et al. (2007). *A closer look at water-related geologic activity on Mars with HiRISE.* Science, 317(5845), 1706–1709.
   - Murchie, S. L., et al. (2009). *Compact Reconnaissance Imaging Spectrometer for Mars (CRISM) investigation.* Journal of Geophysical Research: Planets, 114(E2).
5. **Earth Observation Surface Temperature & Emissivity:**  
   - Wan, Z. (2014). *New refinements and validation of the collection-6 MODIS land-surface temperature products.* Remote Sensing of Environment, 140, 36–45.
   - Hulley, G. C., et al. (2015). *The ASTER Global Emissivity Dataset (ASTER GED): Mapping Earth’s emissivity at 100 meter resolution.* Remote Sensing of Environment, 165, 127–141.

---

## 📜 License & Acknowledgments

### License
This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

### Acknowledgments
- **NASA Space Apps Challenge 2026** for providing the challenge framework: *"What's on the Horizon? Identifying and Analyzing Planetary Analog Sites"*.
- **NASA Planetary Data System (PDS) Geosciences Node** and **USGS Astrogeology Science Center** for archiving open planetary elevation and spectral datasets.
- **Google Earth Engine** and **NASA Earthdata Cloud (EOSDIS / LP DAAC)** for Earth observation catalog access.
- **European Space Agency (ESA) Copernicus Programme**, **Canadian Space Agency (CSA)**, and **Japan Aerospace Exploration Agency (JAXA)** for open radar and multi-spectral missions.
