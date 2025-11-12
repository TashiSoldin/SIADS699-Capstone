# Data Directory

This directory contains data files and instructions for accessing the datasets used in this project. Due to file size constraints, raw data files are not included in this repository.

## Directory Structure

```
Data/
├── README.md (this file)
├── NOAA WOD Dataset Acquisition.pdf (step-by-step download guide)
├── Data Splits/
│   ├── Unprocessed/
│   │   ├── Training Set.csv
│   │   ├── Validation Set.csv
│   │   └── Testing Set.csv
│   └── Processed/
│       ├── Training Set.csv
│       ├── Validation Set.csv
│       └── Testing Set.csv
└── MODIS/
    └── (batch-processed satellite data: subset_045000_050000.csv, etc.)
```

**Note:** The `Data Splits/` and `MODIS/` folders are created automatically by the notebooks when processing data. You do not need to create these manually.

---

## Data Sources

This project uses two primary data sources:

### 1. NOAA World Ocean Database (WOD)
In-situ ocean measurements including dissolved oxygen, temperature, salinity, and geographic/temporal metadata.

### 2. MODIS-Aqua Satellite Data
Remote sensing ocean color and thermal measurements including chlorophyll-a, particulate organic carbon, sea surface temperature, and spectral reflectance.

---

## Data Access Instructions

### Option 1: Download Original Data Sources (Recommended)

#### NOAA World Ocean Database (WOD)

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

#### MODIS-Aqua Satellite Data

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

### Option 2: Request Processed Datasets

For access to pre-processed combined datasets, contact the project team:

**Team Contacts:**
- nsoldin@umich.edu
- mansfire@umich.edu  
- sdharsha@umich.edu

**Available Files:**
- Combined ocean-satellite dataset: `combined_dataset.csv` (~500 MB)
- Pre-split training/validation/test sets

**Note:** Processed datasets may be subject to data sharing agreements. Please specify intended use when requesting access.

---

## Data Usage and Licensing

### NOAA World Ocean Database
- **License:** Public domain (U.S. Government work)
- **Citation:** Mishonov, A.V., T.P. Boyer, O.K. Baranova, et al. (2024). World Ocean Database 2023. NOAA National Centers for Environmental Information. https://doi.org/10.25921/v92s-y066
- **Usage:** Free to use and redistribute without restriction
- **Acknowledgment:** NOAA requests users notify them of publications via ncei.info@noaa.gov

### MODIS-Aqua Data
- **License:** Public domain (NASA Earth Science Data Policy)
- **Citation:** NASA Goddard Space Flight Center, Ocean Ecology Laboratory, Ocean Biology Processing Group. Moderate-resolution Imaging Spectroradiometer (MODIS) Aqua Ocean Color Data. NASA OB.DAAC, Greenbelt, MD, USA. https://doi.org/10.5067/AQUA/MODIS/L3M/CHL/2018
- **Usage:** Free to use and redistribute without restriction
- **Acknowledgment:** NASA requests acknowledgment of OB.DAAC in publications

**Data Policy:** Both datasets are openly available for research, education, and commercial use without restriction.

---

## File Size Reference

### Raw Data:
- NOAA WOD CSV (1990-2023): ~160 MB
- MODIS batch files (multiple subset_*.csv files): ~100-200 MB total

### Processed Data:
- Combined dataset: ~500 MB
- Training set (70%, 2002-2019): ~350 MB
- Validation set (15%, 2020-2021): ~75 MB
- Testing set (15%, 2022-2023): ~75 MB

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

## Troubleshooting

### NOAA WOD Download Issues:

**Problem:** Email with download link not received  
**Solution:** Check spam folder; large requests may take 2-4 hours to process

**Problem:** CSV file appears corrupted or incomplete  
**Solution:** Re-download from link in email; ensure stable internet connection

**Problem:** WODSelect interface times out  
**Solution:** Reduce date range or geographic extent; submit multiple smaller requests

### Google Earth Engine Issues:

**Problem:** `earthengine` command not found  
**Solution:** Ensure earthengine-api is installed: `pip install earthengine-api`

**Problem:** Authentication failure  
**Solution:** Run `earthengine authenticate` and follow browser prompts

**Problem:** Quota exceeded error  
**Solution:** Google Earth Engine has usage limits; wait 24 hours or reduce extraction scope

**Problem:** MODIS data extraction slow  
**Solution:** Expected behavior for large requests; data is extracted in batches and saved as separate CSV files in `MODIS/` folder. Processing ~45,000 measurements may take several hours.

---

## Additional Resources

### NOAA WOD Documentation:
- User Manual: https://www.ncei.noaa.gov/sites/default/files/2021-03/WOD-UserManual.pdf
- Data Format Specifications: https://www.ncei.noaa.gov/products/world-ocean-database
- Quality Control Flags: Described in WOD User Manual Section 4

### MODIS-Aqua Documentation:
- Algorithm Theoretical Basis Documents: https://oceancolor.gsfc.nasa.gov/resources/atbd/
- Product User Guide: https://oceancolor.gsfc.nasa.gov/docs/format/
- Data Processing Levels: https://oceancolor.gsfc.nasa.gov/data/overview/

### Google Earth Engine:
- Python API Tutorial: https://developers.google.com/earth-engine/tutorials/community/intro-to-python-api
- MODIS-Aqua Dataset Catalog: https://developers.google.com/earth-engine/datasets/catalog/NASA_OCEANDATA_MODIS-Aqua_L3SMI

---

## Contact

For questions about data access or processing:
- nsoldin@umich.edu
- mansfire@umich.edu
- sdharsha@umich.edu

For questions about NOAA WOD data: ncei.info@noaa.gov  
For questions about MODIS data: https://oceancolor.gsfc.nasa.gov/forum/
