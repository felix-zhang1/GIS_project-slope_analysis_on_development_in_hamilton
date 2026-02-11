# Parcel-Level Slope Analysis (Hamilton City)

Gongfan Zhang
Technical report summary date: 09/02/2026 

---

# Part 1 — Problem and Goals

## Problem

Development capacity assessments often reflect planning rules but do not explicitly include physical site constraints such as terrain slope. Slope can affect earthworks, access, drainage, stability, and construction cost, which can reduce practical feasibility at the parcel level. 

## Goals

This project assesses parcel-level slope conditions across Hamilton City to identify where slope may constrain residential and non-residential development, and to provide a reproducible workflow that can be re-run when inputs or assumptions change. 

---

# Part 2 — Methodology

## Data

* 1 m LiDAR-derived DEM
* NZ Primary Land Parcels
* Hamilton City boundary
* Residential / Non-Residential zoning
* Planning constraint layers (Facilities Zone, Knowledge Zone, Open Space Zone, Significant Natural Area) 

All datasets were clipped to the Hamilton City boundary, and constraint areas were removed from parcels before calculating slope metrics. 

## Workflow (high level)

* Calculate slope (degrees) from the DEM 
* Reclassify slope for area-based calculations
* Use ***Tabulate Area*** to compute slope-class area within each parcel
* Define steep slope as **≥ 18°**, then compute steep-slope area and proportion per parcel
* Classify steep-slope proportion into fixed **5% intervals** for comparison
* Post-process tables and produce plots with Python (ArcPy + pandas + matplotlib)
* Produce maps to visualise datasets 

## Automation

The workflow was implemented as an automated Python/ArcPy pipeline and validated by comparing automated outputs with manual checks and visual inspection. 

---

# Part 3 — Results

## 3.1 Literature-derived slope classification (planning-oriented)

Slope was classified into six slope ranges, linked to development suitability categories (S1–S4, N1–N2): 

| Suitability | Slope (°) | Summary interpretation        |
| ----------- | --------: | ----------------------------- |
| S1          |       0–6 | Highly suitable               |
| S2          |     >6–12 | Suitable                      |
| S3          |    >12–18 | Moderately suitable           |
| S4          |    >18–25 | Low suitability               |
| N1          |    >25–35 | Generally unsuitable          |
| N2          |       >35 | Unsuitable / high-hazard zone |

Steep slope threshold used for parcel screening: **≥ 18°**. 

---

## 3.2 City-level results

Hamilton City is dominated by low to moderate slopes:

* 0–6°: **80.0%**
* <18° total: **95.9%**
* ≥35°: **0.5%** 

This indicates that slope is not a dominant city-wide constraint.

City-Level Slope Map
<p align="center"> <img src="images/1.city_level_overall slope display.jpg" width="800"> </p>

The map shows that gentle slopes dominate the city.
Steep slopes appear limited and spatially fragmented.
They are mainly associated with river corridors and local terrain features.

---

## 3.3 Zone-level results

Residential land has higher ***steep-slope*** exposure than non-residential land: 

* Residential zone: steep slopes (≥18°) ~ **4.6%**
* Non-residential zone: steep slopes (≥18°) ~ **2.2%** 

This suggests slope constraints are more relevant within residential areas.


* 3.3.1 **Residential** Steep Slope Distribution
<p align="center"> <img src="images/2.zone_level_steep_slope_in_res.jpg" width="800"> </p>
Steep slopes within residential land are spatially localised and often follow terrain structure.

* 3.3.2 **Non-Residential** Steep Slope Distribution
<p align="center"> <img src="images/3.zone_level_steep_slope_in_nonRes.jpg" width="800"> </p>
Spation pattern in non-residential zone is similar to that in residential zone

---

## 3.4 Parcel-level results (core output)

Parcel-level analysis provides the most meaningful insight.

Across all parcels:

* 77.7% contain < 5% steep slope

* ~86% contain < 10% steep slope

* Parcels with > 25% steep slope are rare

However, parcels with ≥10% steep slope coverage form clear spatial clusters.

<p align="center"> <img src="images/parcel_level_all parcels_10plus_steep_slope.jpg" width="800"> </p>

Parcels with ≥10% Steep Slope Coverage

These parcels are:

* Spatially clustered

* Associated with river margins

* Terrain-dependent rather than evenly distributed

This confirms that slope constraints are site-specific and parcel-dependent. 

---

# Part 4 — Limitations & Future work
## 4.1 Limitations

* Scope
The analysis considers slope only. Other constraints (soil, flood risk, infrastructure, access) are not included.

* DEM Resolution
The 1 m LiDAR DEM captures fine terrain detail but may introduce local noise. No smoothing was applied.

* Classification Assumptions
Steep slope is defined as ≥18°. Parcel exposure is grouped into fixed 5% intervals. These thresholds are analytical choices and do not represent engineering cost breakpoints.

## 4.2 Future Work

* Multi-Constraint Analysis
Integrate soil, flood, and infrastructure layers for a broader feasibility assessment.

* Cost Linkage
Relate slope exposure to earthworks intensity and construction implications.

* Capacity Modelling
Incorporate parcel-level slope metrics into development capacity or yield models.

* Sensitivity Testing
Evaluate alternative slope thresholds, interval schemes, and DEM resolutions.
