---
layout: post
title: CERRA GRIB extract
categories: wetland-se
excerpt: "GRIB file information and extraction"
tags:
  - ERA5
  - CERRA
  - climate
  - grib
  - gdal
  - gdalinfo
  - gdal_translate
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-12'
modified: '2022-09-27'
comments: true
share: true
---

## Introduction

General Regularly-distributed Information in Binary form (GRIB) is commonly used for distribution of Meteorological information, and is propagated by the World Meteorological Organization (WMO). Data from the Copernicus European Regional ReAnalysis Land (CERRA-Land), as well as other Copernicus data services, are delivered as <span class='file'>.grib</span> files.

## Prerequisite

This post assumes that you have downloaded a GRIB file as outlined in e.g. the [previous](../wetland-se_cerra/) post.

## GRIB files

GRIB files can contain any number of bands or layers and you need to know which band to retrieve and what it contains. If you are using Karttur's GeoImagine Framework you should have <span class='terminalapp'>GDAL</span> installed, and you can use <span class='terminalapp'>gdalinfo</span> to get information on the dataset and then <span class='terminalapp'>gdal_translate</span> to extract the layers you want.

### Gdalinfo for GRIB file content

The example file used below is the CERRA daily _Total precipitation_ dataset for Europe for all the days of January 2020. I have moved and renamed it to better fit the structure of Karttur's GeoImagine Framework.

