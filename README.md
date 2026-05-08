# Study for Analyzing & Mapping Supplementary Cementitious Materials

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

*EC3 Database:*
75,000+ records from the [Building Transparency EC3](https://buildingtransparency.org/ec3) database, accessed via the EC3 API using the [ec3-python-wrapper](https://github.com/jbf1212/ec3-python-wrapper).

*Broyles EPD Dataset:*
47,000+ records from a [published dataset](https://data.mendeley.com/datasets/r4jgxk2mhn/5) of U.S. concrete mixture EPDs compiled by Jonathan Broyles.

> Broyles, Jonathan (2026), "Compiled Dataset of Concrete Mixture Environmental Product Declarations in the U.S.A.", Mendeley Data, V5, doi: 10.17632/r4jgxk2mhn.5

**Coal Plant Data:**
U.S. coal plant locations and capacity from the [Global Energy Monitor – Global Coal Plant Tracker](https://globalenergymonitor.org/projects/global-coal-plant-tracker/) dataset.

### Summary of Findings
An in-depth analysis of the findings can be found on the [Matter Flows blog](https://www.matterflows.com/2026/03/31/what-72000-concrete-mixes-reveal-about-cement-replacement/). Some key findings are listed below.

**Differences in Datasets:**
This study was originally performed on the EC3 EPD dataset. The EC3 dataset only listed fly ash as being used in 5.5% of mixes, which I suspected was underrepresenting the true count of mixes with fly ash. This is likely due to limitations with data entry into the EC3 system. An analysis based on the Broyles dataset was added later and shows ~47% of mixes including fly ash, which is closer to what we would expect to find.

**SCM Performance by Strength:**
While fly ash is generally assumed to reduce Global Warming Potential (GWP), the EC3 data shows that for higher-strength mixes (≥ 6,000 psi), fly ash substitution is often associated with higher GWP. However, the Broyles dataset shows GWP reductions for fly ash even at these higher strengths. A deeper investigation would be required in order to determine the cause of this discrepancy, though we know the EC3 data is somewhat incomplete and many mixes in the 'Non-SCM' bucket likely contain SCMs.

That said, at higher strengths, fly ash is often getting added as an addition instead of a substitution. A couple reasons for this could be cost (fly ash is often cheaper than portland cement) and workability / pumpability. So there may be reasons as to why we see some differing behaviors in higher strength mixes.

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
   └── ec3_concrete_filtering_scm.ipynb → 02_processed_data/epd_data_fly_ash.csv
                                            02_processed_data/epd_data_fly_ash_or_ggbs.csv

Broyles EPD Dataset (Excel, ~47k records)
   ↓
broyles_epd_data_processing.ipynb       → 02_processed_data/broyles_epd_data_cleaned.csv (44,327 records)
   ↓                                       (includes plant_lat/lon + metro_lat/lon coordinates)
   ├── broyles_epd_analysis.ipynb        → GWP by strength & SCM type (box plot + pie charts)
   └── broyles_a2_gwp_mapping.ipynb      → A2 GWP spike maps by plant & metro location

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
