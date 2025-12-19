# Raw Data Directory

This directory contains raw, unprocessed data files used for supply chain mapping analysis. The data includes Environmental Product Declarations (EPDs) and energy infrastructure location data.

## Directory Structure

### EPD Data Files
JSON files containing Environmental Product Declaration data pulled from the EC3 (Embodied Carbon in Construction Calculator) database.

- `epd_data_all.json` - Complete dataset of EPD data for all concrete compressive strengths, including metadata on supplementary cementitious materials (SCMs) such as fly ash, GGBS, and other additives

Additional EPD JSON files will be added as data collection expands.

### Energy Infrastructure Data

#### Coal Plant Data (`coal_plant_data/`)
Contains location and operational data for coal-fired power plants globally.

**File:** `Global-Coal-Plant-Tracker-October-2025-Supplement-Proposals-outside-of-China.xlsx`

**Description:** Comprehensive dataset of coal plant locations, capacities, and operational status for facilities outside of China.

**Source:** [Global Energy Monitor - Global Coal Plant Tracker](https://globalenergymonitor.org/projects/global-coal-plant-tracker/)

**Citation:**
```
Global Energy Monitor, Global Coal Plant Tracker, October 2025 Supplement release.
```

**Note:** This directory may be expanded in the future to include data on other types of power generation facilities.

## Data Sources

### EC3 Database
The EC3 database provides verified Environmental Product Declarations for construction materials. EPD data in this directory is pulled programmatically using the processing scripts in [`03_processing_scripts/`](../03_processing_scripts/).

### Global Energy Monitor
Global Energy Monitor provides comprehensive tracking of energy infrastructure projects worldwide. Their Global Coal Plant Tracker offers detailed information on coal-fired power plant locations, capacities, ownership, and operational status.

## Usage Notes

- All files in this directory are considered **raw data** and should not be modified directly
- Processed and cleaned versions of this data are stored in the appropriate output directories
- Processing scripts that transform this raw data are located in [`03_processing_scripts/`](../03_processing_scripts/)
- See individual processing scripts for details on how raw data is transformed and used
