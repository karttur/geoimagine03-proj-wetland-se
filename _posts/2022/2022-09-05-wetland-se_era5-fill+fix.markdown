---
layout: post
title: ERA5 correct and fill
categories: wetland-se
excerpt: "ERA5 climate data filling of nodata and fixing projections"
tags:
  - ERA5
  - climate
  - grib
  - gdal_fillnodata

image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-05'
modified: '2022-09-27'
comments: true
share: true
---

## Introduction

The ERA5-Land global climate dataset has some issues that need to be fixed before it can be used for analysis and comparison with other datasets. First, ERA5-Land has a parsimonious definition of land, and large parts of the global near-shore land mass falls outside the land mask that is used. Second, some variables and individual layers are defined with the upper left corner at Longitude 0 (the Greenwich meridian), whereas most global maps are defined with -180 degrees (180 degrees West) as the upper left corner. This post is a tutorial on how to solve these two problems, both in principal for a single layer and automatically for full time series data using Karttur's GeoImagine Framework.

## Prerequisite

This post assumes that you have [downloaded the ERA5 datasets](../wetland-se_era5) and then [extracted the time series of climate data](../wetland-se_era5-grib). To automatically expand the climate data over coastal area and correct the projection in the time series data you must have [installed Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/).

## ERA5-land projection to 180W - 180E

Fixing the projection of the data variables that are delivered as longitude 0 to 360E is a bit more tricky. The principle solution is to cut the raster into two parts and piece them back together with a new offset/scale. This can be achieved using different approaches.

One alternative is to create two virtual layers, one for the western hemisphere and one for the eastern hemisphere, and then mosaic these two halves. A second alternative is to read the raster data as a python numpy array, roll the columns and save the array as a GeoTiff with the left boundary adjusted to 180 degrees West.

The latter is much simpler, faster and does not require any intermediate step. It is thus implemented in Karttur's GeoImagine Framework as the process <span class='process'>NumpyRollRaster</span>. The process reads the meta-data including the project transformation and calculates how many columns (or rows) that must be rolled for fitting the raster to the predefined upper left map corner. Thus you can safely apply the process also to any raster layers - those that have the correct projection transformation to start with will keep the original extent without rolling.

### Framework process <span class='process'>NumpyRollRaster</span>

Framework process: [NumpyRollRaster](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-NumpyRollRaster/).

Json command file: [0165-fillnodata-ERA5-climate](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0165-fillnodata-ERA5-climate/)

```
## project ERA5 climate data to 180 W
## REMOVE TO RUN ## 0162-NumpyRollRaster-ERA5-climate.json
```

## ERA5-Land default land mask

The ERA5 default mask used for defining the global land area leave large terrestrial coastal areas out. The image below compares the default mask (left) with masks that have been expanded 1 (centre) and 2 (right) pixels using [gdal_fillnodata](https://gdal.org/programs/gdal_fillnodata.html#gdal-fillnodata). The region used for the illustration is South Eastern Sweden with some archipelago and the island of Öland.

<figure class="third">
	<a href="../../images/ERA5-landsmask_se-sweden_orignal.png"><img src="../../images/ERA5-landsmask_se-sweden_orignal.png" alt="image"></a>

  <a href="../../images/ERA5-landsmask_se-sweden_1px-fill.png"><img src="../../images/ERA5-landsmask_se-sweden_1px-fill.png" alt="image"></a>

  <a href="../../images/ERA5-landsmask_se-sweden_2px-fill.png"><img src="../../images/ERA5-landsmask_se-sweden_2px-fill.png" alt="image"></a>

	<figcaption>ERA5 Land mask; left: original mask; centre: mask expanded 1 pixel; right: mask expanded 2 pixels. </figcaption>
</figure>

### Expand mask by gdal_fillnodata

The two fill versions of the maps above were expanded using [gdal_fillnodata](https://gdal.org/programs/gdal_fillnodata.html#gdal-fillnodata), with the file names given as expected by Karttur's GeoImagine Framework:

```
gdal_fillnodata.py -md 1 -si 0 /Volumes/sewetland/ancillary/c3s/region/precip/global/195001/tot-precip-m-m_era5_global_195001_v2022.tif /Volumes/sewetland/ancillary/c3s/region/precip/global/195001/tot-precip-m-m_era5_global_195001_v2022-fill-2.tif
gdal_fillnodata.py -md 2 -si 0 /Volumes/sewetland/ancillary/c3s/region/precip/global/195001/tot-precip-m-m_era5_global_195001_v2022.tif /Volumes/sewetland/ancillary/c3s/region/precip/global/195001/tot-precip-m-m_era5_global_195001_v2022-fill-2.tif
```

To fully cover the terrestrial land area of Sweden the original mask must be expanded 8 pixels. As the dataset is going to be used for wetland mapping, including coastal wetlands, 8 pixels was set as the expansion distance.

### Expand mask using Karttur's GeoImagine Framework

Karttur's GeoImagine Framework includes a binding that calls [gdal_fillnodata](https://gdal.org/programs/gdal_fillnodata.html#gdal-fillnodata), and applies the spatial data expansion on all files in the defined time-series:

Framework process: [fillnodataregion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-fillnodataregion/).

Json command file: [0165-fillnodata-ERA5-climate](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0165-fillnodata-ERA5-climate/)
