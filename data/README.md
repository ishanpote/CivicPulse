# Data

CivicPulse uses public municipal complaint data published through the Government of India's Open Government Data (OGD) platform.

## Dataset

**Dataset:** Surat City Complaint Statistics from April 2015 onward (daily)

**Publisher:** Surat Municipal Corporation

The dataset contains daily complaint statistics with the following fields:

| Field | Description |
|---|---|
| `CityName` | City associated with the record |
| `Date` | Date of the observation |
| `No_of_complain_received` | Complaints reported as received |
| `No_of_complain_disposed` | Complaints reported as disposed |
| `No_of_complain_pending` | Reported pending complaints |

## Source

The original dataset is available through the Government of India's Open Government Data platform.

CivicPulse uses the source data for analytical and machine-learning experimentation. The raw CSV is not stored in this repository.

## Data Handling

The raw dataset is ingested into the Databricks Bronze layer:

```text
workspace.civicpulse_bronze.complaints_raw
```

It is then transformed into the Silver layer:

```text
workspace.civicpulse_silver.complaints_daily
```

and subsequently into curated Gold datasets used for analytics, forecasting, intervention intelligence, Power BI, and the Databricks Genie Agent.

## Data Quality Considerations

The source contains missing calendar dates. CivicPulse preserves these gaps rather than interpreting them as zero complaint activity.

The reported pending count also does not consistently follow the assumed relationship:

```text
expected_pending =
previous_pending
+ complaints_received
- complaints_disposed
```

CivicPulse therefore preserves the source-reported pending values and records the difference as a reconciliation gap for data-quality analysis.

## Reproducibility

To reproduce the pipeline:

1. Obtain the source dataset from the Government of India's OGD platform.
2. Place the CSV in the expected Databricks raw-data location.
3. Run the CivicPulse notebooks in sequence.

The notebooks build the Bronze, Silver, and Gold analytical layers.

See the project [README](/README.md) for the complete architecture and analytical methodology.
