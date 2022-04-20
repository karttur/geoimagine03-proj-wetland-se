---
layout: post
title: Datasets
categories: wetland-se
excerpt: "Datasets for mapping Swedish wetlands and their Greenhouse Gas (GHG) balance"
tags:
  - Sweden
  - datasets
  - wetland
  - greenhouse gas
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-06'
modified: '2022-01-06'
comments: true
share: true
---

## Introduction

Sweden has large areas of both intact and drained wetlands. In this project you will access different spatial datasets and combine them for creating wetland maps that relate to the Greenhouse Gas (GHG) balance of Swedish wetlands. This post is a general description of the datasets. The actual retrieval and processing details are given in subsequent posts.

## Datasets

For the project of mapping Swedish wetlands and their climate impact, the following data sets are required:

- Landcover
- Soil moisture
- Wetlands
- Topography
- Ditches
- Geology (incl. organic soils)

### Landcover

Landcover data for Sweden "Nationella Marktäckesdata" (NMD) is freely available 10 m spatial resolution. This data is available from [Metria](https://gpt.vic-metria.nu/data/land/NMD). Wetlands are represented in NMD, with in general "good" or "very good" accuracy.

### Soil moisture

The landcover map (above) also include a soil moisture index product ([Markfuktighetsindex](https://gpt.vic-metria.nu/data/land/NMD)) at 10 m spatial resolution. As this project aims at mapping wetlands and their GHG balance at 10 m spatial resolution that map will suffice. However, recently a more detailed soil moisture product at 2 m spatial resolution] was published by [Ågren et al., 2021](https://doi.org/10.1016/j.geoderma.2021.115280). The more detailed map is available from [Sveriges Lantbruksuniversitet (SLU)](https://www.slu.se), [digital maps and geodata](https://www.slu.se/site/bibliotek/anvanda-biblioteket/soka/digitala-kartor/), but only for approved users.

### Wetlands

The Swedish National Wetland inventory (Våtmarksinveterningen - VMI) is a polygon map layer of Swedish low-land wetlands. The mapping unit in VMI varies from 2 ha (the Baltic Sea islands of Öland och Gotland), 5 ha  (Blekinge and SW Skåne - gamla Malmöhus län), 10 ha for the remaining parts of Southern Sweden (Götaland och Svealand), 15, 20, 25 ha for Värmland and Dalarna, Lappland (Norrland) outside the mountainous region, is mapped at 50 ha. VMI is freely available from [Metria](https://gpt.vic-metria.nu/vmi). Details for downloading using Karttur's GeoImagine framework

### Topography

The national survey of Sweden (METRIA) has produced a national 1 m digital Elevation Model (DEM), also available as a version resampled to 2 m. The DEM is available in tiles, but can not be downloaded as bulk. The 2 m product (GSD-Höjddata, grid 2+) is available [here](https://www.lantmateriet.se/globalassets/kartor-och-geografisk-information/hojddata/hojd2_plus_2.8.pdf). We need to identify a source where we can retrieve the Swedish DEM data in 2m.

### Ditches

Ditches derived from a re-analysis of the LiDAR data used for producing the Swedish DEM has been published in late 2021. The ditches are avalable as 2.5 km x 2.5 grid maps at 1 km spatial resolution from [skogsstyrelsen](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/ftp/). The actual data is available using ftp [ftps://ftpsks.skogsstyrelsen.se](ftps://ftpsks.skogsstyrelsen.se).

### Soils

The [Swedish Geological Survey (SGU)](https://www.sgu.se) have map viewers (kartvisare) for both a [general soil map](https://apps.sgu.se/kartvisare/kartvisare-jordarter-25-100.html) and a map for [organic soils (torv)](https://apps.sgu.se/kartvisare/kartvisare-torv.html). The layers are only available for viewing, and need to be accessed as spatial data layers by request.

### Svenskt Vatten Arkiv (SVAR)

[Sveriges Meterologiska och Hydrologiska institut (SMHI)](www.smhi.se) hosts an online repo called Swedish Water Archive [Svenskt Vattenarkiv (SVAR)](https://www.smhi.se/data/hydrologi/svenskt-vattenarkiv). Downloadable data from SVAR is available [here](https://www.smhi.se/data/hydrologi/sjoar-och-vattendrag/ladda-ner-data-fran-svenskt-vattenarkiv-1.20127). This data is only required if you want to geographically restrict your analysis based on drainage areas.

### Trafikverket (TRV)

Internal spatial datasets produced by TRV, including

- kontaktsträckor mellan väg/järnväg och SMHIs våtmarker
- Ostlänken sträckning

## Hydrological modelling

The original work behind this project was to produce tools for Trafikverket (TRV). As TRV is already using a hydrological model ([flödesapp.se](https://flöde.se)) this project will use the same model. Flödesapp is a proprietary model based on regionalised runoff coefficients calculated from default runoff coefficients.
