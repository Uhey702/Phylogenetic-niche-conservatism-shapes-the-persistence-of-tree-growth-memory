# Phylogenetic niche conservatism shapes the persistence of tree growth memory

## Description of the data and file structure

This repository contains the processed datasets and source code underlying the analysis presented in the manuscript titled above. The study investigates global vegetation growth memory, phylogenetic constraints, and driving factors using tree-ring data and climate reanalysis datasets.

### Files and variables

#### File: 01_ITRDB_data_download_detrend.R

**Description:** Automates the download of raw tree-ring measurement files (`.rwl`) and metadata from the NOAA ITRDB FTP server. Performs standard dendrochronological detrending using the `dplR` package (Modified Negative Exponential method). Filters sites based on quality control criteria (e.g., series length, sample depth). Note: Users can skip this step if using the provided `Annual_Summary.zip` data. (**Language:** R)

#### File: 02_VAR_Model_Analysis_and_IRF.R

**Description:** Main analytical script for quantifying biological memory. Conducts Augmented Dickey-Fuller (ADF) tests for stationarity. Fits Vector Autoregression (VAR) models for each of the 2,466 sites using the data in `Annual_Summary.zip`. Calculates Impulse Response Functions (IRF) to determine the duration and intensity of vegetation growth legacy effects. (**Language:** R)

#### File: 03_Phylogenetic_Tree_Construction_and_Grouping.R

**Description:** Standardizes species names against The Plant List (TPL). Constructs a global phylogenetic tree for the study species using `V.PhyloMaker2`. Computes "Phylogenetic Depth" and classifies species into evolutionary groups (e.g., Ancient vs. Recent) via hierarchical clustering. Generates the input for `site_info_group_mapping.xlsx`. (**Language:** R)

#### File: 04_Phylogenetic_Group_Mapping_and_Climate_Analysis.R

**Description:** Visualizes the global distribution of sites and phylogenetic groups (Figure plotting). Reclassifies Köppen-Geiger climate zones. Produces statistical summaries and cumulative plots showing the distribution of legacy regimes across continents and climate zones. (**Language:** R)

#### File: 05_Driving_Factors_Analysis_SHAP_and_Path.py

**Description:** Performs the analysis of driving mechanisms using `Driving_TRW_yr_merged_depth` data. **(1)** **SHAP Analysis:** Trains a Random Forest classifier to predict memory duration and calculates\
**(2)** **Partial Correlation:** Calculates partial correlations between drivers and memory duration. \
**(3)** **Structural Equation Modeling (SEM):** Uses `semopy` to fit path models quantifying direct and indirect effects of climate and phylogeny on vegetation memory. (**Language:** Python)

#### File: Annual_Summary.zip

**Description:** This archive contains the primary time-series data used for the Vector Autoregression (VAR) modeling. Each CSV file corresponds to one of the 2,466 selected tree-ring sites. 

##### Variables

* **Year**: Time step (annual).
* **TRW**: Detrended and standardized Tree Ring Width indices (processed using the Modified Negative Exponential method).
* **Climate Variables**: Annual mean or cumulative climate metrics (e.g., Temperature, Precipitation, VPD) specific to the site location.

#### File: site_info_group_mapping.xlsx

**Description:** Master metadata table providing biological and geographical details for all 2,466 study sites.

##### Variables

