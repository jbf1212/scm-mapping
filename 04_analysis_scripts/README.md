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

### `broyles_epd_analysis.ipynb`
Analyzes the Broyles EPD dataset by SCM type and compressive strength, producing the same two visualizations as `ec3_epd_analysis.ipynb` for comparison.

**Inputs:** `../02_processed_data/broyles_epd_data_cleaned.csv`

**Outputs:** `../tests/broyles_gwp_by_compressive_strength_scm.png`, `../tests/broyles_scm_type_breakdown_pies.png`

**Key steps:**
- Classifies mixes by SCM type (Fly Ash, Slag, Both, or No SCM) using the `contains_fly_ash` and `contains_slag` boolean fields
- Filters to compressive strengths 2,500–8,000 psi
- Generates a Plotly box + scatter plot of GWP vs. compressive strength, colored by SCM type, with per-bucket EPD counts and random dot sampling for visual clarity
- Generates a grid of pie charts showing the SCM type breakdown for each strength bucket

---

### `broyles_a2_gwp_mapping.ipynb`
Maps transport-phase (A2) GWP by concrete plant location across the continental US using the Broyles dataset. Produces interactive spike maps where triangular polygon markers are scaled in height to median A2 GWP.

**Inputs:** `../02_processed_data/broyles_epd_data_cleaned.csv`

**Outputs:** HTML interactive maps saved to `../tests/` during development:
- `broyles_a2_gwp_spikes.html` — plant-level spike map (594 locations)
- `broyles_a2_gwp_spikes_metro.html` — metro-consolidated spike map (249 locations)
- `broyles_a2_gwp_spikes_scm_grid.html` — 2×2 grid comparing all four SCM buckets with shared colorscale
- `broyles_a2_gwp_spikes_no_scm.html`, `fly_ash_only.html`, `slag_only.html`, `fly_ash_and_slag.html` — individual SCM bucket maps

**Key steps:**
- Filters to continental US bounds (lat 24°–50°, lon -125° to -66°); drops records missing coordinates or A2 GWP
- Aggregates median A2 GWP per plant (by lat/lon) and per metro centroid (via `metro_lat`/`metro_lon`)
- Renders spikes using Plotly `Scattermap` polygon fill on a Carto Positron basemap; spikes are binned into 20 color bands (YlGnBu colorscale) for rendering efficiency
- Produces SCM-bucket breakdowns by filtering on `contains_fly_ash` / `contains_slag` flags before aggregation

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

### `road_routing_poc.ipynb`
Proof-of-concept demonstrating road-based routing from a single concrete plant to its five nearest coal plants by drive distance.

A fully interactive version of this analysis was further developed and can be found at **[flyashmap.matterflows.com](https://flyashmap.matterflows.com/)**.

**Inputs:**
- `../02_processed_data/active_concrete_plants_ec3.csv`
- `../02_processed_data/active_coal_plants_US.csv`

**Outputs:** `../05_outputs/routing_poc_[location].html`

**Key steps:**
- Selects an anchor concrete plant by filtering to a geographic region (e.g., Pennsylvania or near NYC) and picking the plant with the most EPDs
- Pre-filters coal plants to the 20 nearest candidates using a `scipy.spatial.KDTree` on unit-sphere coordinates to minimize API calls
- Queries the OSRM public API for actual road distance and route geometry for each candidate
- Ranks candidates by road distance and selects the top 5
- Renders an interactive Folium map with color-coded route polylines, coal plant markers, distance labels, and per-truckload emissions estimates (GWP and diesel consumption)

---

### `road_shortest_distance_study.ipynb`
Calculates the road distance from every active concrete plant to its nearest coal plant and compares regional averages against NRMCA-reported truck distances.

**Inputs:**
- `../02_processed_data/active_concrete_plants_ec3.csv`
- `../02_processed_data/active_coal_plants_US.csv`
- `../02_processed_data/concrete_plant_closest_coal_distances.csv` *(pre-computed; see note below)*

**Outputs:**
- `../02_processed_data/concrete_plant_closest_coal_distances.csv` — per-plant closest coal plant distances

**Key steps:**
- Downloads US state boundary shapefile (Census Bureau TIGER/Line) and performs a spatial join to assign each concrete plant to an NRMCA region
- Pre-filters coal plant candidates using a `scipy.spatial.KDTree` on unit-sphere coordinates
- Queries the OSRM public API for actual road distance to each candidate, taking the minimum across the top 5 nearest coal plants
- Aggregates mean distances by NRMCA region and nationally
- Outputs a 9-row table (8 regions + national average) comparing NRMCA-reported truck distances to calculated values

**Note on the OSRM routing cell:** Results are pre-computed and saved to `concrete_plant_closest_coal_distances.csv`. That cell only needs to be re-run if the input plant data changes. Running from scratch takes over an hour; the cell resumes automatically from any existing CSV, so only new/missing plants are processed. Coverage from the initial run: 1,026 of 1,035 region-assigned plants (~99%).

---

## Dependencies

- **R packages:** `ggplot2`, `dplyr`, `sf`, `maps` (or equivalent spatial packages)
- **Python packages:** `pandas`, `plotly`, `numpy`, `requests`, `folium`, `scipy`, `pgeocode` (used in processing pipeline; see `requirements.txt`)

## Usage Notes

- Notebooks load data from `../02_processed_data/` — run processing scripts first if files are missing
- Interactive HTML outputs are saved to `../05_outputs/`
- Static PNG outputs (300 DPI) are saved to `../tests/` during development
