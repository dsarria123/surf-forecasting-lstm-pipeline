# Wave Prediction at Pipeline using NOAA Buoy 51101 Data

This project explores time series forecasting of surf conditions at the
Pipeline surf break using historical and real-time observations from
NOAA buoy station 51101. The goal is to translate offshore buoy measurements
into nearshore, spot-specific wave predictions using a combination of
physical transformations and machine learning.

An LSTM model is used to capture temporal patterns in wave and
meteorological data, while preprocessing steps account for wave transit
time, shoaling, and directional effects between the buoy and Pipeline.

---

## Data Source

All data are sourced from NOAA Station 51101 and include both standard
meteorological measurements and spectral wave data. Variables include
wave height, wave period, wave direction, wind speed, and related features.

Data are stored in a PostgreSQL database that is updated periodically using
the NOAA real-time data endpoints.

---

## Data Ingestion and Storage

- **updateSQLDatabase.py**  
  Downloads the latest standard and spectral buoy data from NOAA,
  performs basic cleaning and interpolation, and appends new observations
  to a PostgreSQL database. Duplicate entries are avoided by tracking the
  most recent timestamp already stored.

---

## Preprocessing and Feature Engineering

- **preprocessing.py**  
  Handles missing values, encodes directional variables, and applies
  cyclical transformations (sine and cosine) to time-based features such
  as month and day of year. These transformations help preserve seasonal
  structure in the data.

- **transformations.py**  
  Applies simplified physics-inspired transformations to better approximate
  nearshore wave conditions at Pipeline, including:
  - wave transit time adjustments
  - shoaling coefficient calculations
  - directional refraction effects

These steps are intended to translate offshore buoy observations into
features that more closely reflect conditions at the surf break.

---

## Modeling Approach

- **model.ipynb**  

A Long Short-Term Memory (LSTM) neural network is trained on historical
sequences of buoy and meteorological data to predict future wave conditions.
The model is designed to capture temporal dependencies and evolving trends
in wave behavior.

The notebook includes:
- feature scaling and sequence construction
- model training and validation
- visualizations comparing predicted and observed wave behavior over time

---

## Evaluation

Model performance is evaluated using a time-based train/test split to
preserve temporal ordering. Forecast accuracy is assessed using standard
regression metrics (e.g., RMSE), and results are visualized directly in
the notebook.

---

## Limitations and Future Work

- The model relies on buoy-based observations and simplified physical
  assumptions rather than full nearshore wave simulations.
- Predictions may degrade during rapidly changing storm conditions.
- Future extensions could incorporate data from multiple buoys or explore
  hybrid physical–machine learning approaches to improve spatial accuracy.

---

## Summary

This project demonstrates an end-to-end workflow for scientific time
series modeling, including real-world data ingestion, preprocessing,
domain-informed feature transformations, and neural network forecasting
using real environmental data.