* **Site_ID**: Unique numerical identifier for each site.
* **site_name**: Name of the site, including regional code (e.g., 'africa_dza004').
* **VI**: Vegetation Index type, here 'TRW' (Tree Ring Width).
* **variable**: Specific variable measured, typically same as VI.
* **duration**: Biological memory duration, indicating the length of time (in years) past climate influences current growth.
* **intensity**: Memory intensity, representing the strength of the lag effect.
* **ancient_group**: Broad phylogenetic grouping based on evolutionary history (e.g., 'AncientGroup_E_5').
* **family**: Taxonomic family of the species (e.g., 'Pinaceae').
* **genus**: Taxonomic genus of the species (e.g., 'Cedrus').
* **species**: Scientific name of the species.
* **Lon**: Longitude of the site.
* **Lat**: Latitude of the site.
* **Elevation**: Elevation of the site in meters.
* **Continent**: Continent where the site is located.
* **Country**: Country where the site is located.
* **type**: Climate type of the site (e.g., 'Arid').
* **yr_strat**: Start year of the data record.
* **yr_end**: End year of the data record.
* **yr_len**: Total length of the data record in years.
* **SOS_Month**: Start of Season Month.
* **EOS_Month**: End of Season Month.
* **GS_Months**: List of months in the growing season (comma-separated, e.g., '4,5,6...').
* **Num_GS_Months**: Number of months in the growing season.
* **lab**: Original laboratory or dataset code.
* **TMP_sd_gs_mean**: Standard deviation of growing season Temperature.
* **PRE_sd_gs_mean**: Standard deviation of growing season Precipitation.
* **SR_sd_gs_mean**: Standard deviation of growing season Solar Radiation.
* **CO2_sd_gs_mean**: Standard deviation of growing season Carbon Dioxide concentration.
* **VPD_sd_gs_mean**: Standard deviation of growing season Vapor Pressure Deficit.
* **TRW_mean**: Mean Tree Ring Width.
* **TMP_mean**: Mean Temperature.
* **PRE_mean**: Mean Precipitation.
* **SR_mean**: Mean Solar Radiation.
* **CO2_mean**: Mean Carbon Dioxide concentration.
* **VPD_mean**: Mean Vapor Pressure Deficit.
* **TRW_cv**: Coefficient of Variation for Tree Ring Width.
* **TMP_cv**: Coefficient of Variation for Temperature (climate variability metric).
* **PRE_cv**: Coefficient of Variation for Precipitation (climate variability metric).
* **SR_cv**: Coefficient of Variation for Solar Radiation (climate variability metric).
* **CO2_cv**: Coefficient of Variation for Carbon Dioxide concentration.
* **VPD_cv**: Coefficient of Variation for Vapor Pressure Deficit (climate variability metric).
* **species_for_matching**: Simplified species name used for matching.
* **phylo_depth**: Phylogenetic depth, representing evolutionary time depth.
* **branch_length**: Branch length on the phylogenetic tree.
* **sister_distance**: Evolutionary distance to the nearest sister species.
* **phylo_match_type**: Type of phylogenetic match (e.g., 'species_level').

#### File: Driving_TRW_yr_merged_depth.csv

**Description:** The aggregated dataset used for the driving factor analysis (Machine Learning and SEM). It integrates the calculated biological memory metrics with climate variability indices and phylogenetic traits.

##### Variables

Here is the updated variable description with the corrected definition for `lab`:

**File 1:** **`Driving_TRW_yr_merged_depth.csv`**

* **Site_ID**: Unique numerical identifier for each site.
* **site_name**: Name of the site, including regional code (e.g., 'africa_dza004').
* **VI**: Vegetation Index type, here 'TRW' (Tree Ring Width).
* **variable**: Specific variable measured, typically same as VI.
* **duration**: Biological memory duration, indicating the length of time (in years) past climate influences current growth.
* **intensity**: Memory intensity, representing the strength of the lag effect.
* **ancient_group**: Broad phylogenetic grouping based on evolutionary history (e.g., 'AncientGroup_E_5').
* **family**: Taxonomic family of the species (e.g., 'Pinaceae').
* **genus**: Taxonomic genus of the species (e.g., 'Cedrus').
* **species**: Scientific name of the species.
* **Lon**: Longitude of the site.
* **Lat**: Latitude of the site.
* **Elevation**: Elevation of the site in meters.
* **Continent**: Continent where the site is located.
* **Country**: Country where the site is located.
* **type**: Climate type of the site (e.g., 'Arid').
* **yr_strat**: Start year of the data record.
* **yr_end**: End year of the data record.
* **yr_len**: Total length of the data record in years.
* **SOS_Month**: Start of Season Month.
* **EOS_Month**: End of Season Month.
* **GS_Months**: List of months in the growing season (comma-separated, e.g., '4,5,6...').
* **Num_GS_Months**: Number of months in the growing season.
* **lab**: Raster grid ID corresponding to the site location.
* **TMP_sd_gs_mean**: Standard deviation of growing season Temperature.
* **PRE_sd_gs_mean**: Standard deviation of growing season Precipitation.
* **SR_sd_gs_mean**: Standard deviation of growing season Solar Radiation.
* **CO2_sd_gs_mean**: Standard deviation of growing season Carbon Dioxide concentration.
* **VPD_sd_gs_mean**: Standard deviation of growing season Vapor Pressure Deficit.
* **TRW_mean**: Mean Tree Ring Width.
* **TMP_mean**: Mean Temperature.
* **PRE_mean**: Mean Precipitation.
* **SR_mean**: Mean Solar Radiation.
* **CO2_mean**: Mean Carbon Dioxide concentration.
* **VPD_mean**: Mean Vapor Pressure Deficit.
* **TRW_cv**: Coefficient of Variation for Tree Ring Width.
* **TMP_cv**: Coefficient of Variation for Temperature (climate variability metric).
* **PRE_cv**: Coefficient of Variation for Precipitation (climate variability metric).
* **SR_cv**: Coefficient of Variation for Solar Radiation (climate variability metric).
* **CO2_cv**: Coefficient of Variation for Carbon Dioxide concentration.
* **VPD_cv**: Coefficient of Variation for Vapor Pressure Deficit (climate variability metric).
* **species_for_matching**: Simplified species name used for matching.
* **phylo_depth**: Phylogenetic depth, representing evolutionary time depth.
* **branch_length**: Branch length on the phylogenetic tree.
* **sister_distance**: Evolutionary distance to the nearest sister species.
* **phylo_match_type**: Type of phylogenetic match (e.g., 'species_level').

