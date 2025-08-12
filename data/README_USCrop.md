# USCrop: County-level crop yield, weather, and soil data for the United States (1981-2020)

This dataset contains crop yield, weather, soil, and crop progress information for US counties in 1981-2020. Specifically, each row contains data for a unique county and year. There are two versions: 
1. Weekly - weather data for each week (averaged),
2. Daily - weather data for each day

The column names and indices are listed [here](https://docs.google.com/spreadsheets/d/1hhQ8lGzfgLLyl-gKX13NNboJFywIsJoJOKdttx9hKxE/edit?usp=sharing) (make sure to click the correct tab: daily or weekly). They are also listed in the "column_names_weekly.csv" and "column_names_daily.csv" files.

- Column 0 is the county FIPS code. Lookup codes [here](https://transition.fcc.gov/oet/info/maps/census/fips/fips.txt).
- Column 1 is year.
- Columns 2-7 are crop yields for that county and year, for various crops (corn, upland cotton, sorghum, etc.). The data comes from USDA. Note that for each crop, only some counties/years have data.

The remaining columns are input features used by the model (see the linked sheet for the exact column indices). Note that all features are spatially aggregated to the county level, using a weighted average (where each grid cell is weighted by the fraction of the cell that lies inside the county, multiplied by the percentage of the grid cell that is cropland/pasture/grassland). Temporally, all time-dependent features are also aggregated to weekly or daily frequency - for each variable, there is a column for each week (or day).

## Weather features

Weather features come from the PRISM dataset (Daly and Bryant 2017), with an original spatial resolution of 4 km and a temporal resolution of daily:
- Precipitation
- Mean dewpoint temperature
- Daily max temperature
- Daily mean temperature
- Daily minimum temperature
- Max vapor pressure deficit
- Min vapor pressure deficit

## Land surface features

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

## Soil quality features

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

## Extra features

Extra features also come from the gSSURGO dataset (Soil Survey Staff 2020), but are not depth-dependent. They are listed below:
- National commodity crop productivity index
- Depth to any soil restrictive layer
- NCCPI crop productivity index for small grains, weighted average
- NCCPI crop productivity index for corn
- NCCPI crop productivity index for cotton
- NCCPI crop productivity index for soybean



<!--This readme file was generated on [2025-06-18] by [Joshua Fan]

*Note:
[text within square brackets should be changed to specific information about your dataset.]
*help text within asterisks should be deleted before finalizing your document**

# GENERAL INFORMATION

* Title of Dataset: 
*provide at least two contacts*
## Author/Principal Investigator Information
Name: 
ORCID:
Institution: 
Address: 
Email: 

## Author/Associate or Co-investigator Information
Name: 
ORCID:
Institution: 
Address: 
Email: 

## Author/Alternate Contact Information
Name: 
ORCID:
Institution: 
Address: 
Email: 

* Date of data collection: *provide single date, range, or approximate date; suggested format YYYY-MM-DD)*
* Geographic location of data collection: *provide latitude, longitude, or city/region, State, Country*
* Information about funding sources that supported the collection of the data: 


# SHARING/ACCESS INFORMATION

* Licenses/restrictions placed on the data: 
* Links to publications that cite or use the data: 
* Links to other publicly accessible locations of the data: 
* Links/relationships to ancillary data sets: 
* Was data derived from another source?
	* If yes, list source(s): 
* Recommended citation for this dataset: 


# DATA & FILE OVERVIEW

## File List: *list all files (or folders, as appropriate for dataset organization) contained in the dataset, with a brief description*

* Relationship between files, if important: 
* Additional related data collected that was not included in the current data package: 
* Are there multiple versions of the dataset?
	* If yes, name of file(s) that was updated: 
	* Why was the file updated? 
	* When was the file updated? 


# METHODOLOGICAL INFORMATION

## Description of methods used for collection/generation of data: 
*include links or references to publications or other documentation containing experimental design or protocols used in data collection*

## Methods for processing the data: 
*describe how the submitted data were generated from the raw or collected data*

## Instrument- or software-specific information needed to interpret the data: 
*include full name and version of software, and any necessary packages or libraries needed to run scripts*

*include any additional methodological information needed to interpret and/or use the data, as appropriate*
* Standards and calibration information, if appropriate: 
* Environmental/experimental conditions: 
* Describe any quality-assurance procedures performed on the data: 
* People involved with sample collection, processing, analysis and/or submission: 


# DATA-SPECIFIC INFORMATION FOR: [FILENAME]
*repeat this section for each dataset, folder or file, as appropriate*

* Number of variables: 
* Number of cases/rows: 
* Variable List: *list variable name(s), description(s), unit(s) and value labels as appropriate for each*
* Missing data codes: *list code/symbol and definition*
* Specialized formats or other abbreviations used: -->