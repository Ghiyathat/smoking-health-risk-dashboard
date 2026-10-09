# *Epidemiological Analysis of Smoking-Attributable Morbidity & Biomarkers*

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Dataset](https://img.shields.io/badge/Dataset-2%2C500_Patients-blue)](#dataset-overview)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An interactive, multi-dimensional Power BI clinical intelligence dashboard evaluating the epidemiological relationship between tobacco consumption habits (status, exposure years, daily intensity) and clinical pathology across major organ systems (Heart, Lungs, Liver, Kidneys, and Systemic/Human Body).

---

## 📌 Executive Summary

Tobacco use remains a leading preventable risk factor for cardiovascular disease, respiratory illness, and multi-organ damage. This project transforms a cross-sectional dataset of **2,500 patient records** into dynamic visual intelligence, isolating how smoking severity, hypertension profiles, and metabolic baselines (BMI, cholesterol) intersect with organ health outcomes.

## 🎯 Problem Statement & Clinical Objectives

Chronic exposure to tobacco combustive products induces systemic oxidative stress, endothelial dysfunction, and accelerated tissue degeneration across vital organ networks. However, clinical and public health data often report these factors in silos, masking how dose metrics—such as **Years of Smoking (YOS)** and **Cigarettes Per Day (CPD)**—interact with demographic vulnerabilities and cardiovascular biomarkers like hypertension and BMI.

### Primary Objectives
* **Isolate Organ-Specific Vulnerability:** Evaluate damage frequency across five targets: Heart, Lungs, Liver, Kidneys, and systemic Human Body.
* **Identify Dose-Response Inflection Points:** Contrast duration of exposure against daily cigarette volume across generational cohorts.
* **Facilitate Clinical Decision Support:** Provide health analysts and care teams with dynamic filtering to profile high-risk multimorbid patient segments.

### Target Stakeholders
* **Public Health Program Managers:** For tailoring smoking cessation campaigns to high-risk demographic clusters.
* **Clinical Risk Assessors & Epidemiologists:** For tracking how secondary risk factors (e.g., stage-specific BP risks) compound tobacco-induced organ pathology.

## 📖 Data Dictionary & Biomarker Definitions

The underlying schema comprises 2,500 patient records structured as follows:

| Field Name | Data Type | Description & Clinical Context |
| :--- | :--- | :--- |
| `Patient_ID` | Integer | Unique pseudo-anonymized subject identifier ($1\text{--}2500$). |
| `Age` / `Age_Group` | Integer / Category | Subject chronological age ($18\text{--}89$ years), binned into 10-year cohorts. |
| `Gender` | Text | Biological sex categorization (`Male`, `Female`). |
| `Smoking_Status` | Text | Tobacco habit classification (`Never`, `Current`, `Former`). |
| `Years_of_Smoking` | Integer | Cumulative lifetime exposure duration (YOS). |
| `Cigarettes_Per_Day`| Integer | Daily consumption intensity (CPD). |
| `Organ` | Text | Target anatomical system under evaluation (`Heart`, `Kidney`, `Liver`, `Lungs`, `Human Body`). |
| `Organ_Condition` | Text | Pathological status classified via clinical assessment (`Healthy`, `Damaged`). |
| `BMI` | Decimal | Body Mass Index ($\text{kg/m}^2$), evaluating metabolic baseline risk. |
| `BP_Risk` | Text | Blood pressure stratification categorized into `Normal`, `Low`, and `High` (Hypertensive). |
| `Cholesterol_Level`| Decimal | Total serum cholesterol profile ($\text{mg/dL}$). |

---

## 📊 Key Dashboard Features & Visualizations

| Visual Element | Metric / Dimension Analyzed | Analytical Objective |
| :--- | :--- | :--- |
| **KPI Metric Cards** | Total Patients, $\Delta$ Avg Age, $\Delta$ Avg BMI | Real-time population cohort sizing and physiological baseline variance. |
| **Interactive Slicers** | `Organ` (Heart, Kidney, Liver, Lungs, Systemic) & `Condition` (Healthy vs. Damaged) | Enables instant pathology drill-downs per anatomical site. |
| **Smoking Status Donut** | % Composition (`Never`, `Current`, `Former`) | Quantifies the distribution of tobacco exposure within filtered patient cohorts. |
| **Alluvial / Sankey Flow** | Smoking Status by Gender (`Male` vs. `Female`) | Maps exposure transitions and sex-disaggregated lifestyle patterns. |
| **Dual-Axis Exposure Curves** | Years of Smoking (YOS) vs. Cigarettes Per Day (CPD) | Evaluates dose-response intensity trends across structured age brackets. |
| **Stacked Risk Bars** | Blood Pressure Risk (`High`, `Low`, `Normal`) & Cholesterol | Highlights comorbid cardiovascular vulnerability across age cohorts. |

---

## 🛠️ Step-by-Step Dashboard Build Workflow

### 1. Data Pipeline & Power Query ETL
* **Data Ingestion:** Loaded raw clinical records (`health_dataset.csv`, 2,500 rows, 13 attributes).
* **Data Profiling & Quality Checks:** Checked column distributions, eliminated null values, verified data types (`Age`, `BMI`, `Cholesterol_Level` as decimals/integers).
* **Conditional Binning:** Segmented continuous age variables into standard demographic cohorts:
  $$\text{Age Group} = [18\text{--}28,\; 29\text{--}38,\; 39\text{--}48,\; 49\text{--}58,\; 59\text{--}68,\; 69+]$$

### 2. Analytical Data Modeling & DAX Measures
Implemented DAX calculations to dynamically calculate population counts, biomarker averages, and cross-cohort benchmark deltas:

```dax
-- Total Patient Volume in Selected Cohort
Total Patients = COUNTROWS('health_dataset')

-- Cohort Average Age & Baseline Population Comparison
Avg Age = AVERAGE('health_dataset'[Age])
Population Avg Age = CALCULATE(AVERAGE('health_dataset'[Age]), ALL('health_dataset'))
Delta Avg Age = [Avg Age] - [Population Avg Age]

-- Cohort Average BMI & Population Benchmark
Avg BMI = AVERAGE('health_dataset'[BMI])
Population Avg BMI = CALCULATE(AVERAGE('health_dataset'[BMI]), ALL('health_dataset'))
Delta Avg BMI = [Avg BMI] - [Population Avg BMI]

-- Organ Damage Proportion
Damaged Proportion = 
DIVIDE(
    CALCULATE(COUNTROWS('health_dataset'), 'health_dataset'[Organ_Condition] = "Damaged"),
    COUNTROWS('health_dataset'),
    0
)

```
<img width="518" height="283" alt="Screenshot 2026-10-09 083325" src="https://github.com/user-attachments/assets/63b43122-cb68-43c0-a51e-176c25014be7" />
```

## 💡 Public Health Implications & Recommendations

1. **Targeted Screening in Middle-Aged Cohorts:** Because daily consumption intensity (CPD) peaks in the $39\text{--}48$ demographic bracket while clinical damage accumulates exponentially in older cohorts ($50+$), targeted cessation interventions and diagnostic organ screens must be deployed before the 50-year threshold.
2. **Dual-Risk Cardiovascular Interventions:** With average BMI exceeding $30.0$ in cohorts presenting with heart damage, cessation programs must integrate lifestyle and metabolic management rather than isolating tobacco use alone.
3. **Former Smoker Monitoring:** Former smokers represent nearly $30\%$ of several organ-damaged subsets, underscoring that cessation reduces—but does not instantly eliminate—long-term residual pathological risk. Longitudinal organ surveillance remains essential post-quitting.

---

## 🏁 Conclusion

This dashboard bridges raw clinical survey metrics and actionable public health intelligence. By integrating dynamic cross-filtering between specific organ systems, dose-response measures, and cardiovascular biomarkers, the project demonstrates how modern business intelligence tools can diagnose epidemiological patterns, support risk stratification, and empower proactive preventative care strategies.
