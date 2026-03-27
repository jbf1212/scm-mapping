# Processing Scripts Directory

This directory contains Jupyter notebooks for collecting, cleaning, and filtering data used in the SCM mapping analysis. Run notebooks in the order shown in the pipeline below.

## Data Pipeline

```
EC3 API
   ↓
ec3_concrete_epd_gather_all.ipynb   → 01_raw_data/epd_data_all.json
   ↓
ec3_concrete_data_cleaning.ipynb    → 02_processed_data/epd_data_cleaned_all.pkl
   ↓
   ├── ec3_concrete_filtering_all.ipynb     → 02_processed_data/epd_data_all.csv
   ├── ec3_concrete_filtering_scm.ipynb  → 02_processed_data/epd_data_fly_ash.csv
   │                                          02_processed_data/epd_data_fly_ash_or_ggbs.csv
   └── ec3_concrete_plant_list.ipynb        → 02_processed_data/active_concrete_plants_ec3.csv

Global Energy Monitor (Excel)
   ↓
coal_plant_data_filtering.ipynb     → 02_processed_data/active_coal_plants_US.csv
```

---

## Notebooks

### EC3 Data Collection

#### `ec3_concrete_epd_gather_all.ipynb`
Queries the EC3 database via the `ec3-python-wrapper` API to retrieve Environmental Product Declaration (EPD) data for all US concrete products.

**Inputs:** EC3 API (requires `EC3_KEY` environment variable)

**Outputs:** `../01_raw_data/epd_data_all.json` (~76,100 records)

**Key filters applied:**
- Non-lightweight concrete only
- US plants only
- Manufacturer-specific and plant-specific EPDs

---

#### `ec3_concrete_data_cleaning.ipynb`
Cleans and standardizes the raw EPD JSON export for downstream analysis.

**Inputs:** `../01_raw_data/epd_data_all.json`

**Outputs:** `../02_processed_data/epd_data_cleaned_all.pkl` (69,305 records)

**Key processing steps:**
- Flattens nested JSON structures (`plant_or_group`, cementitious material fields)
- Converts datetime strings to datetime objects; filters to post-2020 EPDs
- Drops records where both `concrete_compressive_strength_28d` and `concrete_compressive_strength_other` are null
- Converts compressive strength to psi (handles MPa and ksi inputs), rounds to nearest 500 psi; uses the larger of `concrete_compressive_strength_28d` and `concrete_compressive_strength_other` when both are present (captures mixes that reach full strength after 28 days)
- Calculates GWP per cubic yard (multiply by 0.764555 m³/yd³ conversion)
- Removes outliers using IQR method globally and per compressive strength bucket

---

### EPD Filtering

#### `ec3_concrete_filtering_all.ipynb`
Exports the full cleaned EPD dataset to CSV.

**Inputs:** `../02_processed_data/epd_data_cleaned_all.pkl`

**Outputs:** `../02_processed_data/epd_data_all.csv` (all EPDs with valid GWP)

---

#### `ec3_concrete_plant_list.ipynb`
Builds a deduplicated list of concrete plants from the cleaned EPD dataset, with coordinates and EPD counts. Filters to continental US only using a geographic point-in-polygon test.

**Inputs:**
- `../02_processed_data/epd_data_cleaned_all.pkl`
- `../01_raw_data/country_shapefiles/` — Natural Earth country boundary shapefile (read via `geopandas`)

**Outputs:** `../02_processed_data/active_concrete_plants_ec3.csv`

**Key processing steps:**
- Strips leading/trailing whitespace from plant names to merge near-duplicate entries
- Checks for coordinate conflicts (multiple distinct plant names sharing the same lat/lon) and reports any found
- Deduplicates on `plant_or_group.name` and joins the total EPD count per plant
- **Geographic filtering:** Uses `geopandas` to run a point-in-polygon test against the US country boundary from the Natural Earth shapefile. This correctly handles the irregular US-Canada border — a simple latitude cutoff is insufficient because Vancouver Island (e.g. Victoria, BC at 48.4°N) dips south of the 49th parallel but is still Canadian territory. A bounding box clip is then applied to drop Alaska and Hawaii, restricting output to the continental US.
- Output columns: `plant_name`, `latitude`, `longitude`, `epd_count`

---

#### `ec3_concrete_filtering_scm.ipynb`
Filters the cleaned EPD dataset to records containing SCM data.

**Inputs:** `../02_processed_data/epd_data_cleaned_all.pkl`

**Outputs:**
- `../02_processed_data/epd_data_fly_ash.csv` — EPDs with fly ash content only
- `../02_processed_data/epd_data_fly_ash_or_ggbs.csv` — EPDs with fly ash and/or GGBS (4,745 records)

---

### Energy Infrastructure Data

#### `coal_plant_data_filtering.ipynb`
Processes the Global Energy Monitor coal plant tracker to extract active US facilities.

**Inputs:** `../01_raw_data/coal_plant_data/Global-Coal-Plant-Tracker-October-2025-*.xlsx`

**Outputs:** `../02_processed_data/active_coal_plants_US.csv`

**Key filtering steps:**
- Filters to US plants only (1,221 initial records)
- Removes retired plants
- Keeps only plants with "operating" status
- Removes plants where `Coal type` is "waste coal"
- Deduplicates by `GEM location ID` — the source "Units" sheet tracks individual generating units, so multi-unit plants appear multiple times at the same coordinates; the unit with the highest capacity is retained to represent each plant location

---

## Requirements

- **Python packages:** `pandas`, `numpy`, `plotly`, `geopandas`, `ec3-python-wrapper`, `openpyxl`
- **Environment variable:** `EC3_KEY` — required for EC3 data collection notebooks

See [`requirements.txt`](../requirements.txt) for complete dependencies.

## Usage Notes

- Notebooks should be run from within the `03_processing_scripts/` directory
- Raw data is saved to [`01_raw_data/`](../01_raw_data/)
- Cleaned/processed data is saved to [`02_processed_data/`](../02_processed_data/)
- Ensure `EC3_KEY` is set before running EC3 collection notebooks
