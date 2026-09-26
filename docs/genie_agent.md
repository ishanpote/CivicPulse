# CivicPulse Genie Agent

CivicPulse includes a Databricks Genie Agent that provides natural-language access to the curated Gold-layer intelligence.

The agent is designed to answer questions about complaint demand, backlog pressure, anomalies, persistence, intervention scores, and demand forecasts using the CivicPulse Gold datasets.

## Connected Gold Tables

The Genie Agent uses:

```text
workspace.civicpulse_gold.intervention_intelligence_daily
workspace.civicpulse_gold.demand_forecast_latest
```

These tables provide the historical intervention intelligence and latest forecasting information required by the agent.

## Agent Purpose

The Genie Agent provides evidence-based natural-language analysis of the CivicPulse data.

It can answer questions such as:

- What is the current operational situation?
- Why was a particular date classified as an operational surge?
- Which dates had the highest intervention pressure scores?
- What are the latest complaint demand forecasts?
- What operational conditions were present during recent demand surges?
- How persistent was backlog pressure during a given period?

The agent is instructed to use only the connected CivicPulse Gold data and defined business logic.

## Business Logic

### Operational Surge

An observation is classified as an operational surge when:

```text
demand_z_score >= 3
AND
complaints_received >= 314
```

The `314` threshold represents the historical 95th percentile of complaint demand.

### Demand Persistence

Using the previous 7 recorded observations:

| Operational surges | Classification |
|---:|---|
| 2+ | `SUSTAINED` |
| 1 | `RECENT_SURGE` |
| 0 | `NO_RECENT_SURGE` |

### Backlog Pressure

Current backlog pressure is active when:

```text
complaints_pending >= 8,950
```

Backlog persistence is evaluated separately using the previous 7 recorded observations.

### Intervention Pressure

The intervention pressure score combines:

- Demand intensity
- Backlog intensity
- Demand persistence
- Backlog persistence

The resulting score is an interpretable prioritization signal rather than a calibrated probability.

### Forecast Interpretation

The Genie Agent uses the latest demand forecast from:

```text
workspace.civicpulse_gold.demand_forecast_latest
```

The current forecasting baseline uses a past-only 7-observation moving average.

`forecast_next_observation` represents the expected complaint count for the next recorded observation.

Because the source contains missing calendar dates, the forecast should not automatically be described as a next-day forecast.

## Example Questions

### Current Situation

> What is the current operational situation in Surat, and what evidence supports it?

### Forecast

> What is the latest complaint demand forecast, and how does it compare with the current demand?

### Anomaly Explanation

> Why was September 24, 2026 classified as an `OPERATIONAL_SURGE`?

### Historical Pressure

> Which dates in the last 12 months had the highest intervention pressure scores?

### Surge Events

> What are the top 5 operational surge events in the last 12 months?

## Example Current Output

For the latest available observation, the CivicPulse data contains:

| Metric | Value |
|---|---:|
| Date | 24 September 2026 |
| Complaints Received | 1,055 |
| Complaints Pending | 10,566 |
| Demand Z-Score | 6.24 |
| Anomaly Type | `OPERATIONAL_SURGE` |
| Surge Persistence | `SUSTAINED` |
| Pressure State | `CRITICAL_COMBINED_PRESSURE` |
| Intervention Pressure Score | 78.57 |
| Intervention Pressure Band | `VERY_HIGH` |
| Next-Observation Forecast | 412.57 |

These values are model outputs and observed metrics from the CivicPulse Gold layer.

## Design Principles

The agent is configured to:

- Use only connected CivicPulse Gold data.
- Distinguish observed metrics from calculated metrics and forecasts.
- Respect the recorded-observation nature of the dataset.
- Avoid treating missing calendar dates as zero complaints.
- Preserve source-reported pending values.
- Avoid claiming causal explanations that are not supported by the data.
- Avoid inventing departments, staffing conditions, locations, incidents, or operational facts.
- Explain intervention scores as heuristic prioritization signals rather than probabilities.

## Role in the CivicPulse Architecture

```text
Databricks Gold Layer
        │
        ├── Historical Intervention Intelligence
        │
        └── Latest Demand Forecast
                    │
                    ▼
             Genie Agent
                    │
                    ▼
       Natural-Language Analysis
```

The Genie Agent provides a conversational interface over the analytical outputs produced by the CivicPulse data pipeline.
