# Code for the high-resolution near-surface meteorological forcing dataset for arid Xinjiang (3DVAR-MF-XJ)

This repository provides the custom scripts, configuration files and documentation used to generate and validate the **Three-Dimensional Variational Xinjiang Meteorological Forcing Dataset (3DVAR-MF-XJ)**. The dataset provides hourly and daily 0.1° near-surface meteorological fields over Xinjiang for 1961–2020, including 2 m air temperature, precipitation, surface pressure, 10 m wind speed, relative humidity and downward shortwave radiation.

The public dataset is available from Science Data Bank (ScienceDB):

**Data DOI:** https://doi.org/10.57760/sciencedb.32106

## 1. Repository contents

The code archive is organized into two main parts.

```text
code/
├── Assimilation case/
│   ├── geog/
│   ├── namelists/
│   ├── radiance_info/
│   ├── scripts/
│   └── README.md
└── validation code/
    ├── figure4.m
    ├── figure5.m
    ├── figure6.m
    ├── figure7.m
    ├── figure8.m
    ├── table2.m
    ├── table3.m
    ├── table4.m
    ├── Independent site 3DVAR-MF-XJ data.m
    ├── Independent station observation data.m
    ├── station_metadata_3DVAR_MF_XJ.xlsx
    ├── Code Description.docx
    └── boundary shapefile files
```

## 2. Assimilation case

The folder `Assimilation case/` provides a reproducible example of the WRF–WRFDA 3DVAR workflow used to generate the dynamically consistent intermediate product, 3DVAR-XJ. It is intended as a case template rather than a full redistribution of WRF, WPS, WRFDA or third-party input datasets.

Main files include:

- `scripts/run_xinjiang_3dvar_ahi_case.sh`: example shell script for a 5-day Xinjiang WRF–WRFDA assimilation and forecast cycle.
- `scripts/bashrc_wrfda_case_example`: example environment configuration for WRF/WPS/WRFDA execution.
- `namelists/namelist.wps`: WPS configuration.
- `namelists/namelist.input`: WRF model configuration.
- `namelists/namelist.input.3dvar.xinjiang`: WRFDA 3DVAR configuration.
- `namelists/namelist.obsproc.3dvar.xinjiang`: observation preprocessing configuration.
- `radiance_info/himawari-8-ahi.info`: Himawari-8/AHI radiance-channel information used by the assimilation example.
- `geog/GEOGRID.TBL.override.template` and `geog/README_GEOGRID.md`: notes and template settings for geographic input fields, including terrain and land-use preprocessing.

The assimilation example follows the workflow described in the manuscript: ERA5 is used for initial and lateral boundary conditions and as the background field, surface station observations provide the primary near-surface constraint, and Himawari-8/AHI water-vapour-channel radiances are assimilated when available.

## 3. Validation code

The folder `validation code/` contains MATLAB scripts used to reproduce the main validation figures and tables in the revised manuscript.

### 3.1 Figures

- `figure4.m`: plots seasonal cycles of the six variables evaluated at the 30 withheld stations.
- `figure5.m`: plots annual-mean time series of the six variables evaluated at the 30 withheld stations.
- `figure6.m`: plots station-based boxplots of ME, CC, RMSE and DISO using daily time series at the 30 withheld stations.
- `figure7.m`: maps station-level mean error (ME) of 3DVAR-MF-XJ-S at the 30 withheld stations.
- `figure8.m`: maps station-level root-mean-square error (RMSE) of 3DVAR-MF-XJ-S at the 30 withheld stations.

### 3.2 Tables

- `table2.m`: calculates the percentage reduction in RMSE of 3DVAR-MF-XJ-S relative to ERA5, ERA5-Land, CN05.1 and the uncorrected 3DVAR-XJ-S product.
- `table3.m`: calculates sub-daily RMSE validation statistics at 02:00, 08:00, 14:00 and 20:00 Beijing Time for variables with available sub-daily station observations.
- `table4.m`: calculates split-period validation statistics for 1961–1990 and 1991–2020, reported as mean ± standard deviation across the 30 withheld stations.

### 3.3 Independent-station extraction

- `Independent site 3DVAR-MF-XJ data.m`: extracts 3DVAR-MF-XJ values at the 30 withheld station locations and the four available Beijing Time observation hours.
- `Independent station observation data.m`: extracts the corresponding station observations for the same 30 withheld stations and observation times.

These scripts were used to support the additional sub-daily validation added during manuscript revision.

### 3.4 Station metadata and boundary files

- `station_metadata_3DVAR_MF_XJ.xlsx`: provides station identifier, station name, longitude, latitude, elevation and available period for the surface meteorological stations used in the study.
- `降水重建_南北.*`: shapefile components used for plotting the Xinjiang boundary and regional divisions in validation figures.

The station metadata file contains descriptive metadata only. The original station observation time series are not included in this repository because they are subject to the data-use restrictions of the provider.

## 4. Required input data

The validation scripts require the released 3DVAR-MF-XJ dataset and comparison/reference datasets described in the manuscript. Users should modify the file paths at the beginning of each script before running the code.

The main required datasets include:

- 3DVAR-MF-XJ and verification products generated in this study.
- ERA5 and ERA5-Land products used for comparison.
- CN05.1 gridded observation data used for comparison.
- Withheld-station observations for users with authorized access.
- Grid files, station-location files and boundary shapefiles required by the plotting scripts.

The released gridded dataset is available from ScienceDB under the DOI listed above. Restricted raw station observation time series are not redistributed by the authors.

## 5. Software requirements

The scripts were developed and tested mainly in MATLAB. The assimilation example requires an external WRF/WPS/WRFDA environment.

Typical requirements are:

- MATLAB for validation, figure generation and table generation.
- WRF, WPS and WRFDA for the assimilation-case example.
- ANUSPLIN for thin-plate spline interpolation used in station-to-grid correction steps.
- Standard MATLAB mapping/shapefile-reading functions for spatial plotting.

ANUSPLIN is a third-party non-free interpolation package and is not redistributed in this repository. Users who wish to reproduce executable-dependent station-interpolation steps should obtain ANUSPLIN from its official source and ensure that they have appropriate permission to use it.

## 6. Notes on reproducibility

This repository documents the workflow used to generate and validate the dataset, but it does not redistribute all third-party software or restricted observational records. In particular:

1. WRF, WPS and WRFDA must be installed separately by the user.
2. ERA5, ERA5-Land, ASTER GDEM, MODIS MCD12Q1 and WPS geographical input data should be obtained from their official providers.
3. National surface meteorological station observations are subject to provider data-use restrictions and are not included.
4. The released 3DVAR-MF-XJ product and associated gridded data are available from ScienceDB.

Users should adjust paths, filenames and environment variables according to their local computing environment.

## 7. Citation

If you use the dataset or code, please cite the dataset record:

Xu, Y., Zhang, L., Wang, H., Ning, D., Bai, M., Cong, X. & Hao, Z. A high-resolution near-surface meteorological forcing dataset for arid Xinjiang. Science Data Bank. https://doi.org/10.57760/sciencedb.32106 (2025).

Please also cite the associated manuscript when it becomes available.

## 8. Contact

For questions about the dataset or code, please contact the corresponding author listed in the associated manuscript.
