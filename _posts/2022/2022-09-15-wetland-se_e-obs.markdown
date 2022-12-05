---
layout: post
title: E-OBS
categories: wetland-se
excerpt: "Download and organize climate data from E-OBS"
tags:
  - e-obs
  - climate
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-15'
modified: '2022-11-25'
comments: true
share: true
---

## Introduction

[European Climate Assessment & Dataset (ECA&D)](https://www.ecad.eu) offers datasets with daily gridded climate parameters, the [E-OBS gridded dataset](https://www.ecad.eu/download/ensembles/ensembles.php). This dataset is freely available for research and directly available for download without any registration or special scripting.

## Prerequisite

There is no prerequisite for accessing and downloading the E-OBS data, but use is restricted. If you want to automate the download as described in this post you must have Karttur's GeoImagine Framework installed.

## E-OBS

E-OBS is a daily gridded observational dataset at 0.1 or 0.25 degrees spatial resolution over Europe derived from ECA&D information. the following thematic daily data is available:

| **dataset** | **abbreviation** |
| daily mean temperature | TG |
| daily minimum temperature | TN |
| daily maximum temperature | TX |
| daily precipitation sum | RR |
| daily mean sea level pressure | PP |
| daily mean wind speed | FG |
| daily mean relative humidity | HU |
| global radiation | QQ |

### Access E-OBS data

[E-OBS data access chunks](https://surfobs.climate.copernicus.eu/dataaccess/access_eobs_chunks.php) offers downloading by just clicking the datasets you want. All data are offered at both 0.1 and 0.25 degrees spatial resolutions. The data is derived from an ensemble of models, and both the mean and the spread are available.

#### Using Karttur's GeoImagine Framework

You can use Karttur's GeoImagine to automate the download. Instead of clicking the individual files to download you create a list of the files you want to download and then save it as a simple text file (<span class='file'>*.txt</span>). The example below lists all the files with Daily precipitation sum (RR) at 0.1 degrees spatial resolution:

```
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/rr_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/rr_ens_mean_0.1deg_reg_1965-1979_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/rr_ens_mean_0.1deg_reg_1980-1994_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/rr_ens_mean_0.1deg_reg_1995-2010_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/rr_ens_mean_0.1deg_reg_2011-2022_v26.0e.nc
```

To download the listed files, use the Framework process [<span class='process'>DownloadAncillary</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/) and define the download in a Json command file: [0115-download-E-OBS](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0115-download-E-OBS/). You then have to link the Json command file to the list of executions to run:

```
## Download E-OBS Climate data ##
0115-download-E-OBS.json
```

### Extract E-OBS data

The E-Obs data is delivered as Network Common Data Form version 4 [NetCDF-4](https://www.unidata.ucar.edu/software/netcdf/) (<span class='file'>*.nc</span>) files. To extract the 15 years worth of data from the file you need to define the connection between the bands in the NetCDF and the date they represent. To view the information of a NetCDF I use <span class='terminalapp'>GDAL</span> (see post on [Install GDAL, QGIS and GRASS](https://karttur.github.io/setup-ide/setup-ide/install-gis/)).

Using the naming of the downloaded files as defined in the Json command file: [0115-download-E-OBS](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0115-download-E-OBS/):

<span class='terminal'>gdalinfo path/to/rr_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc</span>

```
% gdalinfo /Volumes/sewetland/DOWNLOADS/E-OBS/RR/data/rr_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc
Driver: netCDF/Network Common Data Format
Files: /Volumes/sewetland/DOWNLOADS/E-OBS/RR/data/rr_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc
Size is 705, 465
Origin = (-25.000139508999698,71.499858543732898)
Pixel Size = (0.099999999599954,-0.099999995440814)
Metadata:
  latitude#axis=Y
  latitude#long_name=Latitude values
  latitude#standard_name=latitude
  latitude#units=degrees_north
  longitude#axis=X
  longitude#long_name=Longitude values
  longitude#standard_name=longitude
  longitude#units=degrees_east
  NC_GLOBAL#Conventions=CF-1.4
  NC_GLOBAL#E-OBS_version=26.0e
  NC_GLOBAL#history=Fri Sep 30 08:24:40 2022: ncks --no-abc -d time,0,5478 /data2/Else/EOBSv26.0e/Grid_0.1deg/rr//rr_ensmean_master_rectime.nc /data2/Else/EOBSv26.0e/Grid_0.1deg/rr//rr_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc
Fri Sep 30 08:22:30 2022: ncks --no-abc --mk_rec_dmn time /data2/Else/EOBSv26.0e/Grid_0.1deg/rr//rr_ensmean_master.nc /data2/Else/EOBSv26.0e/Grid_0.1deg/rr//rr_ensmean_master_rectime.nc
Fri Sep 30 07:11:36 2022: ncks -O -d time,0,26478 /data2/Else/EOBSv26.0e/Grid_0.1deg/rr/rr_ensmean_master_untilJul2022.nc /data2/Else/EOBSv26.0e/Grid_0.1deg/rr/rr_ensmean_master.nc
  NC_GLOBAL#NCO=netCDF Operators version 4.7.5 (Homepage = http://nco.sf.net, Code = http://github.com/nco/nco)
  NC_GLOBAL#References=http://surfobs.climate.copernicus.eu/dataaccess/access_eobs.php
  NETCDF_DIM_EXTRA={time}
  NETCDF_DIM_time_DEF={5479,4}
  NETCDF_DIM_time_VALUES={0,1,2 ... 5478}
  rr#add_offset=0
  rr#cell_methods=time: mean
  rr#long_name=rainfall
  rr#scale_factor=0.1
  rr#standard_name=thickness_of_rainfall_amount
  rr#units=mm
  rr#_FillValue=-9999
  time#calendar=standard
  time#cell_methods=time: mean
  time#long_name=Time in days
  time#standard_name=time
  time#units=days since 1950-01-01 00:00
Corner Coordinates:
Upper Left  ( -25.0001395,  71.4998585)
Lower Left  ( -25.0001395,  24.9998607)
Upper Right (  45.4998602,  71.4998585)
Lower Right (  45.4998602,  24.9998607)
Center      (  10.2498603,  48.2498596)
Band 1 Block=705x465 Type=Int16, ColorInterp=Undefined
  NoData Value=-9999
  Unit Type: mm
  Offset: 0,   Scale:0.100000001490116
  Metadata:
    add_offset=0
    cell_methods=time: mean
    long_name=rainfall
    NETCDF_DIM_time=0
    NETCDF_VARNAME=rr
    scale_factor=0.1
    standard_name=thickness_of_rainfall_amount
    units=mm
    _FillValue=-9999
Band 2 Block=705x465 Type=Int16, ColorInterp=Undefined
  NoData Value=-9999
  Unit Type: mm
  Offset: 0,   Scale:0.100000001490116
  Metadata:
    add_offset=0
    cell_methods=time: mean
    long_name=rainfall
    NETCDF_DIM_time=1
    NETCDF_VARNAME=rr
    scale_factor=0.1
    standard_name=thickness_of_rainfall_amount
    units=mm
    _FillValue=-9999
...
...
Band 5479 Block=705x465 Type=Int16, ColorInterp=Undefined
  NoData Value=-9999
  Unit Type: mm
  Offset: 0,   Scale:0.100000001490116
  Metadata:
    add_offset=0
    cell_methods=time: mean
    long_name=rainfall
    NETCDF_DIM_time=5478
    NETCDF_VARNAME=rr
    scale_factor=0.1
    standard_name=thickness_of_rainfall_amount
    units=mm
    _FillValue=-9999
```
With 15 years worth of daily data, each NetCDF file contains approximately 5479 bands (dependent on how the leap years fall). You can not extract these individually; an automated process is needed. For this tutorial we will use Karttur's GeoImagine Framework and the process <span class='process'>OrganizeNetCDFetGRIB</span>
In the background the process calls <span class='terminalapp'>gdal_translate</span>.

#### Extract NetCDF using Karttur's GeoImagine Framework

If you installed Karttur's GeoImagine Framework, you can use the process <span class='process'>OrganizeNetCDFetGRIB</span> for extracting all bands in a NetCDF file automatically. They will then be extracted into the default system of the Framework and also registered in the Framework database.




gdal_translate -of GTiff -b 10 file.nc test.tiff



daily mean temperature (0.1):
```
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/tg_ens_mean_0.1deg_reg_1950-1964_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/tg_ens_mean_0.1deg_reg_1965-1979_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/tg_ens_mean_0.1deg_reg_1980-1994_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/tg_ens_mean_0.1deg_reg_1995-2010_v26.0e.nc
https://knmi-ecad-assets-prd.s3.amazonaws.com/ensembles/data/Grid_0.1deg_reg_ensemble/tg_ens_mean_0.1deg_reg_2011-2022_v26.0e.nc
```
