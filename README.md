# Predicting Dissolved Ocean Oxygen from Multispectral Satellite Features

**SIADS 699 Capstone Project - Group 13**

This project develops machine learning models to predict surface dissolved oxygen concentrations from MODIS-Aqua satellite ocean color and thermal data, integrated with in-situ measurements from NOAA's World Ocean Database.

## Project Overview

Dissolved oxygen is a critical indicator of ocean health, but traditional measurement methods (ships and buoys) provide limited spatial and temporal coverage. This project demonstrates how satellite remote sensing combined with machine learning can expand ocean oxygen monitoring capabilities.

**Key Features:**
- Integration of NOAA World Ocean Database (WOD) with MODIS-Aqua satellite data
- Feature engineering from spectral reflectance, chlorophyll-a, POC, and sea surface temperature
- Multiple machine learning models (Linear Regression, Random Forest, LightGBM, XGBoost, Neural Networks)
- Temporal data splitting to prevent data leakage
- Hyperparameter optimization using TimeSeriesSplit cross-validation

## Repository Structure

```
.
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 1_Data_Collection_and_Formatting.ipynb
│   ├── 2_Data_Integration_and_Feature_Engineering.ipynb
│   ├── 3_Baseline_Models.ipynb
│   └── 4_Model_Development_Optimisation_and_Interpretation.ipynb
├── data/
│   └── README.md (data access instructions)
├── results/
│   ├── figures/
│   └── model_performance/
└── src/
    └── utils.py (helper functions)
```

## Data Access

### NOAA World Ocean Database (WOD)
- **Source:** https://www.ncei.noaa.gov/products/world-ocean-database
- **Access:** Publicly available, downloaded in NetCDF format
- **Data Used:** Ocean Station Data (OSD) subset, surface measurements (≤10m depth), 2002-2023
- **License:** Public domain (U.S. Government work)

### MODIS-Aqua Satellite Data
- **Source:** NASA Ocean Biology Processing Group (OB.DAAC)
- **Access:** Via Google Earth Engine (https://earthengine.google.com/)
- **Dataset:** `NASA/OCEANDATA/MODIS-Aqua/L3SMI` (Level-3 Standard Mapped Image, 4km resolution)
- **Variables Used:** 
  - Chlorophyll-a concentration (chlor_a)
  - Particulate organic carbon (poc)
  - Sea surface temperature (sst)
  - Remote sensing reflectance (Rrs_412 through Rrs_678, 10 bands)
- **Temporal Window:** ±3 days from ocean measurement date
- **License:** Public domain (NASA data policy)

**Note:** Raw data files are not included in this repository due to size constraints. Users must download data from the sources above or contact the project team for processed datasets.

## Setup Instructions

### Prerequisites
- Python 3.8 or higher
- Google Earth Engine account (for satellite data extraction)
- Jupyter Notebook or JupyterLab

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/ocean-oxygen-prediction.git
cd ocean-oxygen-prediction
```

2. **Create a virtual environment (recommended):**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install required packages:**
```bash
pip install -r requirements.txt
```

4. **Authenticate Google Earth Engine (for satellite data extraction):**
```bash
earthengine authenticate
```

## Running the Code

The analysis pipeline consists of four Jupyter notebooks that should be run sequentially:

### 1. Data Collection and Formatting
```bash
jupyter notebook notebooks/1_Data_Collection_and_Formatting.ipynb
```
**Purpose:** Download and format NOAA WOD ocean data
**Outputs:** `data/processed/formatted_ocean_data.csv`

### 2. Data Integration and Feature Engineering
```bash
jupyter notebook notebooks/2_Data_Integration_and_Feature_Engineering.ipynb
```
**Purpose:** Extract MODIS satellite data via Google Earth Engine and merge with ocean measurements
**Outputs:** 
- `data/processed/combined_dataset.csv`
- Training/validation/test splits (70/15/15, temporally ordered)

### 3. Baseline Models
```bash
jupyter notebook notebooks/3_Baseline_Models.ipynb
```
**Purpose:** Train baseline models with default hyperparameters
**Outputs:** Baseline performance metrics

### 4. Model Development and Optimization
```bash
jupyter notebook notebooks/4_Model_Development_Optimisation_and_Interpretation.ipynb
```
**Purpose:** Hyperparameter tuning and model selection
**Outputs:** 
- Optimized model performance metrics
- Feature importance analysis
- `results/best_model.pkl`

## Key Results

| Model | Validation RMSE | Validation R² | Validation MAE |
|-------|-----------------|---------------|----------------|
| Linear Regression | 42.28 | 0.542 | 26.04 |
| Elastic Net | 42.40 | 0.539 | 26.19 |
| Random Forest | 40.13 | 0.589 | 22.64 |
| LightGBM | 39.84 | 0.596 | 23.47 |
| XGBoost | 40.20 | 0.588 | 23.89 |
| Neural Network | 42.87 | 0.532 | 27.32 |

**Best Model:** LightGBM (Validation R² = 0.596, RMSE = 39.84 µmol/kg)

## Data Leakage Prevention

- **Temporal splitting:** Training (2002-2019), Validation (2020-2021), Testing (2022-2023)
- **Feature engineering:** Applied after data splitting using training set only
- **Hyperparameter tuning:** TimeSeriesSplit cross-validation on training data
- **Test set:** Reserved for final evaluation only (not used during development)

## Code Attribution

- **Satellite data extraction:** Adapted from Google Earth Engine Python API documentation
  - Source: https://developers.google.com/earth-engine/tutorials/community/intro-to-python-api
  - License: Apache 2.0
  
- **TimeSeriesSplit implementation:** scikit-learn library
  - Source: https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html
  - License: BSD 3-Clause

All machine learning model implementations use standard scikit-learn, XGBoost, and LightGBM APIs without modification.

## Requirements

See `requirements.txt` for complete list. Key dependencies:
- pandas >= 1.5.0
- numpy >= 1.23.0
- scikit-learn >= 1.2.0
- xgboost >= 1.7.0
- lightgbm >= 3.3.0
- earthengine-api >= 0.1.0
- jupyter >= 1.0.0

## Team

**University of Michigan School of Information - SIADS 699 Capstone**
- Team Member 1
- Team Member 2
- Team Member 3

## License

This project is licensed under the MIT License - see LICENSE file for details.

## Contact

For questions about data access or code usage, please open an issue on this repository or contact [your email].

## Acknowledgments

- NOAA National Centers for Environmental Information for World Ocean Database
- NASA Ocean Biology Processing Group for MODIS-Aqua data
- Google Earth Engine for data access platform
