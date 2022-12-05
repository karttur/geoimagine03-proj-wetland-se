---
layout: post
title: C3S and other climate data
categories: wetland-se
excerpt: "Copernicus Climate Change Service"
tags:
  - climate
  - copernicus
  - data
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-01'
modified: '2022-09-28'
comments: true
share: true
---

## Introduction

The [Copernicus Climate Change Service (C3S)](https://cds.climate.copernicus.eu/about-c3s) provide climate [historical data and future climate forecasts](https://climate.copernicus.eu/climate-datasets). C3S combine observations of the climate system with the latest science to develop authoritative, quality-assured information about the past, current and future states of the climate in Europe and worldwide. This post is an overview of the gridded historical climate datasets available for download from C3S [Climate Data Store (CDS)](https://cds.climate.copernicus.eu/cdsapp#!/home) and from [European Climate Assessment & Dataset (ECA&D)](https://www.ecad.eu). Details on how to go about retrieving and organising the different datasets is covered in the other posts mentioned below.

## Prerequisit

The CDS data can be accessed in different ways, but the most efficient is to use [CDS Application Program Interface (API), or CDSAPI](https://cds.climate.copernicus.eu/api-how-to). CDSAPI is a Python package, and you can use it as a stand alone python script for accessing CDS data. But you can also use it embedded in Karttur's GeoImagine Framework. Please see the post on [CDSAPI](../wetland-se_cdsapi/) for details.

## Reanalysis datasets

For mapping and modelling Earth surface processes, the [CDS reanalysis products](https://climate.copernicus.eu/climate-reanalysis) are vital. Climate reanalyses products combine past observations with models to generate consistent time series for a large set of climate variables.

### ERA5

Probably the most widely used of CDS reanalysis products is [ERA5](https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5). ERA5 is a global product and widely used for analysing past climate conditions and trends from the 1950s to the present. This blog includes a [special post on ERA5](../wetland-se_era5/) that details how to access and then process ERA5 using Karttur's GeoImagine Framework.

### CERRA

Copernicus regional reanalysis for Europe (CERRA) is a more detailed reanalysis product. It is developed by the Swedish Meteorological and Hydrological Institute (SMHI) and uses ER5 as a starting point. There is also a [special post on CERRA](../wetland-se_cerra/) in this blog.

### E-OBS

While the Copernicus program is the most important provider of climate data in Europe, there are other organisations that compile climate data that are then offered for free (but with restricted use). One such organisation is the [European Climate Assessment & Dataset (ECA&D)](https://www.ecad.eu). Accessing the data from ECA&D is simpler compared to the Copernicus C3S, but the ECA&D is not as comprehensive. Also the access and organisation of [ECA&D climate data, called E-OBS, is covered in a special post](../wetland-se_e-obs).