```
% gdalinfo /Volumes/sewetland/DOWNLOADS/CERRA/tot-p/tot-p_cerra_europe_20200101-20200131_0.grib
Driver: GRIB/GRIdded Binary (.grb, .grb2)
Files: /Volumes/sewetland/DOWNLOADS/CERRA/tot-p/tot-p_cerra_europe_20200101-20200131_0.grib
Size is 1069, 1069
Coordinate System is:
PROJCRS["unnamed",
    BASEGEOGCRS["Coordinate System imported from GRIB file",
        DATUM["unnamed",
            ELLIPSOID["Sphere",6371229,0,
                LENGTHUNIT["metre",1,
                    ID["EPSG",9001]]]],
        PRIMEM["Greenwich",0,
            ANGLEUNIT["degree",0.0174532925199433,
                ID["EPSG",9122]]]],
    CONVERSION["Lambert Conic Conformal (2SP)",
        METHOD["Lambert Conic Conformal (2SP)",
            ID["EPSG",9802]],
        PARAMETER["Latitude of false origin",50,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8821]],
        PARAMETER["Longitude of false origin",8,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8822]],
        PARAMETER["Latitude of 1st standard parallel",50,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8823]],
        PARAMETER["Latitude of 2nd standard parallel",50,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8824]],
        PARAMETER["Easting at false origin",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8826]],
        PARAMETER["Northing at false origin",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8827]]],
    CS[Cartesian,2],
        AXIS["(E)",east,
            ORDER[1],
            LENGTHUNIT["Metre",1]],
        AXIS["(N)",north,
            ORDER[2],
            LENGTHUNIT["Metre",1]]]
Data axis to CRS axis mapping: 1,2
Origin = (-2939750.045080067124218,2939749.990544109605253)
Pixel Size = (5500.000000000000000,-5500.000000000000000)
Corner Coordinates:
Upper Left  (-2939750.045, 2939749.991) ( 58d10'52.61"W, 63d45'58.21"N)
Lower Left  (-2939750.045,-2939750.009) ( 17d30' 0.63"W, 20d15'51.33"N)
Upper Right ( 2939749.955, 2939749.991) ( 74d10'52.60"E, 63d45'58.22"N)
Lower Right ( 2939749.955,-2939750.009) ( 33d30' 0.63"E, 20d15'51.33"N)
Center      (  -0.0450801,  -0.0094559) (  8d 0' 0.00"E, 50d 0' 0.00"N)
Band 1 Block=1069x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC="Ground or water surface"
  Metadata:
    GRIB_COMMENT=Total precipitation rate [kg/(m^2*s)]
    GRIB_DISCIPLINE=0(Meteorological)
    GRIB_ELEMENT=TPRATE
    GRIB_FORECAST_SECONDS=0
    GRIB_IDS=CENTER=84(Toulouse) SUBCENTER=0 MASTER_TABLE=23 LOCAL_TABLE=0 SIGNF_REF_TIME=1(Start_of_Forecast) REF_TIME=2020-01-01T06:00:00Z PROD_STATUS=10 TYPE=0(Analysis)
    GRIB_PDS_PDTN=8
    GRIB_PDS_TEMPLATE_ASSEMBLED_VALUES=1 52 0 255 84 65535 255 1 0 1 -127 -2147483647 255 -127 -2147483647 2020 1 1 6 0 0 1 0 1 2 1 0 255 0
    GRIB_PDS_TEMPLATE_NUMBERS=1 52 0 255 84 255 255 255 1 0 0 0 0 1 255 255 255 255 255 255 255 255 255 255 255 7 228 1 1 6 0 0 1 0 0 0 0 1 2 1 0 0 0 0 255 0 0 0 0
    GRIB_REF_TIME=1577858400
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[kg/(m^2*s)]
    GRIB_VALID_TIME=1577858400
Band 2 Block=1069x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC="Ground or water surface"
  Metadata:
    GRIB_COMMENT=Total precipitation rate [kg/(m^2*s)]
    GRIB_DISCIPLINE=0(Meteorological)
    GRIB_ELEMENT=TPRATE
    GRIB_FORECAST_SECONDS=0
    GRIB_IDS=CENTER=84(Toulouse) SUBCENTER=0 MASTER_TABLE=23 LOCAL_TABLE=0 SIGNF_REF_TIME=1(Start_of_Forecast) REF_TIME=2020-01-02T06:00:00Z PROD_STATUS=10 TYPE=0(Analysis)
    GRIB_PDS_PDTN=8
    GRIB_PDS_TEMPLATE_ASSEMBLED_VALUES=1 52 0 255 84 65535 255 1 0 1 -127 -2147483647 255 -127 -2147483647 2020 1 2 6 0 0 1 0 1 2 1 0 255 0
    GRIB_PDS_TEMPLATE_NUMBERS=1 52 0 255 84 255 255 255 1 0 0 0 0 1 255 255 255 255 255 255 255 255 255 255 255 7 228 1 2 6 0 0 1 0 0 0 0 1 2 1 0 0 0 0 255 0 0 0 0
    GRIB_REF_TIME=1577944800
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[kg/(m^2*s)]
    GRIB_VALID_TIME=1577944800
...
...
Band 31 Block=1069x1 Type=Float64, ColorInterp=Undefined
  Description = 0[-] SFC="Ground or water surface"
  Metadata:
    GRIB_COMMENT=Total precipitation rate [kg/(m^2*s)]
    GRIB_DISCIPLINE=0(Meteorological)
    GRIB_ELEMENT=TPRATE
    GRIB_FORECAST_SECONDS=0
    GRIB_IDS=CENTER=84(Toulouse) SUBCENTER=0 MASTER_TABLE=23 LOCAL_TABLE=0 SIGNF_REF_TIME=1(Start_of_Forecast) REF_TIME=2020-01-31T06:00:00Z PROD_STATUS=10 TYPE=0(Analysis)
    GRIB_PDS_PDTN=8
    GRIB_PDS_TEMPLATE_ASSEMBLED_VALUES=1 52 0 255 84 65535 255 1 0 1 -127 -2147483647 255 -127 -2147483647 2020 1 31 6 0 0 1 0 1 2 1 0 255 0
    GRIB_PDS_TEMPLATE_NUMBERS=1 52 0 255 84 255 255 255 1 0 0 0 0 1 255 255 255 255 255 255 255 255 255 255 255 7 228 1 31 6 0 0 1 0 0 0 0 1 2 1 0 0 0 0 255 0 0 0 0
    GRIB_REF_TIME=1580450400
    GRIB_SHORT_NAME=0-SFC
    GRIB_UNIT=[kg/(m^2*s)]
    GRIB_VALID_TIME=1580450400
```

As the example file (from the [previous](../wetland-se_cerra/) post) contained daily values for the month of January 2020, the GRIB file contains 31 bands, one for each day.

### Gdal_translate for GRIB file extraction

To extract a single band from the GRIB file using <span class='terminalapp'>gdal_translate</span>, the command adhering to the concept of Karttur's geoImagine Framework looks like this:

```
gdal_translate -b 1 /Volumes/sewetland/DOWNLOADS/CERRA/tot-p/tot-p_cerra_europe_20200101-20200131_0.grib /Volumes/sewetland/ancillary/cerra/region/precip/europe/20200101/precip_cerra_europe_20200101_0.tif
```

### Framework extraction

If you installed Karttur's GeoImagine Framework, you can use the process <span class='process'>OrganizeNetCDFetGRIB</span> for extracting all bands in a GRIB file automatically. They will then be extracted into the default system of the Framework and also registered in the Framework database.

Framework process: [OrganizeNetCDFetGRIB](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeNetCDFetGRIB/)

Json command file: [0160-OrganizeNetCDFetGRIB-CERRA-tot-precip.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0160-OrganizeNetCDFetGRIB-CERRA-tot-precip/)

```
## Organize CERRA climate data ##
0160-OrganizeNetCDFetGRIB-CERRA-tot-precip.json
```
