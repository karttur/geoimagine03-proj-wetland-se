---
layout: post
title: ERA5 tiling to SWEREF99
categories: wetland-se
excerpt: "Tile ERA5 to SWEREF99"
tags:
  - ERA5
  - climate
  - tiling
  - SWEREF99
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-06'
modified: '2022-09-30'
comments: true
share: true
---

## Introduction

The previous posts have dealt with ERA5 climate data, including how to [download](../wetland-se_era5/), [extract](../wetland-se_era5-grib/) and [correct and fill](../wetland-se_era5-fill+fix/) the data. The complete datasets should now be stored locally in the predefined system of Karttur's GeoImagine Framework and also registered in the Framework postgres database. This post explains how to tile the global (regional) datasets to the [SWEREF99 predefined tiling system](../wetland-se_defregion).

## Prerequisite

This post assumes that you have [downloaded the ERA5 datasets](../wetland-se_era5/), [extracted the time series of climate data](../wetland-se_era5-grib/) and [fixed projection and land mask issues](../wetland-se_era5-fill+fix/). It also requires that you have [installed Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Tiling of ancillary regions

The ERA5 climate datasets are global in extend and were added to the Framework as ancillary geographic (EPSG:4326) datasets with 0.1 degrees spatial resolution. The Framework process for tiling these time-series to the predefined tile system of SWEREF99 is <span class='process'>TileAncillaryRegion</span>.

Framework process: [TileAncillaryRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileAncillaryRegion/)

Json command file: [0180_TileAncillaryRegion_ERA5-climate.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0180_TileAncillaryRegion_ERA5-climate/)

```
## tile ERA5 climate data ##
0180_TileAncillaryRegion_ERA5-climate.json
```

The complete time-series for both total precipitation and air temperature @ 2 m above ground from 1950 to 2021 amounts to 63900 tiles for SWEREF99. Thus the process will take some time finish.
