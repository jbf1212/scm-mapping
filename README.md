# Study for Mapping Supplementary Cementitious Materials

**Status:** *Work in Progress*
**Note:** The data and analyses in this repository are incomplete and actively evolving.

---

## Overview

This repository contains the working materials, data, and analysis for the capstone project of the **Yale School of the Environment – Environmental Data Science Certificate Program**.

The study investigates methods for mapping, analyzing, and understanding **Supplementary Cementitious Materials (SCMs)** — particularly fly ash and ground granulated blast-furnace slag (GGBS) — by correlating concrete Environmental Product Declaration (EPD) data with the locations of coal and steel plants that produce SCM byproducts.

This is an ongoing project, and the contents will be updated frequently as the research progresses. Some datasets, notebooks, and outputs may be partial, experimental, or subject to revision.

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

## License

This project is licensed under the **MIT License**.
See the `LICENSE` file for details.
