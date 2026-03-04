# Analysis Scripts Directory

This directory contains Jupyter notebooks for analyzing and visualizing the processed SCM and EPD data. Notebooks here consume outputs from [`02_processed_data/`](../02_processed_data/) and write visualizations to [`05_outputs/`](../05_outputs/).

## Notebooks

### `ec3_epd_analysis.ipynb`
Analyzes EPD data by SCM type and compressive strength, and generates an interactive GWP visualization.

**Inputs:** `../02_processed_data/epd_data_fly_ash_or_ggbs.csv`

**Outputs:** `../05_outputs/gwp_by_compressive_strength_scm.html`

**Key steps:**
- Filters to compressive strengths ≥ 2,500 psi with ≥ 30 samples per strength class
- Classifies mixes by SCM type: Fly Ash, Slag/GGBS, or Both
- Generates an interactive Plotly box plot (GWP vs. compressive strength, colored by SCM type)

---

### `fly_ash_analysis_01.ipynb`
Produces three publication-quality visualizations analyzing fly ash usage patterns and the geographic relationship between concrete plants and coal plants. Written in R using `ggplot2`.

**Inputs:**
- `../02_processed_data/epd_data_fly_ash_or_ggbs.csv`
- `../02_processed_data/active_coal_plants_US.csv`

**Visualizations:**
1. **Distribution of fly ash substitution rates** — Histogram of fly ash as percent of cement content, distinguishing fly-ash-only vs. fly ash + slag mixes
2. **Fly ash percentage vs. GWP** — Scatterplot of substitution rate vs. embodied carbon, colored by compressive strength, with EC3 national baseline reference lines
3. **Geographic map** — US continental map showing concrete plants using fly ash (teal dots) and active coal plants (blue circles scaled to capacity)

---

## Dependencies

- **R packages:** `ggplot2`, `dplyr`, `sf`, `maps` (or equivalent spatial packages)
- **Python packages:** `pandas`, `plotly`

## Usage Notes

- Notebooks load data from `../02_processed_data/` — run processing scripts first if files are missing
- Interactive HTML outputs are saved to `../05_outputs/`
- Static PNG outputs (300 DPI) are saved to `../tests/` during development
