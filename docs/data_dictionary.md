# Data Dictionary: OpenSky Flight Analysis

Use this file to document every important field in your dataset. A strong data dictionary makes your cleaning decisions, KPI logic, and dashboard filters much easier to review.

## How To Use This File

1. Add one row for each column used in analysis or dashboarding.
2. Explain what the field means in plain language.
3. Mention any cleaning or standardization applied.
4. Flag nullable columns, derived fields, and known quality issues.

## Dataset Summary

| Item | Details |
|---|---|
| Dataset name | OpenSky Flight Data |
| Source | OpenSky Network API (`/states/all`) |
| Raw file name | `opensky.csv` |
| Processed file name | `cleaned_dataset.csv` |
| Last updated | 2024-03-20 (based on extraction notebook) |
| Granularity | One row per state vector (per aircraft per timestamp) |

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|---|---|---|---|---|---|
| `icao24` | string | Unique ICAO 24-bit address of the transponder | `a808b5` | Identification | Primary Key. Case-insensitive. |
| `callsign` | string | Callsign of the vehicle (8 characters) | `UAL123` | Identification | Trimmed and standardized. |
| `origin_country` | string | Country name which the transponder is registered to | `United States` | Aggregation | Used to derive `country_group`. |
| `time_position` | float | Unix timestamp for the last position update | `1614556800` | Temporal logic | Used for `time_position_dt`. |
| `last_contact` | float | Unix timestamp for the last update in general | `1614556805` | Temporal logic | Used for `last_contact_dt`. |
| `longitude` | float | WGS-84 longitude in decimal degrees | `-122.375` | Geospatial | Rows with null coordinates dropped. |
| `latitude` | float | WGS-84 latitude in decimal degrees | `37.619` | Geospatial | Rows with null coordinates dropped. |
| `baro_altitude` | float | Barometric altitude in meters | `10668.0` | Flight Profile | Median imputed for missing values. |
| `on_ground` | boolean | Whether aircraft is on the ground | `False` | KPIs | Critical for filtering airborne flights. |
| `velocity` | float | Ground speed in m/s | `230.5` | Flight Profile | Median imputed for missing values. |
| `true_track` | float | True track in decimal degrees (0 is north) | `90.0` | Geospatial | Used for `heading_sin` and `heading_cos`. |
| `vertical_rate` | float | Vertical speed in m/s | `0.0` | Flight Profile | Set to 0 if `on_ground` is True. |
| `geo_altitude` | float | Geometric altitude in meters | `11000.0` | Flight Profile | Median imputed for missing values. |

## Derived Columns

| Column Name | Data Type | Logic / Formula | Description |
|---|---|---|---|
| `time_position_dt` | datetime | `pd.to_datetime(time_position, unit='s')` | Human-readable position timestamp. |
| `last_contact_dt` | datetime | `pd.to_datetime(last_contact, unit='s')` | Human-readable contact timestamp. |
| `country_group` | string | Top 8 countries + "Other" | Categorical grouping for cleaner visualization. |
| `heading_sin` | float | `sin(true_track)` | Sine transform of heading for ML models. |
| `heading_cos` | float | `cos(true_track)` | Cosine transform of heading for ML models. |

## Data Quality Notes

- **Dropped Columns**: `sensors`, `spi`, `squawk`, and `position_source` were removed due to high null counts (>90%).
- **Null Handling**: Coordinates (`latitude`, `longitude`) are mandatory; rows without them were dropped.
- **Imputation**: Altitudes and velocity were imputed using group-wise medians (by country/on_ground) to preserve distribution.
- **Logical Consistency**: `vertical_rate` was forced to `0.0` for all records where `on_ground` is `True`.
