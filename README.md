# 🏥 Healthcare Delta Live Tables (DLT) Pipeline

A robust healthcare data processing pipeline built on **Databricks Delta Live Tables (DLT)** to support streaming ingestion, data enrichment, and analytical aggregations using a **bronze-silver-gold architecture**.

---

## 📊 Pipeline Overview

This project processes healthcare patient records and diagnosis mapping data in a layered fashion:

- **Bronze Layer** – Ingests raw data from CSV files into Delta Tables.
- **Silver Layer** – Cleans and joins patient and diagnosis data.
- **Gold Layer** – Aggregates patient statistics by diagnosis and gender.

---

## 🚀 Technologies Used

- **Databricks Delta Live Tables (DLT)**
- **Apache Spark (PySpark + SQL)**
- **Delta Lake**
- **Databricks Workflows**
- **Google Cloud Storage (GCS)** (data source)

## 🔄 DLT Pipeline Stages
### 🧱 Bronze Layer
Table	                Description
diagnostic_mapping	  Raw diagnosis mapping table
daily_patients	      Raw patient records (streaming)

### 🧱Silver Layer
Table	                  Description
processed_patient_data	Joined and enriched patient + diagnosis data
`
SELECT
  p.patient_id, p.name, p.age, p.gender,
  p.address, p.contact_number, p.admission_date,
  m.diagnosis_description
FROM STREAM(live.daily_patients) p
LEFT JOIN live.diagnostic_mapping m
ON p.diagnosis_code = m.diagnosis_code `

### 🥇 Gold Layer
Table	                           Description
patient_statistics_by_diagnosis	 Aggregates stats by diagnosis
patient_statistics_by_gender	   Aggregates stats by gender

Example:

`SELECT
  diagnosis_description,
  COUNT(patient_id) AS patient_count,
  AVG(age) AS avg_age,
  MIN(age) AS min_age,
  MAX(age) AS max_age
FROM live.processed_patient_data
GROUP BY diagnosis_description`

### 🧪 How to Run on Databricks
-Upload files to /Volumes/incremental_load/default/healthcare_data/.

-Run Feed_raw_tables.ipynb to load raw data.

-Deploy the DLT pipeline using the workflow.json.

-Monitor table creation in the DLT UI.

