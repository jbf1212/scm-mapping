# Study for Mapping Supplementary Cementitious Materials

---

## Overview

This repository contains the working materials, data, and analysis for the capstone project of the **Yale School of the Environment – Environmental Data Science Certificate Program**.

The study investigates methods for mapping, analyzing, and understanding **Supplementary Cementitious Materials (SCMs)** — particularly fly ash and ground granulated blast-furnace slag (GGBS) — by correlating concrete Environmental Product Declaration (EPD) data with the locations of coal plants that produce SCM byproducts.

---

## Background

This project explores how cement replacements—specifically fly ash and slag—affect the global warming potential (GWP) of concrete. Cement production accounts for roughly 8% of global CO₂ emissions, making cement reduction one of the most impactful levers for lowering concrete's embodied carbon.

Fly ash and slag, both industrial byproducts, are commonly used as Supplementary Cementitious Materials (SCMs) to partially replace cement. After identifying fly ash as the dominant SCM in the U.S., this project maps active concrete plants and coal plants (a primary domestic source of fly ash) to evaluate geographic proximity and potential supply dynamics.

### Key Questions

- How do different SCMs compare in their ability to reduce GWP?
- How does SCM performance vary across concrete strength classes?
- What is the geographic relationship between concrete plants and coal plants in the continental U.S.?

### Data Sources

**Environmental Product Declarations (EPDs):**
72,000+ records from the [Building Transparency EC3](https://buildingtransparency.org/ec3) database, accessed via the EC3 API using the [ec3-python-wrapper](https://github.com/jbf1212/ec3-python-wrapper).

**Coal Plant Data:**
U.S. coal plant locations and capacity from the [Global Energy Monitor – Global Coal Plant Tracker](https://globalenergymonitor.org/projects/global-coal-plant-tracker/) dataset.

### Summary of Findings

**SCM Performance by Strength:**
While fly ash is generally assumed to reduce GWP, the data shows that for higher-strength mixes (≥ 6,000 psi), fly ash substitution is often associated with higher GWP. Emissions benefits are more consistently observed in lower-strength mixes, suggesting that SCM optimization should be strength-dependent rather than assumed universally beneficial.

**Geographic Patterns:**
Mapping concrete plants against coal plants reveals regional disparities in fly ash availability. The West Coast and Northeast have relatively few coal plants, potentially increasing transport distances and embodied emissions. In these regions, imported fly ash via ship or alternative SCMs may be more viable decarbonization strategies.

---

## Data Pipeline

```
EC3 API
   ↓
ec3_concrete_epd_gather_all.ipynb       → 01_raw_data/epd_data_all.json (~76k records)
   ↓
ec3_concrete_data_cleaning.ipynb        → 02_processed_data/epd_data_cleaned_all.pkl (69,305 records)
   ↓
   ├── ec3_concrete_filtering_all.ipynb → 02_processed_data/epd_data_all.csv
   └── ec3_concrete_filtering_flyash.ipynb → 02_processed_data/epd_data_fly_ash.csv
                                            02_processed_data/epd_data_fly_ash_or_ggbs.csv

Global Energy Monitor (Excel)
   ↓
coal_plant_data_filtering.ipynb         → 02_processed_data/active_coal_plants_US.csv (391 plants)

02_processed_data/ → 04_analysis_scripts/ → 05_outputs/
```

---

## Repository Structure

- **[01_raw_data/](01_raw_data/)**
  Unprocessed source files (JSON, XLSX). EC3 EPD exports and Global Energy Monitor coal/steel plant trackers.

- **[02_processed_data/](02_processed_data/)**
  Cleaned, filtered, and standardized datasets ready for analysis (CSV, PKL).

- **[03_processing_scripts/](03_processing_scripts/)**
  Jupyter notebooks for data collection, cleaning, and filtering.

- **[04_analysis_scripts/](04_analysis_scripts/)**
  Jupyter notebooks for analysis and visualization of processed data.

- **[05_outputs/](05_outputs/)**
  Generated charts, maps, and other exported analysis products.

- **[06_archive/](06_archive/)**
  Deprecated or superseded scripts and data retained for reference.

---

## Interactive Web Tool

An interactive fly ash routing tool built as an extension of this research is available at **[flyashmap.matterflows.com](https://flyashmap.matterflows.com/)**. It is based on the road routing proof-of-concept in [`04_analysis_scripts/road_routing_poc.ipynb`](04_analysis_scripts/road_routing_poc.ipynb).

---

## License

This project is licensed under the **MIT License**.
See the `LICENSE` file for details.
