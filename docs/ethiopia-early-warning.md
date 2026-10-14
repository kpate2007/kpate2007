# Early Warning for Childhood Disease Surges in Ethiopia

**Geospatial data engineering, machine learning, and public health | Research in progress**

I'm a research assistant working with **Dr. Rishi Mediratta** at Stanford on a collaborative effort to develop an early-warning system for **pneumonia, malaria, and diarrheal disease among children under five in Ethiopia**. The goal is to investigate whether historical health surveillance, climate and environmental conditions, and population-level risk factors can help anticipate increases in disease burden **one to four weeks ahead**.

The long-term aim is practical: give public health teams more time to prepare supplies, staffing, and care capacity in places likely to experience a surge. Potential responses include positioning oxygen and antibiotics for pneumonia, antimalarials, oral rehydration supplies, and other resources before facilities become overwhelmed.

This research involves collaborators at **Stanford University and Addis Ababa University**, alongside stakeholders at **Ethiopia's Ministry of Health (MoH) and Ethiopian Public Health Institute (EPHI)**. The system is being developed and evaluated as research; **it is not a deployed or clinically validated public health alert system**.

## The problem

Routine surveillance can reveal rising case counts after facilities have already begun seeing more patients. Reporting delays, inconsistent data completeness, and geographic differences in access to care complicate early detection, especially in rural areas.

Our research question is whether combining epidemiological history with signals such as rainfall, temperature, air quality, and local vulnerability can improve the timeliness and reliability of population-level forecasts. The intended unit of analysis is a **woreda (district) × week × disease**; forecasts target upcoming disease burden or surge risk, rather than individual patient diagnoses.

## Data sources

The project combines, or is designed to incorporate, several complementary sources. **Availability and permission differ by source**; not every source listed below is already integrated into a validated operational model.

| Source | What it contributes | Role in the research |
| --- | --- | --- |
| **Ethiopian health surveillance: DHIS2 and EPHI** | Aggregated reports of childhood pneumonia, malaria, and diarrhea, including weekly reportable-disease indicators where available | Intended primary disease-outcome and historical-surveillance inputs; access and use are governed by research partners |
| **Ethiopian Meteorological Institute (EMI)** | Ground-station observations of precipitation and other weather conditions | Local environmental observations and comparisons with satellite-derived estimates |
| **Demographic and Health Surveys (DHS)** | Child nutrition, vaccination, caregiver education, household conditions, sanitation, and illness indicators | Survey-derived contextual factors and research outcomes, handled under applicable DHS access terms |
| **Satellite precipitation: CHIRPS and NASA IMERG** | Rainfall totals, variability, and extreme precipitation | Potential early environmental signals, including lagged rainfall patterns |
| **Weather and atmospheric reanalysis: ERA5-Land, NASA MERRA-2, and CAMS** | Temperature, humidity, soil moisture, and air-pollution or aerosol proxies | Environmental feature engineering and comparisons of source coverage and latency |
| **Earth observation and geography: MODIS NDVI, elevation, administrative boundaries, and population estimates** | Vegetation, terrain, woreda boundaries, and estimated under-five populations | Geographic harmonization, environmental context, and population denominators |
| **National health call-center information** | Aggregated symptom- or illness-related call trends, if available under an agreement | Proposed additional surveillance signal; not represented as an already validated input |

DHS provides **infrequent, survey-based context**, not live weekly measurements. Similarly, openly accessible satellite products do not make the full, integrated research dataset open for redistribution.

## What I'm building

My technical work spans two connected workstreams:

### 1. Data integration and feature engineering

The current Python pipeline assembles data with different geographic and temporal resolutions into a common analysis structure. This work includes:

- Building **woreda-level, weekly datasets** that align disease, geography, population, survey, and environmental variables.
- Integrating **six satellite, climate, and atmospheric datasets**—CHIRPS, IMERG, ERA5-Land, MERRA-2, CAMS, and MODIS NDVI—using **Google Earth Engine** and cloud-based pipelines, alongside DHS and disease-outcome data.
- Implementing spatial joins, regional DHS aggregations, time alignment, lagged features, and checks for missing or inconsistent observations.
- Separating **missing reports from actual zero case counts**, a distinction that matters for disease surveillance.
- Developing **SLURM-based batch-processing workflows on Stanford's Sherlock HPC cluster** to parallelize multi-year environmental-data extraction and feature generation.
- Preparing the datasets for subsequent disease-specific modeling and evaluation.

### 2. Forecasting and model evaluation

An earlier research framework explores and benchmarks statistical and machine-learning approaches on **synthetic disease-surveillance data**. Implemented experimental model families include:

- Simple and seasonal baselines, along with **SARIMAX** time-series models.
- **Logistic regression, random forests, and XGBoost**.
- **Prophet**, generalized additive approaches, and distributed-lag models.
- Additional exploratory anomaly-detection and factor-model approaches.

The newer pipeline is also beginning model development, including logistic-regression experiments linking lagged environmental features to DHS-reported symptom indicators. The modeling work uses **lagged epidemiological and environmental features**, with **walk-forward validation** for experimental **one- to four-week-ahead** forecasts. The broader research plan includes comparisons across regions and seasons, and attention to reporting delays and model calibration.

**Important research distinction:** the earlier modeling benchmarks use **synthetic or dummy disease outcomes** to develop and test methods. Their scores are **not evidence of real-world predictive performance in Ethiopia**. Models must be evaluated using appropriately governed, real surveillance data before operational claims can be made.

## Technical approach

```text
Health surveillance + climate + DHS + geography/population
                         |
                         v
          Data validation and spatial alignment
                         |
                         v
             Woreda x week x disease panel
                         |
                         v
       Lagged features + missingness assessment
                         |
                         v
           Model development and backtesting
                         |
                         v
         Experimental 1–4 week ahead forecasts
                         |
                         v
      Future evaluation with public health partners
```

**Tools and methods:** Python, pandas, GeoPandas, scikit-learn, **Random Forest, XGBoost, SARIMAX, Prophet**, Google Earth Engine, Google Cloud, geospatial feature engineering, and time-series evaluation. Larger data-preparation workloads use **Stanford Sherlock (SLURM)**.

## Project status and code availability

**Research in progress.** The data-processing pipeline and model prototypes are under active development. Experimental forecasting and synthetic-data benchmarking have been conducted, but an operationally validated system has **not** been launched for Ministry use.

**Why isn't the full source code or dataset public?** This is collaborative research involving government health-surveillance systems, registered or licensed survey data, partner-provided observations, and internal research infrastructure. The working repositories and integrated research datasets are kept private while data-use permissions, ethics and governance requirements, partner review, and validation are addressed. Keeping them private avoids presenting unfinished methods or synthetic results as a deployed public health product.

Some underlying climate and satellite datasets are publicly available from their original providers. That is separate from permission to release the project's integrated datasets, partner-provided observations, or internal research artifacts. Approved methods or code may be shared later where collaborators and data-use terms permit.

**This public page is a project overview, not a release of restricted data or production software.**

## Research support

I work with **Dr. Rishi Mediratta**, who mentors my research at Stanford. The project is supported by the **Stanford Center for Innovation in Global Health**, the **Stanford Center for Digital Health**, and the **Stanford Institute for Human-Centered Artificial Intelligence (HAI)**, alongside **Google Cloud** support through the Stanford HAI Google Cloud Credits program. I'm grateful to the Stanford, Addis Ababa University, and Ethiopian public-health collaborators contributing scientific, clinical, and local implementation expertise.

---

*Project overview by Kushal Patel. Research methods and scope may evolve as the study progresses.*