# Processing Scripts Directory

This directory contains scripts and notebooks for collecting, processing, and cleaning data used in the supply chain mapping analysis.

## Scripts

### EC3 Data Collection

#### `ec3_concrete_epd_gather.ipynb`
Jupyter notebook that queries the EC3 (Embodied Carbon in Construction Calculator) database to retrieve Environmental Product Declaration (EPD) data for concrete products.

**Outputs:**
- `../01_raw_data/epd_data_all.json` - Complete dataset of concrete EPDs across all compressive strengths

**Key Features:**
- Filters concrete EPDs by various parameters (compressive strength, plant location, etc.)
- Retrieves detailed cementitious material composition data (fly ash, GGBS, etc.)
- Uses the `ec3-python-wrapper` package for API access

#### `ec3_concrete_data_cleaning.ipynb`
Jupyter notebook for cleaning and processing raw EPD data from EC3.

**Inputs:**
- `../01_raw_data/epd_data_all.json` - Raw EPD data

**Outputs:**
- `../02_processed_data/epd_data_cleaned.pkl` - Cleaned and processed EPD data as a pandas pickle file

**Key Processing Steps:**
- Flattens nested JSON structures (plant_or_group, cementitious materials)
- Converts datetime strings to datetime objects
- Extracts and normalizes compressive strength values
- Calculates GWP per cubic yard
- Filters and validates data quality

### OpenEPD Data Collection

#### `openEPD_concrete_epd_gather.ipynb`
Jupyter notebook for gathering concrete EPD data from the OpenEPD database (alternative/supplementary data source).

### Energy Infrastructure Data (coming soon...)

### Python Packages
- `pandas` - Data manipulation and analysis
- `ec3-python-wrapper` - EC3 API access
- `plotly` - Data visualization
- `numpy` - Numerical operations

See [`requirements.txt`](../requirements.txt) for complete package dependencies.

### Environment Variables
- `EC3_KEY` - EC3 API access token (required for EC3 data collection)

## Usage Notes

- Scripts should be run in the order indicated by their filenames when processing a complete data pipeline
- All raw data is saved to [`01_raw_data/`](../01_raw_data/)
- All cleaned/processed data is saved to `02_processed_data/`
- Ensure EC3 API credentials are configured before running EC3 data collection scripts
- Notebooks are designed to be run from within the `03_processing_scripts/` directory

## Data Flow

```
1. Data Collection (ec3_concrete_epd_gather.ipynb)
   ↓
   01_raw_data/epd_data_all.json
   ↓
2. Data Cleaning (ec3_concrete_data_cleaning.ipynb)
   ↓
   02_processed_data/epd_data_cleaned.pkl
   ↓
3. Analysis (../04_analysis_scripts/)
```
