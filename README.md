# Drug Related Mortality Clustering

[![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=fff)](#)
[![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=fff)](#)
[![NumPy](https://img.shields.io/badge/NumPy-4DABCF?logo=numpy&logoColor=fff)](#)
[![Scikit-learn](https://img.shields.io/badge/-scikit--learn-%23F7931E?logo=scikit-learn&logoColor=white)](#)

## Project Overview
Can unsupervised clustering of accidental drug related death data identify distinct profiles, and are there different patterns of medical comorbidities and prescribed medication use within them?

This project clusters 13,000 accidental drug related deaths from Connecticut (2012-2024) by drug co-occurrence patterns to identify distinct overdose profiles. The resulting clusters are then profiled against medication classes created from cause of death data, medical comorbidities, demographics, and year.

## Dataset
- **Source:** [Connecticut Office of the Chief Medical Examiner via data.ct.gov](https://data.ct.gov/Health-and-Human-Services/Accidental-Drug-Related-Deaths-2012-2024/rybz-nyjw/about_data)
- **Date Range:** 2012-2024
- **Records:** 12,961 after cleaning (Raw dataset: 12,963)
- **Features:** 14 binary drug indicators used for clustering, comorbidities and medication classes created from free text columns in the original dataset and demographics used for post-cluster profiling

## Machine Learning Pipeline
- Text parsing of free-text fields (Other, Other Opioid, Cause of Death, Other Significant Conditions) to standardize drug names and extract medication classes and comorbid conditions.
- Feature engineering: binary comorbidity flags from Other Significant Conditions, COD medication class indicators from Cause of Death
- Evaluation of four feature sets, two using only original drug columns and two including created med classes and comorbid conditions, across K-Medoids and K-Modes algorithms
- Feature set selection based on silhouette scores and cluster interpretability
- Final model: K-Medoids (cityblock distance) with k=10 on the original drug columns

## Cluster Profiles
| Cluster | n | Defining Drugs |
|---|---|---|
| Cocaine & Fentanyl | 2,266 | 100% Cocaine, 100% Fentanyl |
| Fentanyl | 2,071 | 100% Fentanyl |
| Heroin & Fentanyl | 1,912 | 100% Heroin, 100% Fentanyl |
| Heroin | 1,561 | 100% Heroin |
| Cocaine | 947 | 100% Cocaine |
| Cocaine Ethanol Fentanyl | 935 | 100% Cocaine, 100% Ethanol, 100% Fentanyl |
| Mixed Low Ethanol & RX Opioids | 893 | 33% Oxycodone, 29% Ethanol, 24% Methadone |
| Ethanol & Fentanyl | 819 | 100% Ethanol, 100% Fentanyl |
| Benzodiazepines & Fentanyl | 807 | 100% Benzodiazepine, 100% Fentanyl |
| Benzodiazepines & RX Opioids | 750 | 100% Benzodiazepine, 31% Oxycodone, 27% Methadone |

### Prescribed Medications & Comorbidities
- The two benzodiazepine/prescription opioid clusters showed significantly higher rates of prescribed medications in cause of death compared to street-drug clusters
- Medical comorbidities showed minimal differentiation across clusters overall, likely due to low reporting rates in the free-text fields. Cardiovascular disease was the exception, elevated in the cocaine-only cluster (16%) and prescription opioid clusters (8-12%) compared to heroin clusters (2%)

### Trends Over Time
- Though year was not used as a clustering feature, plotting clusters over time revealed they align with the known phases of the [opioid crisis](https://www.congress.gov/crs-product/IF12260): prescription opioid and heroin-dominant profiles concentrated in 2012-2015 and fentanyl-involved clusters dominating by 2017
- Total overdose deaths nearly quadrupled from ~350 in 2012 to ~1,500 in 2021 before declining

### Demographics
- Age showed minimal differentiation across clusters, with medians ranging from 39-51
- Cocaine related clusters had the most racial diversity while the heroin and prescription opioids clusters were mostly White. 

## Limitations
- Comorbidity and prescribed medication data came from free-text fields. The low rates across most clusters may reflect underreporting rather than true absence.

## How To Run
```bash
git clone https://github.com/Sarah-Eden/drug-related-mortality-clustering.git
cd drug-related-mortality-clustering
pip install -r requirements.txt
jupyter notebook drug-related-mortality-clustering.ipynb
```
Data should be placed in a `data/` directory. Source data is available from [Connecticut Open Data](https://data.ct.gov/Health-and-Human-Services/Accidental-Drug-Related-Deaths-2012-2024/rybz-nyjw/about_data).