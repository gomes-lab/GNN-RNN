# USCrop: County-level crop yield, weather, and soil data for the United States (1981-2020)

This readme file was generated on 2025-08-15 by Joshua Fan

# GENERAL INFORMATION

* Title of Dataset: USCrop: County-level crop yield, weather, and soil data for the United States (1981-2020)

This dataset contains crop yield, weather, soil, and crop progress information for US counties in 1981-2020. Specifically, each row contains data for a unique county and year. There are two versions: one with weekly weather (temporal) data and one with daily weather data. 

## Author/Principal Investigator Information
Name: Joshua Fan  
ORCID: 0000-0002-8525-0222  
Institution: Cornell University  
Address: 344 Gates Hall, Cornell University, Ithaca NY 14853  
Email: jyf6@cornell.edu  

## Author/Associate or Co-investigator Information
Name: Zhiyun Li  
ORCID: 0000-0001-8467-6240  
Institution: UCLA Anderson School of Business  
Email: zhiyun.li@anderson.ucla.edu   

* Date of data collection: Data processed 2021-02 to 2021-06, comes from 1981-2020
* Geographic location of data collection: Contiguous United States
* Information about funding sources that supported the collection of the data: NSF NRT Digital Plant Science Fellowship, USDA Cooperative Agreement 58-6000-9-0041, and USDA NIFA Hatch Project 1017421.


# SHARING/ACCESS INFORMATION

