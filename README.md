<p align="center">
  <img src="assets/banner.svg" alt="NASA Space Apps - Kepler Exoplanet Transit Detection" width="100%" />
</p>

<p align="center">
  <strong>Machine Learning Classification of Kepler Space Telescope Light Curves & Planetary Transit Signals using XGBoost and React 19</strong>
</p>

<p align="center">
  <a href="https://github.com/AbinVarghexe/NasaSpaceApps/blob/feature/tailwind-css-migration/LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="License" /></a>
  <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/Python-3.10%2B-3776AB.svg?style=for-the-badge&logo=python&logoColor=white" alt="Python" /></a>
  <a href="https://xgboost.readthedocs.io/"><img src="https://img.shields.io/badge/Model-XGBoost-EB5424.svg?style=for-the-badge&logo=xgboost&logoColor=white" alt="XGBoost" /></a>
  <a href="https://fastapi.tiangolo.com/"><img src="https://img.shields.io/badge/Backend-FastAPI-009688.svg?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI" /></a>
  <a href="https://react.dev/"><img src="https://img.shields.io/badge/Frontend-React%2019-61DAFB.svg?style=for-the-badge&logo=react&logoColor=black" alt="React 19" /></a>
  <a href="https://exoplanetarchive.ipac.caltech.edu/"><img src="https://img.shields.io/badge/Data-NASA%20Exoplanet%20Archive-105BD8.svg?style=for-the-badge&logo=nasa&logoColor=white" alt="NASA Archive" /></a>
</p>

---

## Overview

Developed for the **NASA International Space Apps Challenge**, this project applies machine learning to Kepler Space Telescope photometry data to identify bona fide exoplanet transits from false-positive astrophysical signatures (e.g., eclipsing binary star systems, stellar variability, and instrumental noise artifacts).

The system integrates a trained **XGBoost gradient-boosted decision tree classifier** served via a low-latency **FastAPI REST API**, alongside an interactive **React 19 + Tailwind CSS** laboratory dashboard for Kepler Object of Interest (KOI) validation.

```
+------------------------------------+
|  NASA Kepler Exoplanet Archive     |
|  Cumulative KOI Transit Photometry |
+-----------------+------------------+
                  |
                  v
+------------------------------------+
|  Feature Engineering & Cleaning    |
|  - Orbital Period (koi_period)     |
|  - Transit Depth & Duration        |
|  - Planetary Radius & Equilibrium  |
|  - Stellar Temp, Gravity & Radius  |
+-----------------+------------------+
                  |
                  v
+------------------------------------+
|  Trained XGBoost Classifier        |
|  (exoplanet_xgb.joblib)            |
+-----------------+------------------+
                  |
         +--------+--------+
         |                 |
         v                 v
+----------------+  +-------------------------------------+
| FastAPI Server |  | React 19 Space Exploration Frontend |
| (Port 8000)    |  | - Single KOI Lookup by KepID        |
| - /predict     |  | - Custom Transit Feature Simulator  |
| - /features    |  | - Batch CSV Multi-Target Evaluator  |
| - /batch       |  +-------------------------------------+
+----------------+
```

## Features

- **High-Accuracy Kepler Transit Evaluation**: Predicts the probability of a candidate Kepler Object of Interest being a genuine exoplanet vs. a false positive.
- **Three Prediction Modalities**:
  1. **KepID Lookup**: Queries historical Kepler telemetry by celestial Kepler identifier directly from the cumulative archive.
  2. **Custom Transit Parameter Simulator**: Allows researchers to input orbital period, transit depth, planetary radius, and stellar parameters to simulate hypothetical candidates.
  3. **Batch CSV Evaluation**: Uploads datasets containing thousands of candidate rows and exports enriched CSV files with probability scores and verdicts (`CONFIRMED` / `FALSE POSITIVE`).
- **Modern Responsive Dashboard**: Interactive React 19 UI featuring responsive dark space aesthetics, animated nebula effects, and real-time probability gauges.

## Key Photometric Features Analyzed

| Feature | Description | Unit / Metric |
| :--- | :--- | :--- |
| `koi_period` | Orbital period between successive transits | Days |
| `koi_time0bk` | Transit midpoint epoch timestamp | BJD - 2454833.0 |
| `koi_impact` | Sky-projected distance between planetary and stellar centers | Fractional stellar radius |
| `koi_duration` | Duration of the observed transit | Hours |
| `koi_depth` | Fraction of stellar flux obscured during transit | Parts per million (ppm) |
| `koi_prad` | Estimated radius of the candidate planet | Earth radii ($R_\oplus$) |
| `koi_teq` | Equilibrium temperature approximation | Kelvin ($K$) |
| `koi_insol` | Insolation flux received by the candidate planet | Earth flux units |
| `koi_model_snr` | Transit model signal-to-noise ratio | Signal-to-noise ($\sigma$) |
| `koi_steff` | Effective surface temperature of the host star | Kelvin ($K$) |
| `koi_slogg` | Surface gravity of the host star | $\log_{10}(\text{cm/s}^2)$ |
| `koi_srad` | Photometric radius of the host star | Solar radii ($R_\odot$) |

## Getting Started

### 1. Prerequisites

- Python 3.10+
- Node.js 18+ and npm

### 2. Backend Setup (FastAPI & XGBoost)

```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\activate
pip install -r requirements.txt

# Start the FastAPI model server
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```

Interactive API documentation will be available at `http://localhost:8000/docs`.

### 3. Frontend Setup (React 19 + Tailwind CSS)

```bash
cd ../frontend
npm install
npm run dev
```

Open `http://localhost:5173` to explore the Exoplanet transit prediction workbench.

## Repository Structure

```
NasaSpaceApps/
├── assets/
│   └── banner.svg               # Vector visual banner
├── backend/
│   ├── app.py                   # FastAPI REST API endpoints
│   ├── cumulative.csv           # NASA Kepler cumulative dataset
│   ├── exoplanet_xgb.joblib     # Pretrained serialized XGBoost pipeline
│   ├── hugging.py               # Hugging Face Spaces sync utility
│   ├── requirements.txt         # Python dependencies (scikit-learn, xgboost, fastapi)
│   └── README.md                # Model specification
└── frontend/
    ├── src/
    │   ├── components/
    │   │   ├── ExoPredict.jsx   # Interactive prediction controller & CSV batch processor
    │   │   ├── SpaceBackground.jsx
    │   │   └── KeplerVisualizer.jsx
    │   ├── App.jsx
    │   └── main.jsx
    ├── package.json
    └── vite.config.js
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
