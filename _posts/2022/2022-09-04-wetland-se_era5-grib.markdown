---
layout: post
title: ERA5 GRIB extract
categories: wetland-se
excerpt: "ERA5 GRIB file information and extraction"
tags:
  - ERA5
  - climate
  - grib
  - gdal
  - gdalinfo
  - gdal_translate
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-04'
modified: '2022-09-27'
comments: true
share: true
---

## Introduction

General Regularly-distributed Information in Binary form (GRIB) is commonly used for distribution of Meteorological information, and is propagated by the World Meteorological Organization (WMO). The ERA5 data from C3S, as well as other Copernicus data services, are delivered as <span class='file'>.grib</span> files.

## Prerequisite

This post assumes that you have downloaded a GRIB file as outlined in e.g. the [previous post on ERA5](../wetland-se_era5/).

## GRIB files

GRIB files can contain any number of bands or layers and you need to know which band to retrieve and what it contains. If you are using Karttur's GeoImagine Framework you should have <span class='terminalapp'>GDAL</span> installed, and you can use <span class='terminalapp'>gdalinfo</span> to get information on the dataset and then <span class='terminalapp'>gdal_translate</span> to extract the layers you want.

### Gdalinfo for GRIB file content

The example file used below is the ERA5 monthly _precipitation_ dataset for the years 2010-2011. I have moved and renamed the grib file to fit the structure of Karttur's GeoImagine Framework.

```
% gdalinfo /Volumes/sewetland/DOWNLOADS/ERA5/tot-p/precip_2010-2011.grib
Warning: Inside GRIB2Inventory, Message # 25
ERROR: Ran out of file reading SECT0
Driver: GRIB/GRIdded Binary (.grb, .grb2)
Files: /Volumes/sewetland/DOWNLOADS/ERA5/tot-p/precip_2010-2011.grib
Size is 3600, 1801
Coordinate System is:
GEOGCRS["Coordinate System imported from GRIB file",
    DATUM["unnamed",
        ELLIPSOID["Sphere",6367470,0,
            LENGTHUNIT["metre",1,
                ID["EPSG",9001]]]],
    PRIMEM["Greenwich",0,
        ANGLEUNIT["degree",0.0174532925199433,
            ID["EPSG",9122]]],
    CS[ellipsoidal,2],
        AXIS["latitude",north,
            ORDER[1],
            ANGLEUNIT["degree",0.0174532925199433,
                ID["EPSG",9122]]],
        AXIS["longitude",east,
            ORDER[2],
            ANGLEUNIT["degree",0.0174532925199433,
                ID["EPSG",9122]]]]
Data axis to CRS axis mapping: 2,1
Origin = (-180.050000000000011,90.049999999999997)
Pixel Size = (0.100000000000000,-0.100000000000000)
Corner Coordinates:
Upper Left  (-180.0500000,  90.0500000) (180d 3' 0.00"W, 90d 3' 0.00"N)
Lower Left  (-180.0500000, -90.0500000) (180d 3' 0.00"W, 90d 3' 0.00"S)
Upper Right ( 179.9500000,  90.0500000) (179d57' 0.00"E, 90d 3' 0.00"N)
Lower Right ( 179.9500000, -90.0500000) (179d57' 0.00"E, 90d 3' 0.00"S)
Center      (  -0.0500000,  -0.0000000) (  0d 3' 0.00"W,  0d 0' 0.00"S)
Band 1 Block=3600x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC (Ground or water surface)
  NoData Value=9999
  Metadata:
    GRIB_COMMENT=Total precipitation [m]
    GRIB_ELEMENT=TP
    GRIB_FORECAST_SECONDS=0
    GRIB_REF_TIME=1262304000
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[m]
    GRIB_VALID_TIME=1262304000
Band 2 Block=3600x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC (Ground or water surface)
  NoData Value=9999
  Metadata:
    GRIB_COMMENT=Total precipitation [m]
    GRIB_ELEMENT=TP
    GRIB_FORECAST_SECONDS=0
    GRIB_REF_TIME=1264982400
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[m]
    GRIB_VALID_TIME=1264982400
...
...
Band 24 Block=3600x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC (Ground or water surface)
  NoData Value=9999
  Metadata:
    GRIB_COMMENT=Total precipitation [m]
    GRIB_ELEMENT=TP
    GRIB_FORECAST_SECONDS=0
    GRIB_REF_TIME=1322697600
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[m]
    GRIB_VALID_TIME=1322697600
```

As the example file (from the [previous](../wetland-se_era5/) post) contained montlhy values for two years (2010 and 2011), the GRIB file contains 24 bands, one for each month.

### Gdal_translate for GRIB file extraction

To extract a single band from the GRIB file using <span class='terminalapp'>gdal_translate</span>, the command adhering to the concept of Karttur's geoImagine Framework looks like this:

```
gdal_translate -b 1 /Volumes/sewetland/DOWNLOADS/ERA5/tot-p/precip_2010-2011.grib /Volumes/sewetland/ancillary/c3s/region/precip/global/201001/tot-precip-m-m_era5_global_201001_v2022.tif
```
The first part of the filename _tot-precip-m-m_ needs to be specific for each timespan (month) and unit (m) used even if the variable (precipitation) is the same. This is required for separating monthly precipitation from e.g. daily, and units given in m with other using (e.g. mm or kg m^-2).

<figure>
<img src="../../images/era5_total-precip_201010.png">
<figcaption> ERA5 global monthly total precipitation January 2010. </figcaption>
</figure>

### Framework extraction

If you installed Karttur's GeoImagine Framework, you can use the process <span class='process'>OrganizeNetCDFetGRIB</span> for extracting all bands (for a particular variable) in a GRIB file automatically. They will then be extracted into the default system of the Framework and also registered in the Framework database.

#### Single variable file extraction

The framework is defaulted to extract non-gap time-series of a single variable. It is, however, with some additional paramters, also possible to extract files with multiple variables and seasonal data.

The links below are for extracting the GRBI file with total precipitation as a single variable used above and downloaded in the [previous](../wetland-se_era5/) post.

Framework process: [OrganizeNetCDFetGRIB](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeNetCDFetGRIB/)

Json command file: [0160-OrganizeNetCDFetGRIB-ERA5-tot-precip_2010-2011.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0160-OrganizeNetCDFetGRIB-CERRA-tot-precip/)

```
## Organize CERRA climate data ##
0160-OrganizeNetCDFetGRIB-CERRA-tot-precip.json
```

#### Multiple variables file extraction

GRIB files with multiple variables need to have additional parameters, identifying the number of variables (_nvars_) and the (python system starting at 0) number of the first band to extract.