**File 2:** **`site_info_group_mapping.xlsx - Sheet 1.csv`**

* **species**: Scientific name of the species.
* **lab**: Raster grid ID corresponding to the site location.
* **site_name**: Unique identifier for the site.
* **yr_strat**: Start year of the data record.
* **yr_end**: End year of the data record.
* **yr_len**: Total length of the data record in years.
* **Continent**: Continent where the site is located.
* **Site_Code**: Short code for the site.
* **Country**: Country where the site is located.
* **Site_Name**: Full detailed name of the site.
* **Lon**: Longitude of the site.
* **Lat**: Latitude of the site.
* **Elevation**: Elevation of the site.
* **Tree_Species_Code**: Abbreviated code for the tree species (e.g., 'ABAL' for *Abies alba*).
* **Genus**: Genus name (capitalized).
* **Common_Name**: Common name of the tree species.
* **SOS_Month**: Start of Season Month.
* **EOS_Month**: End of Season Month.
* **GS_Months**: List of months in the growing season.
* **SOS_DOY**: Day of Year for the start of the growing season.
* **EOS_DOY**: Day of Year for the end of the growing season.
* **Num_GS_Months**: Number of months in the growing season.
* **Genus_type**: Genus type indicator (e.g., 'Yes' might indicate inclusion in analysis).
* **species_clean**: Cleaned species name (formatted for matching/plotting, e.g., using underscores).
* **genus**: Genus name (often lowercase or duplicate for matching).
* **family**: Taxonomic family name.
* **group**: Detailed phylogenetic subgroup (e.g., 'CutreeGroup_e_5') based on clustering.
* **ancient_group**: Major phylogenetic group (e.g., 'AncientGroup_E_5'), a broader classification category.

## Code/software

The analysis and data processing were performed using free and open-source software:

* **R** (version 4.4.3 or higher recommended)
* **Python** (version 3.12 or higher recommended)

## Access information

All observational data that support the findings of this study are available as follows. The tree-ring width data are available at [https://www.ncei.noaa.gov/products/paleoclimatology/tree-ring](https://www.ncei.noaa.gov/products/paleoclimatology/tree-ring). The temperature, precipitation, and vapor pressure deficit from the CRU v4.0.7 data are available at [https://crudata.uea.ac.uk/cru/data/hrg/](https://crudata.uea.ac.uk/cru/data/hrg/). The short-wave radiation flux data from ECMWF Reanalysis 5 are available at [https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels-monthly-means?tab=overview](https://cds.climate.copernicus.eu/cdsapp%23!/dataset/reanalysis-era5-single-levels-monthly-means?tab=overview). The historical and SSP245 scenario atmospheric CO₂ concentration data, derived from CMIP6, are available at [https://aims2.llnl.gov/search/cmip6/](https://aims2.llnl.gov/search/cmip6/). The elevation data from WorldClim 2.1 is available at [https://www.worldclim.org/data/worldclim21.html](https://www.worldclim.org/data/worldclim21.html). The world map of Köppen-Geiger climate classification was freely obtained from [http://koeppen-geiger.vu-wien.ac.at/present.htm](http://koeppen-geiger.vu-wien.ac.at/present.htm).
