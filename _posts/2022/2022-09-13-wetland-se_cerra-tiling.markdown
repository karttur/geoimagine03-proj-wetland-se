---
layout: post
title: CERRA GRIB extract
categories: wetland-se
excerpt: "GRIB file information and extraction"
tags:

  - CERRA
  - climate
  - tiling
  - SWEREF99
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-13'
modified: '2022-09-27'
comments: true
share: true
---

## Introduction

The previous posts have dealt with CERRA climate data, including how to [download](../wetland-se_cerra/) and [extract](../wetland-se_cerra-grib/) the data. The complete datasets should now be stored locally in the predefined system of Karttur's GeoImagine Framework and also registered in the Framework postgres database. This post explains how to tile the global (regional) datasets to the [SWEREF99 predefined tiling system](../wetland-se_defregion).

## Prerequisite

This post assumes that you have [downloaded the CERRA datasets](../wetland-se_cerra/) and then [extracted the time series of climate data](../wetland-se_cerra-grib/). It also requires that you have [installed Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Tiling of ancillary regions

The CERRA climate datasets are projected using a customized Lambert Conformal Conical projection. The projection is not predefined as an EPSG code.

Further, the projection coding is not transferred when extracting the data with <span class='terminalapp'>gdal_translate</span> (and thus the Framework process [OrganizeNetCDFetGRIB](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeNetCDFetGRIB/). The tiling of the CERRA data thus demands that you create a customized projection definition. To do that you must first retrieve the information on the projection, and then implement it.

### Projection information

Information on the projection can be found under [5.1.5 The CERRA grid description in the CERRA product user guide](https://confluence.ecmwf.int/display/CKB/Copernicus+European+Regional+ReAnalysis+%28CERRA%29%3A+product+user+guide). Or using <span class='terminalapp'>gdalinfo</span>:

```
% gdalinfo /Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022.tif
Driver: GTiff/GeoTIFF
Files: /Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022.tif
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
        AXIS["easting",east,
            ORDER[1],
            LENGTHUNIT["metre",1,
                ID["EPSG",9001]]],
        AXIS["northing",north,
            ORDER[2],
            LENGTHUNIT["metre",1,
                ID["EPSG",9001]]]]
Data axis to CRS axis mapping: 1,2
Origin = (-2939750.045080067124218,2939749.990544109605253)
Pixel Size = (5500.000000000000000,-5500.000000000000000)
Metadata:
  AREA_OR_POINT=Area
Image Structure Metadata:
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (-2939750.045, 2939749.991) ( 58d10'52.61"W, 63d45'58.21"N)
Lower Left  (-2939750.045,-2939750.009) ( 17d30' 0.63"W, 20d15'51.33"N)
Upper Right ( 2939749.955, 2939749.991) ( 74d10'52.60"E, 63d45'58.22"N)
Lower Right ( 2939749.955,-2939750.009) ( 33d30' 0.63"E, 20d15'51.33"N)
Center      (  -0.0450801,  -0.0094559) (  8d 0' 0.00"E, 50d 0' 0.00"N)
```

gdalwarp -t_srs EPSG:4326 "/Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022.tif" "/Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022-epsg4326.tif"


gdalwarp -s_srs "+proj=lcc +lat_0=50 +lon_0=8 +  lat_1=50 +lat_2=50 +x_0=0 +y_0=0 +R=6371229 +units=m +no_defs" -t_srs EPSG:4326 "/Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022.tif" "/Users/thomasgumbricht/tot-precip-kg*m-2_cerra_europe_20100101_v2022-epsg4326-proj4.tif"


 in extend and were added to the Framework as ancillary geographic (EPSG:4326) datasets with 0.1 degrees spatial resolution. The Framework process for tiling these time-series to the predefined tile system of SWEREF99 is <span class='process'>TileAncillaryRegion</span>.
