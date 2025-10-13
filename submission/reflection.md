# Reflection

## Objectives

The aim was to build a compact, **AI-free** weather advisor that turns raw forecast data into genuinely useful guidance. Concretely, the system should (1) fetch current and short-range forecasts, (2) normalise hourly data into daily metrics that non-specialists can read at a glance, (3) visualise the outlook, and (4) produce concise, defensible recommendations (e.g., whether to take an umbrella).

## Method

I used the Open-Meteo API for both geocoding and forecast retrieval. The pipeline converts a place name to latitude/longitude, fetches **hourly** variables—`temperature_2m`, `precipitation`, and `wind_speed_10m`—and aggregates them to **daily** indicators: mean temperature (°C), total precipitation (mm), and mean wind (km/h, converted from m/s). I fixed the planning horizon at **four days**, which is long enough to be useful while avoiding the rapid uncertainty growth of longer forecasts. Visual outputs are a temperature line chart and a precipitation bar chart. The advisory is deliberately **deterministic**: *carry an umbrella if total rain across the horizon ≥ **2.0 mm***; show *heat caution if mean temperature ≥ **30 °C***. These thresholds are transparent and easy to justify.

## Results

For **Perth, Western Australia**, the notebook consistently produced a populated four-day window. The temperature trend and precipitation totals were coherent and matched the plotted values. The final “Answer” sentence mirrors the aggregated figures (average temperature, total rain, and high/low), which makes it easy to cross-check the narrative with the charts. Adding common naming variants to the geocoder reduced failures for ambiguous inputs.

## Design Rationale

Hourly ingestion preserves temporal detail, but **daily aggregation** improves interpretability and prevents overreacting to short, noisy spikes (e.g., a brief shower). The four-day horizon is a pragmatic compromise between planning value and forecast reliability. A rule-based advisor keeps the logic auditable for assessment: stakeholders can debate threshold choices without dealing with a black-box model.

## Limitations

The system depends on a public API (availability and rate limits apply). Aggregation can smooth extremes and understate brief downpours or gusty periods. Name-based geocoding can still misinterpret smaller localities. The advisor does not provide severe-weather nowcasts, confidence intervals, or probabilistic outputs.

## Future Work

Extend the advisory with gust thresholds and humidity/heat-index adjustments; add simple uncertainty bands to the plots; and cache responses to reduce latency. A small UI for location disambiguation and question presets would further improve usability.

## Academic Integrity

Runtime is **AI-free**: the notebook uses deterministic Python (requests/pandas/matplotlib) and a public weather API. Conversational assistance supported planning and debugging only; it did not generate program logic or produce outputs during execution.

## References
- Open-Meteo Forecast API — https://api.open-meteo.com/v1/forecast
- Open-Meteo Geocoding API — https://geocoding-api.open-meteo.com/v1/search
- pandas — https://pandas.pydata.org/
- matplotlib — https://matplotlib.org/
