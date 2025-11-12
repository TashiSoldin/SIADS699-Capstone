# Data Directory

This directory contains data files and instructions for accessing the datasets used in this project. Due to file size constraints, raw data files are not included in this repository.

## Directory Structure

```
Data/
├── README.md
├── NOAA WOD Dataset Acquisition.pdf
├── Data Splits/
│   ├── Unprocessed/
│   └── Processed/
└── MODIS/
```

### Folder Descriptions:
**Unprocessed/:** Contains the raw combined ocean-satellite data after temporal splitting but before feature engineering. Created by Notebook 1.
**Processed/:** Contains data with engineered features ready for model training. Created by Notebook 2.
**MODIS/:** Stores intermediate batch files of satellite data extractions from Google Earth Engine. Created by Notebook 1.

**Note:** The `Data Splits/` and `MODIS/` folders must be created manually before running the notebooks. The notebooks will automatically populate these folders with processed data files.

---

## Data Sources

This project uses two primary data sources:

### 1. NOAA World Ocean Database (WOD)
In-situ ocean measurements including dissolved oxygen and geographic/temporal metadata.

### 2. MODIS-Aqua Satellite Data
Remote sensing ocean color and thermal measurements including chlorophyll-a, particulate organic carbon, sea surface temperature, spectral reflectance and geographic/temporal metadata.

---

## Data Access Instructions

### Download NOAA World Ocean Database (WOD) from Portal

**Quick Start:**  
A detailed step-by-step guide with screenshots is provided in `NOAA WOD Dataset Acquisition.pdf` in this directory.

**Summary of Steps:**

1. **Navigate to NOAA WOD website:**  
   https://www.ncei.noaa.gov/products/world-ocean-database

2. **Launch WODSelect tool:**  
   https://www.ncei.noaa.gov/access/world-ocean-database-select/dbsearch.html

3. **Configure search criteria:**
   - **Geographic Coordinates:** No restriction (global coverage)
   - **Observation Dates:** 1990-01-01 to 2023-12-31
   - **Dataset:** Ocean Station Data (OSD) only
   - **Measured Variables:** Oxygen (required)

4. **Select output format:**
   - Format: Comma Delimited Value (CSV)
   - Output: Standard output
   - Depth Level: Observed level data

5. **Submit extraction request:**
   - Provide email address
   - Wait for email notification (~2 hours for large requests)
   - Download zip file from link in email

6. **Extract files:**
   - Unzip downloaded file
   - CSV file will be approximately 160 MB (uncompressed)

**Expected Output:**  
- File: `ocldb[timestamp].csv`
- Size: ~160 MB
- Records: ~282,000 casts (as of 2024)

---

### Extract Corresponding MODIS-Aqua Satellite Data

MODIS-Aqua data is extracted programmatically using Google Earth Engine in Notebook 1. **No manual download is required.**

**Requirements:**
- Google Earth Engine account (free): https://earthengine.google.com/signup/
- Authentication via `earthengine authenticate` command

**Data Accessed:**
- **Dataset:** `NASA/OCEANDATA/MODIS-Aqua/L3SMI`
- **Product:** Level-3 Standard Mapped Image (daily composites, 4km resolution)
- **Variables:**
  - Chlorophyll-a concentration (chlor_a) - mg/m³
  - Particulate organic carbon (poc) - mg/m³
  - Sea surface temperature (sst) - °C
  - Remote sensing reflectance (Rrs_412 through Rrs_678) - sr⁻¹ at 10 wavelengths
- **Temporal Matching:** ±3 day window from each ocean measurement date
- **Spatial Matching:** Nearest pixel to measurement location

**Processing:**  
Notebook 1 extracts MODIS data in batches to avoid memory issues and API limits. Satellite data for groups of ~5,000 ocean measurements are saved as separate CSV files in the `MODIS/` folder (e.g., `subset_045000_050000.csv`, `subset_050000_055000.csv`). These batch files are then merged with ocean measurements in subsequent processing steps.

---

## Data Usage and Licensing

### NOAA World Ocean Database
- **License:** Public domain (U.S. Government work)
- **Citation:** Mishonov, A.V., T.P. Boyer, O.K. Baranova, et al. (2024). World Ocean Database 2023. NOAA National Centers for Environmental Information. https://doi.org/10.25921/v92s-y066
- **Usage:** Free to use and redistribute without restriction
- **Acknowledgment:** NOAA requests users notify them of publications via ncei.info@noaa.gov

### MODIS-Aqua Data
- **License:** Public domain (NASA Earth Science Data Policy)
- **Citation:** NASA Goddard Space Flight Center, Ocean Ecology Laboratory, Ocean Biology Processing Group. Moderate-resolution Imaging Spectroradiometer (MODIS) Aqua Ocean Color Data. NASA OB.DAAC, Greenbelt, MD, USA. Accessed via Google Earth Engine.
- **Usage:** Free to use and redistribute without restriction
- **Acknowledgment:** NASA requests acknowledgment of OB.DAAC in publications

**Data Policy:** Both datasets are openly available for research, education, and commercial use without restriction.

---

## File Size Reference

### Raw Data:
- NOAA WOD CSV (1990-2023): ~490 MB
- MODIS batch files (multiple subset_*.csv files): ~400-900 KB each

### Processed Data:
- Combined dataset: ~14.1 MB
- Data Splits/Processed/Training set (70%, 2002-2019): ~13.5 MB
- Data Splits/Processed/Validation set (15%, 2020-2021): ~3 MB
- Data Splits/Processed/Testing set (15%, 2022-2023): ~3 MB

---

## Data Quality and Processing Notes

### NOAA WOD Data:
- **Temporal range:** 1990-2023 (filtered to align with MODIS-Aqua availability for 2002-2023 analysis)
- **Spatial coverage:** Global ocean, all geographic regions
- **Depth filter:** Surface measurements only (0-10 meters depth)
- **Quality control:** WOD quality flags applied (flag values 0-2 accepted)
- **Oxygen range:** Filtered to physically realistic values (0-500 µmol/kg)
- **Missing values:** Records with missing oxygen measurements removed

### MODIS-Aqua Data:
- **Temporal range:** July 2002 - December 2023
- **Spatial resolution:** 4 km (at equator)
- **Temporal resolution:** Daily L3 composites
- **Matching criteria:** ±3 day window, nearest pixel within ~10 km
- **Cloud masking:** Applied by MODIS L3 processing
- **Missing data:** Occurs due to cloud cover, sun glint, or sensor issues
- **Quality flags:** NASA OB.DAAC quality control applied

### Data Integration:
- **Matching method:** Spatiotemporal nearest-neighbor
- **Records matched:** ~45,000 ocean measurements successfully matched with satellite data
- **Unmatched records:** Discarded (primarily due to cloud cover or temporal gaps)
- **Temporal splits:** Strictly chronological to prevent data leakage
  - Training: 2002-2019 (70%)
  - Validation: 2020-2021 (15%)  
  - Testing: 2022-2023 (15%)

---

## Additional Resources

### NOAA WOD Documentation:
- User Manual: https://www.ncei.noaa.gov/data/oceans/woa/WOD/DOC/wodreadme.pdf
- Quality Control Flags: Described in WOD User Manual Section 4

### MODIS-Aqua Documentation:
- Google Earth Enginer Ocean Color SMI: https://developers.google.com/earth-engine/datasets/catalog/NASA_OCEANDATA_MODIS-Aqua_L3SMI
- Data Processing Levels: https://www.earthdata.nasa.gov/learn/earth-observation-data-basics/data-processing-levels

