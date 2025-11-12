# Predicting Dissolved Ocean Oxygen from Multispectral Satellite Features

**SIADS 699 Capstone Project - Group 13**

This project develops machine learning models to predict surface dissolved oxygen concentrations from MODIS-Aqua satellite data, integrated with in-situ measurements from NOAA's World Ocean Database.

## Project Overview

Dissolved oxygen is a critical indicator of ocean health, but traditional measurement methods (ships and buoys) provide limited spatial and temporal coverage. This project demonstrates how satellite remote sensing combined with machine learning can expand ocean oxygen monitoring capabilities.

## Repository Structure

```
.
├── README.md
├── requirements.txt
├── Notebooks/
│   ├── 1. Data Ingestion, Preparation and Splitting.ipynb
│   ├── 2. Exploratory Data Analysis and Feature Engineering.ipynb
│   ├── 3. Baseline Models.ipynb
│   ├── 4. Model Development, Optimisation, and Interpretation.ipynb
│   └── 5. Final Evaluation.ipynb
├── Data/
│   ├── Data Splits/
│       ├── Unprocessed/
│       └── Processed/
│   ├── MODIS/
│   ├── README.md (data access instructions)
│   └── NOAA WOD Dataset Acquisition.pdf  
└── Results/
    ├── figures/
    └── Final Report
```

## Data Access
This project uses two primary data sources:
1. NOAA World Ocean Database (WOD) - In-situ ocean measurements (public domain)
2. MODIS-Aqua Satellite Data - Ocean color and thermal data via Google Earth Engine (public domain)

For detailed download instructions, data specifications, and licensing information, see `Data/README.md`
**Note:** Raw data files are not included in this repository due to size constraints. Users must download data from the sources above or contact the project team for processed datasets.

## Setup Instructions
### Prerequisites
- Python 3.8 or higher
- Google Earth Engine account (for satellite data extraction)
- Jupyter Notebook or JupyterLab

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/TashiSoldin/SIADS699-Capstone.git
cd SIADS699-Capstone
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

The analysis pipeline consists of five Jupyter notebooks that should be run sequentially:

### 1. Data Ingestion, Preparation and Splitting
```bash
jupyter notebook Notebooks/1. Data Ingestion, Preparation and Splitting.ipynb
```
**Purpose:** Ingest and format NOAA WOD ocean data, extract MODIS satellite data, process, merge and split data
**Outputs:** `Data/Data Splits/Unprocessed/Training Set.csv`, `Data/Data Splits/Unprocessed/Validation Set.csv`, `Data/Data Splits/Unprocessed/Testing Set.csv`

### 2. Exploratory Data Analysis and Feature Engineering
```bash
jupyter notebook Notebooks/2. Exploratory Data Analysis and Feature Engineering.ipynb
```
**Purpose:** Perform EDA on training dataset and perform feature engineering
**Outputs:** `Data/Data Splits/Processed/Training Set.csv`, `Data/Data Splits/Processed/Validation Set.csv`, `Data/Data Splits/Processed/Testing Set.csv`

### 3. Baseline Models
```bash
jupyter notebook Notebooks/3. Baseline Models.ipynb
```
**Purpose:** Train baseline models with default hyperparameters
**Outputs:** Baseline performance metrics

### 4. Model Development and Optimization
```bash
jupyter notebook Notebooks/4. Model Development, Optimisation, and Interpretation.ipynb
```
**Purpose:** Hyperparameter tuning and model selection
**Outputs:** 
- Optimized model performance metrics
- Feature importance analysis

### 5. Final Evaluation
```bash
jupyter notebook Notebooks/5. Final Evaluation.ipynb
```
**Purpose:** Evaluate best model on held-out test set
**Outputs:** 
- Final test set performance metrics
- Model evaluation visualizations
- `Results/best_model.pkl`

## Key Results
**Best Model:** LightGBM (Validation R² = 0.596, RMSE = 39.84 µmol/kg)

## Data Leakage Prevention

- **Temporal splitting:** Training (2002-2019), Validation (2020-2021), Testing (2022-2023)
- **Feature engineering:** Applied after data splitting using training set only
- **Hyperparameter tuning:** TimeSeriesSplit cross-validation on training data
- **Test set:** Reserved for final evaluation only (not used during development)

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
- Natasha Soldin
- Ryan Mansfield
- Dharshana Somasunderam

## License

This project is licensed under the MIT License - see LICENSE file for details.

## Contact

For questions about data access or code usage, please contact nsoldin@umich.edu, mansfire@umich.edu or sdharsha@umich.edu

## Acknowledgments
This project uses data from NOAA National Centers for Environmental Information and NASA Ocean Biology Processing Group.
**NOAA World Ocean Database:**
Mishonov, A.V., T.P. Boyer, O.K. Baranova, et al. (2024). World Ocean Database 2023. NOAA National Centers for Environmental Information. https://doi.org/10.25921/v92s-y066
**NASA MODIS-Aqua Ocean Color Data:**
NASA Goddard Space Flight Center, Ocean Ecology Laboratory, Ocean Biology Processing Group. Moderate-resolution Imaging Spectroradiometer (MODIS) Aqua Ocean Color Data. NASA OB.DAAC, Greenbelt, MD, USA. https://doi.org/10.5067/AQUA/MODIS/L3M/CHL/2018

We acknowledge NOAA NCEI and NASA OB.DAAC for providing open access to these valuable datasets, and Google Earth Engine for facilitating satellite data access.
We thank the lecturers and instructional team at the University of Michigan for their help and guidance.
