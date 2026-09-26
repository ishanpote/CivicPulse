# Power BI Dashboard

CivicPulse includes a three-page Power BI dashboard connected to the Databricks Gold layer.

## Data Connection

The dashboard is connected to the following Databricks Gold datasets:

- `workspace.civicpulse_gold.service_intelligence_daily`
- `workspace.civicpulse_gold.intervention_intelligence_daily`
- `workspace.civicpulse_gold.demand_forecast_latest`
- `workspace.civicpulse_gold.decision_support_latest`

The Power BI model keeps these analytical tables independent because they serve different purposes and do not require artificial relationships.

---

## Dashboard Pages

### 1. Operations Overview

Provides a high-level view of the latest operational metrics:

- Current complaints
- Current backlog
- Intervention pressure
- Next-observation forecast
- Current operational situation
- Daily complaint demand trend
- Daily complaint backlog trend

The page is designed to provide a quick view of the current state of the service system.

---

### 2. Demand & Anomaly Intelligence

Focuses on unusual complaint-demand behavior.

Visuals include:

- Demand anomaly classification
- Operational demand surges
- Demand surge persistence
- Operational surge event table

The event table provides:

- Date
- Complaints received
- Demand Z-score
- Surge persistence
- Intervention pressure
- Pressure state

---

### 3. Intervention Intelligence

Focuses on prioritization signals derived from demand and backlog conditions.

Visuals include:

- Intervention pressure distribution
- High-priority intervention events
- Backlog pressure persistence
- Priority intervention event table
- Current demand Z-score
- Forecast change
- Current intervention band
- Current decision signal

---

## Key Metrics

### Intervention Pressure

The intervention pressure score is an interpretable heuristic combining:

- Demand intensity
- Backlog intensity
- Demand persistence
- Backlog persistence

It is a prioritization signal rather than a probability or calibrated risk score.

### Next-Observation Forecast

The forecast represents the expected complaint count for the next recorded observation using the historical 7-observation moving-average baseline.

Because the source contains missing calendar dates, this should not automatically be interpreted as a next-day forecast.

---

## Dashboard Screenshots

Screenshots of the dashboard pages are stored in this directory:

```text
powerbi/
