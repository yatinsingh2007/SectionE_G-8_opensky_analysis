# OpenSky Global Flight Analysis

End-to-end data pipeline and analytics project built on OpenSky Network state vectors. The workflow ingests raw API data, cleans and validates it, engineers features, and produces Tableau-ready datasets and dashboards.

## Project Highlights

- Flight volume, altitude, and velocity trends
- Geospatial distributions by country and corridor
- Feature engineering for ML-ready heading and time fields
- Tableau dashboards and project reports

## Repository Layout

```
data/
  raw/opensky.csv
  processed/cleaned_dataset.csv
  processed/tableau_ready_dataset.csv
docs/data_dictionary.md
notebooks/01_extraction.ipynb
notebooks/02_cleaning.ipynb
notebooks/03_eda.ipynb
notebooks/04_statistical_analysis.ipynb
notebooks/05_final_load_prep.ipynb
reports/OpenSky-Capstone-Report .pdf
reports/OpenSky-Capstone-Presentation.pdf
tableau/dashboard_links.md
tableau/screenshots/
DVA-focused-Portfolio/
DVA-oriented-Resume/
```

## Pipeline Notebooks

1. 01_extraction: Collects OpenSky data via the REST API.
2. 02_cleaning: Cleans and imputes missing fields.
3. 03_eda: Exploratory data analysis of temporal and spatial patterns.
4. 04_statistical_analysis: Feature engineering and distribution analysis.
5. 05_final_load_prep: Prepares the Tableau-ready dataset.

## Data Dictionary

Field definitions, cleaning logic, and derived columns are documented in docs/data_dictionary.md.

## Tableau Dashboards

Links to the published dashboards are listed in tableau/dashboard_links.md. Screenshots are stored in tableau/screenshots/.

## Reports

The final report and presentation are available in reports/.

## Team Portfolios and Resumes

Portfolio links are stored in DVA-focused-Portfolio/. Resumes are stored in DVA-oriented-Resume/.

## Setup

Install the core analysis dependencies:

```
pip install pandas numpy matplotlib seaborn scipy requests
```

Run the notebooks in order from 01_extraction through 05_final_load_prep.
