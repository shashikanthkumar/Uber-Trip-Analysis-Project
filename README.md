# Uber Trip Analysis: Spatial Clustering & Demand Forecasting

## Project Overview

This project analyses approximately **1 million Uber pickup records in New York City from September 2014** to examine the spatial distribution of pickups, identify geographic demand clusters and forecast hourly pickup activity.

The analysis combines exploratory data analysis (EDA), geographic clustering, temporal profiling and time-series forecasting to investigate where and when Uber pickups occur.

### Objectives

- Perform exploratory data analysis to understand pickup patterns and data quality.
- Identify geographic pickup clusters using K-Means clustering.
- Evaluate candidate cluster counts using multiple internal clustering metrics.
- Compare K-Means with density-based clustering using DBSCAN.
- Analyse pickup activity by cluster, hour and date.
- Forecast hourly pickup demand using multiple time-series models.
- Evaluate forecasting performance on a chronological holdout period.

## Dataset

- **Dataset:** Uber pickup data, New York City, September 2014.
- **Source:** Uber NYC pickup dataset.
- **Records analysed:** 1,003,491 cleaned pickup records.
- **Time period:** September 1–30, 2014.

Each record contains:

| Variable | Description |
|---|---|
| Date/Time | Pickup timestamp |
| Lat | Pickup latitude |
| Lon | Pickup longitude |
| Base | TLC-licensed dispatching base code |

## Methodology

### 1. Exploratory Data Analysis (EDA)

The dataset was inspected and cleaned to understand pickup distribution, data quality and temporal patterns.

The analysis includes:
- Data cleaning and validation.
- Missing and duplicate record checks.
- Geographic bounding-box filtering.
- Pickup distribution analysis.
- Temporal analysis by hour and date.
- Visual exploration of spatial and temporal patterns.

### 2. Geographic Preprocessing

Latitude and longitude coordinates were transformed into a local planar coordinate system measured in kilometres using a local equirectangular projection.

This transformation allows distance-based clustering to operate on geographically meaningful distances rather than raw latitude and longitude values.

The transformation was validated through inverse-coordinate round-trip checks and comparison with haversine distances.

### 3. Geographic Clustering

#### K-Means

K-Means was used as the primary clustering method to partition pickup locations into interpretable geographic zones.

- Candidate values of k: 2–10.
- Candidate cluster counts evaluated on a reproducible sample.
- Final clustering performed using MiniBatchKMeans on the cleaned dataset.
- Cluster centroids and pickup volumes analysed.

#### Cluster Selection

Candidate cluster counts were evaluated using:

- Inertia.
- Silhouette score.
- Davies–Bouldin index.
- Calinski–Harabasz index.

A documented selection rule was used to identify the final candidate cluster count.

#### DBSCAN

DBSCAN was used as a comparative density-based clustering approach, with the haversine metric applied to geographic coordinates.

The comparison explores the differences between centroid-based clustering and density-based clustering.

### 4. Temporal Analysis

Pickup activity was examined across time to identify patterns in demand.

The analysis includes:
- Hourly pickup distributions.
- Pickup activity by date.
- Cluster-level hourly activity.
- Cluster-level temporal profiles.
- Hourly demand aggregation.

### 5. Time-Series Forecasting

Hourly pickup counts were aggregated into a chronological time series.

A chronological train/test split was used, with the final seven days reserved for model evaluation.

The forecasting models implemented include:

| Model | Description |
|---|---|
| Seasonal Naive (24h) | Daily seasonal baseline |
| Seasonal Naive (168h) | Weekly seasonal baseline |
| Holt-Winters | Additive trend and seasonal model |
| Holt-Winters | Seasonal model without trend |
| SARIMA | Non-seasonal and seasonal ARIMA configurations |

The models were evaluated on the same holdout period to compare their forecasting performance.

### 6. Evaluation

Forecasting performance was evaluated using:

- Mean Absolute Error (MAE).
- Root Mean Squared Error (RMSE).
- Mean Absolute Percentage Error (MAPE).

A final validation suite was also implemented to verify data integrity, clustering outputs, chronological split integrity, forecasting outputs and consistency of the analytical pipeline.

## Results

### Geographic Clustering

- **1,003,491** cleaned pickup records were analysed.
- **8 geographic clusters** were selected using the documented candidate-selection procedure.
- Cluster centroids, cluster sizes and temporal profiles were generated.
- DBSCAN was implemented as a comparative clustering method.

### Forecasting Results

The best-performing model in the evaluated holdout period was:

| Model | MAE | RMSE | MAPE |
|---|---:|---:|---:|
| Seasonal Naive (168h) | 158.3 | 238.4 | 10.55% |

The weekly seasonal naive model outperformed the other successfully evaluated forecasting models on the holdout period based on MAE.

The results suggest that repeating the preceding week's hourly demand pattern provided a stronger baseline for this dataset than the other tested configurations.

### Pipeline Validation

The analytical pipeline passed **30 out of 30 validation checks**, covering:

- Data cleaning and integrity.
- Coordinate transformation.
- Clustering consistency.
- Cluster summary integrity.
- Temporal aggregation.
- Train/test chronological separation.
- Forecast generation.
- Forecast metric validation.

## Technologies Used

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Scikit-learn**
- **Statsmodels**
- **Jupyter Notebook**

## Limitations

- The dataset covers only September 2014, limiting the analysis of long-term seasonal patterns.
- Geographic K-Means assumes compact clusters and is sensitive to initialization and outliers.
- DBSCAN results depend on the selected density parameters.
- Forecasting evaluation uses a single chronological holdout period and forecast origin.
- Forecasting models were trained on a relatively short historical window.
- No external variables such as weather, public events or traffic conditions were included.
- Forecasting performance on this holdout period does not establish general performance on other periods.

## Future Improvements

- Extend the analysis to multiple months or years.
- Introduce external variables such as weather, events and holidays.
- Use rolling-origin cross-validation for forecasting.
- Explore alternative spatial clustering techniques such as HDBSCAN or Gaussian Mixture Models.
- Develop cluster-level forecasting models.
- Investigate additional forecasting approaches for longer-term demand prediction.

## Project Structure

```text
Uber-Trip-Analysis-Project/
│
├── notebook.ipynb
├── uber-data.csv
├── README.md
└── ...
```

## Author

**Shashikanth Kumar Sole**

MSc Data Science & Business Analytics  
University of Warsaw

[GitHub Profile](https://github.com/shashikanthkumar)

