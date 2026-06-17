# hdhi-healthcare-analytics-postgresql
Healthcare analytics and data engineering project analyzing hospital admissions, ICU utilization, 
patient outcomes, and cardiovascular conditions using PostgreSQL.

# Heart Disease Hospitalization Inferences (HDHI)

## PostgreSQL Healthcare Analytics & Data Engineering Project

### Project Overview

The Heart Disease Hospitalization Inferences (HDHI) project is an end-to-end healthcare analytics initiative built using PostgreSQL. The project focuses on transforming raw hospital admission records into a clean, analytics-ready dataset capable of supporting clinical reporting, operational decision-making, and business intelligence.

The underlying dataset originates from Hero DMC Heart Institute, a tertiary cardiac care center operating under Dayanand Medical College and Hospital in Ludhiana, Punjab, India. The data was collected over a two-year period from April 2017 through March 2019 and contains information from 14,845 hospital admissions representing 12,238 unique patients. During the study period, 1,921 patients experienced multiple admissions, creating real-world challenges related to duplicate records and patient-level hospitalization tracking.

The dataset captures a broad range of clinical and operational variables, including:

* Patient demographics (age, gender, rural/urban locality)
* Admission pathways (Emergency vs Outpatient)
* Length of hospital stay
* ICU stay duration
* Lifestyle factors (smoking and alcohol use)
* Comorbidities such as diabetes, hypertension, coronary artery disease, chronic kidney disease, and cardiomyopathy
* Laboratory measurements including hemoglobin, glucose, creatinine, BNP, platelet count, and cardiac enzymes
* Cardiovascular diagnoses including STEMI, ACS, Heart Failure, Arrhythmias, Cardiogenic Shock, Pulmonary Embolism, and Valvular Heart Disease
* Patient discharge outcomes

The original dataset was created to support clinical outcome research and was later used in a peer-reviewed study focused on predicting in-hospital cardiac outcomes using machine learning techniques.

---

## Project Objectives

The primary goal of this project was to simulate a real-world healthcare data engineering workflow by building a structured analytical environment capable of answering operational and clinical business questions.

Specific objectives included:

* Designing a PostgreSQL-based healthcare data warehouse
* Importing and storing raw hospital admission data
* Standardizing inconsistent source formats
* Performing duplicate detection and remediation
* Validating clinical timelines and hospitalization records
* Building reusable analytical views
* Developing business intelligence queries for healthcare decision support
* Creating a trusted reporting layer for downstream analytics

---

## Data Engineering Architecture

The project follows a multi-layered analytical architecture commonly used in modern healthcare analytics platforms.

### Layer 1 — Raw Data Ingestion

Raw CSV records are loaded into a PostgreSQL landing table.

Responsibilities:

* Preserve source system integrity
* Enable reproducible ingestion
* Maintain raw historical records

```text
CSV Dataset
     ↓
https://www.kaggle.com/datasets/ashishsahani/hospital-admissions-data/data?select=HDHI+Admission+data.csv
public.admissions
```

---

### Layer 2 — Data Quality & Cleansing Layer

The second layer performs critical transformations and validation.

Key processes include:

#### Date Standardization

Admission and discharge dates are converted from text into PostgreSQL DATE types.

#### Duplicate Detection

Patients with multiple admissions are identified using window functions and business rules.

```sql
ROW_NUMBER() OVER (
    PARTITION BY
        mrd_no,
        admission_date,
        discharge_date,
        admission_type
)
```

#### Clinical Validation

Records are filtered to remove:

* Invalid admission timelines
* Negative hospitalization durations
* Duplicate encounters
* Incomplete patient identifiers

---

### Layer 3 — Analytical Reporting View

A reusable analytical view was developed:

```sql
public.vw_admission
```

This view acts as a trusted reporting layer and centralizes business logic, 
ensuring that all reporting and dashboarding activities use the same validated dataset.

---

## Business Questions Addressed

### 1. ICU Resource Utilization

Which cardiovascular conditions consume the greatest proportion of ICU resources?

Metrics:

* Total admissions
* Average hospital stay
* Average ICU stay
* ICU utilization percentage

Business Impact:

Supports ICU staffing decisions, bed management, and resource planning.

---

### 2. Emergency vs Planned Admissions

How do emergency admissions differ from outpatient admissions in terms of patient hospitalization intensity?

Metrics:

* Admission volume
* Average length of stay
* ICU utilization
* Maximum hospitalization duration

Business Impact:

Improves operational planning and emergency department management.

---

### 3. Seasonal Hospitalization Trends

Are there identifiable seasonal patterns in cardiovascular admissions?

Metrics:

* Monthly admission volume
* Average monthly length of stay

Business Impact:

Supports forecasting, budgeting, and workforce planning.

---

### 4. Rural vs Urban Patient Analysis

Do rural patients experience different hospitalization outcomes than urban patients?

Metrics:

* Admission count
* Average hospital stay
* ICU utilization rates

Business Impact:

Supports population health analysis and healthcare accessibility planning.

---

## Advanced SQL Concepts Demonstrated

This project demonstrates several advanced SQL techniques used in production analytics environments:

* Common Table Expressions (CTEs)
* Window Functions
* Data Quality Validation Rules
* Analytical Views
* Conditional Logic (CASE Statements)
* Aggregate Functions
* Healthcare Data Transformation
* Duplicate Detection Frameworks

---

## Key Skills Demonstrated

### Data Engineering

* Data Modeling
* Data Ingestion
* ETL Design
* Data Cleansing
* Data Validation

### SQL Development

* PostgreSQL
* Complex Queries
* Window Functions
* View Design
* Performance-Oriented Querying

### Healthcare Analytics

* Clinical Data Analysis
* Hospital Operations Analytics
* ICU Utilization Analysis
* Outcome Monitoring
* Patient Flow Analytics

--------------------------------------------------
### Business Intelligence     we upload the dashboard later 
---------------------------------------------------
* KPI Development
* Trend Analysis
* Operational Reporting
* Decision Support Analytics

---

## Project Outcome

The final solution delivers a clean, validated, and analytics-ready healthcare dataset capable of supporting operational reporting and strategic decision-making. Through the implementation of a layered PostgreSQL architecture, the project demonstrates how raw clinical data can be transformed into trusted business intelligence assets suitable for healthcare analytics environments.