* Licenses/restrictions placed on the data: eCommons Deposit License
* Links to publications that cite or use the data:

		Fan et al. 2022, [A GNN-RNN Approach for Harnessing Geospatial and Temporal Information: Application to Crop Yield Prediction](https://ojs.aaai.org/index.php/AAAI/article/view/21444/21193). In <em>Proceedings of the AAAI Conference on Artificial Intelligence (AAAI-22)</em>, AI for Social Impact track, 11873-11881.

* Was data derived from another source? Yes.
	* If yes, list source(s): 
	
			Daly, C.; and Bryant, K. 2017. The PRISM climate and weather system: an introduction. Northwest Alliance for Computational Science and Engineering. Oregon State University, Corvallis, USA.

			Xia, Y.; Mitchell, K.; Ek, M.; Sheffield, J.; Cosgrove, B.; Wood, E.; Luo, L.; Alonge, C.; Wei, H.; Meng, J.; et al. 2012. Continental-scale water and energy flux analysis and validation for the North American Land Data Assimilation System project phase 2 (NLDAS-2): 1. Intercomparison and application of model products. Journal of Geophysical Research: Atmospheres, 117(D3)

			Soil Survey Staff. 2020. Gridded Soil Survey Geographic (gSSURGO) Database for the Conterminous United States.

			USDA. 2013. National Agricultural Statistics Service. United States Department of Agriculture.

* Recommended citation for this dataset: 

		Fan et al. 2022, [A GNN-RNN Approach for Harnessing Geospatial and Temporal Information: Application to Crop Yield Prediction](https://ojs.aaai.org/index.php/AAAI/article/view/21444/21193).


# DATA & FILE OVERVIEW

## File List: *list all files (or folders, as appropriate for dataset organization) contained in the dataset, with a brief description*

- `Fan_USCrop_DatasetWeekly_20250815.npz` contains raw data, with averaged weather/temporal data for each week in the year
- `Fan_USCrop_ColumnNamesWeekly_20250815.csv` contains column names and indices for the weekly dataset
- `Fan_USCrop_DatasetDaily_20250815.npz` contains raw data, with weather/temporal data for each day in the year
- `Fan_USCrop_ColumnNamesDaily_20250815.csv` contains column names and indices for the daily dataset

* Are there multiple versions of the dataset? Yes.
	* If yes, name of file(s) that was updated: `combined_dataset_weekly.npz` renamed to `Fan_USCrop_DatasetWeekly_20250815.npz`, `column_names.csv` renamed to `Fan_USCrop_ColumnNamesWeekly_20250815.csv`, `README_USCrop.md`  renamed to `Fan_USCrop_README_20250815.md`. Also added `Fan_USCrop_DatasetDaily_20250815.npz`, `Fan_USCrop_ColumnNamesDaily_20250815.csv`
	* Why was the file updated? Weekly .npz dataset was changed to use 32-bit numbers instead of 64-bit, reducing file size by half. Daily dataset was added. 
	* When was the file updated? 15 August 2025


# METHODOLOGICAL INFORMATION

## Description of methods used for collection/generation of data: 

See Appendix to the [GNN-RNN paper](https://arxiv.org/pdf/2111.08900) for information on how the data was collected and processed.


## Methods for processing the data: 

Variables that were originally gridded are spatially aggregated to the county level, using a weighted average (where each grid cell is weighted by the fraction of the cell that lies inside the county, multiplied by the percentage of the grid cell that is cropland/pasture/grassland according to NLCD land cover data). Temporally, all time-dependent features are also aggregated to weekly or daily frequency - for each variable, there is a column for each week (or day).

## Software to load the data:

Example code for loading the data can be found in the [GNN-RNN Github repo](https://github.com/gomes-lab/GNN-RNN).

For example, [here](https://github.com/gomes-lab/GNN-RNN/blob/main/baseline/single_year_train.py#L268-L270) (`baseline/single_year_train.py`, start of `train()` function) is some code
that loads the .npz file , and [here](https://github.com/gomes-lab/GNN-RNN/blob/main/baseline/baseline_utils.py#L132) (`baseline/single_year_utils.py`, `get_X_Y()` function) is code that processes the raw data into
X/Y matrices for train/validation/test.

* People involved with sample collection, processing, analysis and/or submission: Zhiyun Li processed the gSSURGO soil quality data and aggregated them to the county-level. Ariel Ortiz-Bobea wrote R scripts that were used to efficiently aggregate gridded PRISM data to county level, using cropland cover data from NLCD as weights. Junwen Bai helped with some data processing.


# DATA-SPECIFIC INFORMATION FOR: combined_dataset_weekly_32bit.npz, combined_dataset_daily_32bit.npz

* Number of variables: 6322 (weekly), 43569 (daily)
* Number of cases/rows: 124320
* Variable List: See below.
* Missing data codes: Numpy nan. The only county with missing data (NLDAS) is FIPS 25019 (Nantucket County, MA), which we typically filter out.

## Variable descriptions

Each row represents a county at a given year (3108 counties in the contiguous US * 40 years between 1981-2020 inclusive).

The column names and indices are listed [here](https://docs.google.com/spreadsheets/d/1hhQ8lGzfgLLyl-gKX13NNboJFywIsJoJOKdttx9hKxE/edit?usp=sharing) (make sure to click the correct tab: daily or weekly). They are also listed in the "column_names_weekly.csv" and "column_names_daily.csv" files.

- Column 0 is the county FIPS code. Lookup codes [here](https://transition.fcc.gov/oet/info/maps/census/fips/fips.txt).
- Column 1 is year.
- Columns 2-7 are crop yields for that county and year, for various crops (corn, upland cotton, sorghum, etc.). The data comes from USDA. Note that for each crop, only some counties/years have data.

The remaining columns are input features used by the model (see the linked sheet for the exact column indices). For temporal features, there is a column for the value at each week/day in the year.

### Weather features

Weather features come from the PRISM dataset (Daly and Bryant 2017), with an original spatial resolution of 4 km and a temporal resolution of daily:
- Precipitation
- Mean dewpoint temperature
- Daily max temperature
- Daily mean temperature
- Daily minimum temperature
- Max vapor pressure deficit
- Min vapor pressure deficit

### Land surface features

Land surface features come from the NLDAS land surface model (Xia et al. 2012), with an original spatial resolution of 0.125 degrees (14 km) and a temporal resolution of hourly:
- Precipitation hourly total (kg/m2)
- Moisture availability (%), 0-200 cm
- Moisture availability (%), 0-100 cm
- Soil moisture content (kg/m2), 0-200cm
- Soil moisture content (kg/m2), 0-100cm
- Soil moisture content (kg/m2), 0-10cm
- Soil moisture content (kg/m2), 10-40cm
- Soil moisture content (kg/m2), 40-100cm
- Soil moisture content (kg/m2), 100-200cm
- 2-m above ground specific humidity (kg/kg)
- 2-m above ground temperature (K)
- Soil temperature (K), 0-10 cm
- Soil temperature (K), 10-40 cm
- Soil temperature (K), 40-100 cm
- Soil temperature (K), 100-200 cm
- Wind speed (m/s), hourly max
(Note that the cm ranges represent depths in the soil.)

### Soil quality features

Soil quality features come from the Gridded Soil Survey Geographic Database (gSSURGO) (Soil Survey Staff 2020). The dataset has a 30-m spatial resolution for the continental U.S. These variables do not change over time. However, they vary with depths, which are measured at 6 soil depth layers (0-5cm, 5-15cm, 15-30cm, 30-60cm, 60-100cm, 100-200cm). Because soil quality at a given point can vary substantially within a county, accounting for the location of agricultural activity can be critical when constructing appropriate county-level soil variables. Thus, the “weighted-average” technique is especially important here. We aggregate the fine-scale soil data to the county level based on the percentage of each NLCD Land Cover grid
cell that was covered by agricultural land (grassland, pasture, cropland) in 2011.
- Available water capacity of the dominant soil component
- Bulk density
- Electrical conductivity of the dominant soil component
- Organic matter
- Average % silt
- Average % clay
- Average % sand
- % area covered by Clay soil type
- % area covered by Silty Clay soil type
- % area covered by Sandy Clay soil type
- % area covered by Clay Loam soil type
- % area covered by Silty Clay Loam soil type
- % area covered by Sandy Clay Loam soil type
- % area covered by Loam soil type
- % area covered by Silt Loam soil type
- % area covered by Sandy Loam soil type
- % area covered by Silt Loam soil type
- % area covered by Loamy Sand soil type
- % area covered by Sand soil type
- pH, which is influenced by chemical reactions between water and the dominant soil component
Note that features 8-19 were not present in the original gSSURGO dataset. Rather, for each pixel, we used the raw silt, clay, and sand percentages to compute the “soil texture type” of that pixel, based on the National Resources Conservation Service Soil Survey’s classification scheme (Soil Survey 2021). This classification scheme is depicted below. After classifying each pixel’s soil texture type, we compute the fraction of each county that is occupied by each soil
texture type. 

### Extra features

Extra features also come from the gSSURGO dataset (Soil Survey Staff 2020), but are not depth-dependent. They are listed below:
- National commodity crop productivity index
- Depth to any soil restrictive layer
- NCCPI crop productivity index for small grains, weighted average
- NCCPI crop productivity index for corn
- NCCPI crop productivity index for cotton
- NCCPI crop productivity index for soybean