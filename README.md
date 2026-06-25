# Machine Learning Based Landcover Classification Using Landsat 9 Architecture (2024)

[![Google Earth Engine](https://img.shields.io/badge/Google_Earth_Engine-JavaScript-blue?logo=googleearthengine&logoColor=white)](https://earthengine.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository contains a cloud-optimized Google Earth Engine (GEE) JavaScript processing framework designed to map urban environments using the latest generation of civil earth observation telemetry. The script ingests top-tier imagery from the **Landsat 8/9 Operational Land Imager-2 (OLI-2)** sensor suite to evaluate contemporary land cover distributions for the year 2024 using an optimized Random Forest machine learning classifier.

---

## 🛰️ Technical Framework & Data Parameters

The script applies a highly disciplined, automated workflow broken into five modular stages:

### 1. Advanced Image Acquisition & Scene Reduction
* **Sensor Collection:** Landsat 9 Surface Reflectance Tier 1 Level 2 (`LANDSAT/LC09/C02/T1_L2`).
* **Temporal Window:** January 10, 2024, to March 25, 2024 (strictly capturing dry-season winter envelopes for optimal surface feature contrast).
* **Atmospheric Quality Target:** Scene inclusion strictly constrained to images containing `< 2%` cloud-cover metadata.
* **Reduction Algorithm:** Overlapping scenes are run through a statistical `.mean()` pixel reducer to produce an artifact-free composite, clipped exactly to the research Area of Interest (`aoi`).

### 2. Spectral Feature Predictors
The classifier analyzes land surface reflectance across six critical spectral regions:
* `SR_B2` (Blue), `SR_B3` (Green), `SR_B4` (Red), `SR_B5` (Near-Infrared), `SR_B6` (Shortwave Infrared 1), and `SR_B7` (Shortwave Infrared 2).

### 3. Classifier Tuning & Target Typology
* **Algorithm Execution:** Spreads feature selection over **100 independent decision trees** via the `ee.Classifier.smileRandomForest` package.
* **Target Schema:** Optimizes pixel categorizations across three localized surface boundaries merged from training nodes: `water` (Class 0), `vegetation` (Class 1), and `urban` (Class 2).

### 4. Machine Learning Cross-Validation
* **Partition Splitting Ratio:** Data points are divided randomly using a robust **70/30 partition threshold** ($70\%$ assigned to model training, $30\%$ completely held back for performance verification).
* **Statistical Deliverables:** Automatically creates non-parametric accuracy metrics right in the GEE console window, including a **Confusion/Error Matrix**, **Overall Accuracy (OA)**, and the **Kappa Coefficient ($\kappa$)**.

---

## 💻 Repository Structure & Usage

### File Directory
* `L9_Classification_2024.js` — Core JavaScript pipeline for the GEE runtime environment.

### Execution Instructions
1. Navigate to the web-based [Google Earth Engine Code Editor](https://code.earthengine.google.com/).
2. Create or link an asset polygon representing your study area boundary named `aoi`.
3. Draw or load multi-point vector geometries corresponding to your class signatures: `water`, `vegetation`, and `urban`.
4. Paste the script file from this repository into the editor canvas and execute the pipeline by clicking **Run**.

---

## 📦 Data Exports Enabled
The script features an active export pipeline configured to push processing outputs directly to your cloud directory storage:
* **Task Type:** Automated spatial image export to Google Drive via `Export.image.toDrive`.
* **Output Format:** Multi-band classified GeoTIFF raster.
* **Resolution:** 30 meters.
* **Destination Folder:** `GEE_Exports`

*(Note: The script architecture also includes commented-out modular blocks pre-configured to handle pixel-to-vector conversions (`reduceToVectors`) for direct generation of Shapefiles (`SHP`) and calculations for urban net area metrics in $\text{km}^2$.)*

---

## 📜 License
This repository is open-source software licensed under the [MIT License](LICENSE).
