---
layout: post
title: Import DEM
categories: wetland-se
excerpt: "Import DEM to Framework"
tags:
  - wetland
  - dem
  - Sweden
  - organize
  - tiling
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-04-02'
modified: '2022-04-02'
comments: true
share: true
---

<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

This post outlines the processing of a very high resolution Digital Elevation Model (DEM), the national Swedish DEM at 2 m spatial resolution. A more comprehensive discussion on DEM and how to process elevation data using Karttur's GeoImagine Framework is covered by the blog [Framework project: Digital Elevation Models (DEMs)](https://karttur.github.io/geoimagine03-proj-copdem/).

## Introduction

Processing DEMs all the way from downloading tiles to create coherent indices and metrics of landscapes, is a typical task for which Karttur's GeoImagine Framework was built. Once the DEM is imported and organized within the Framework, it is easy to test different algorithms and visualizations. If you have access to some ground data you can also apply the Framework for comparing the results of different algorithms and parameters against ground data, and thus select the most appropriate landscape model.

## Introduction



### Import

Once you have retrieved the DEM, you need to prepare and import the dataset to the Framework. A tiled dataset, like the Metria 2m DEM over Sweden, must be prepared as a virtual full coverage dataset prior to importing and then retiling into the (customized) projection system that are used for the actual spatial data processing in the Framework. For the Metria 2 m DEM over Sweden, the sequence of processes thus becomes:

- identification of raw tile bounding boxes (optional),
- mosaic raw tiles,
- organize (import into Framework), and
- tiling to the Framework projection system.

#### Bounding Boxes for raw tiles

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_gs-CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0122_BBoxTiledRawData_metria-DEM.json/)

The process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) creates bounding boxes for each raw tile. For the Swedish 2 m DEM all the branches of the root directory containing the individual DEM tiles is searched (the parameter _getlist_ is set to _oswalk_). As the directory tree also contains supplementary (metadata) <span class='file'>.tif</span> files, the search is set to exclude files containing the string "density" (the parameter _nonpattern_ is set to _density_). The reason that the search is set to excluding a particular pattern is that the actual data tiles do not have any distinguishable naming pattern.

**Note** that the process can take a long time if there are many (thousands) of tiles to loop over. When running this April 2022, the 2 m DEM from Metria is composed of of 73195 tiles. The process generates a single vector data source with all the included tiles. The version of the DEM I had access to in April 2022 does not cover the complete Swedish land mass (see figure below). The no-data holes for large lakes causes further problems when applying the DEM for hydrological modelling.

<figure>
<img src="../../images/MetriaDEM2m_rawtiles_bboxes.png">
<figcaption>73195 bounding boxes for the Metria DEM in 2m over Sweden.</figcaption>
</figure>

#### Mosaic raw tiles

Framework process: [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)

Json command file: [0125_MosaicAncillary_CopDEM-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0125_MosaicAncillary_CopDEM-90m.json/)

Downloaded data that are delivered as arbitrary tiles need to be mosaicked together before importing to the Framework. (SMAP, MODIS and Sentinel standard products do not need mosaicking of raw tiles as these datasets either are downloaded in predefined tiling systems or as complete global datasets). The mosaicking must be done as virtual (<span class='file'>.vrt</span>) dataset. To actually create a true mosaic of Sweden at 2 m spatial resolution would take a long time and also take up unnecessary much disk space. The process [<span class='process'>MosaicAncillary</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/) does not register the imported (organized in the terminology of the Framework) dataset into the database, that is done in the next step.

#### Organize (import and register) ancillary dataset

Framework process: [OrganizeAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeAncillary//)

Json command file: [0160_OrganizeAncillary_CopDEM-90m](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0160_OrganizeAncillary_CopDEM-90m.json/)

With the Copernicus data downloaded, unzipped and mosaicked, the data can be organized for (or imported to) the Framework. For the Copernicus DEM data, the process [_OrganizeAncillary_](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeAncillary//) as defined in the json object below only imports the virtual mosaic linked to the original (downloaded and unzipped) tiles.

Once you have run the json command, you can check in the Framework database, and you should find that the _ancillary_ schema contains a _layer_ with the _compid_ _dem_copdem_. Apart from being defined as an individual layer, the dataset is also registered as a product (table: _compprod_) and a composition (table: _compdef_):

```
SELECT * FROM ancillary.layer WHERE compid = 'dem_copdem';
SELECT * FROM ancillary.compprod WHERE compid = 'dem_copdem';
SELECT * FROM ancillary.compdef WHERE compid = 'dem_copdem';
```

#### Tiling to Framework projection system

Framework process: [TileAncillaryRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileAncillaryRegion/)

Json command file: [0180_TileAncillaryRegion_CopDem-90m.json](https://karttur.github.io/geoimagine03-proj-copdem-json/projects/projects-0180_TileAncillaryRegion_CopDem-90m.json/)

All basic processing in the Framework is done using predefined tiling systems. The process [<span class='process'>TileAncillaryRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileAncillaryRegion/) constructs tiles while also reprojecting the source (_ancillary_) data to the projection system defined under _userproject_: _system_. For the CopDEM data in this example, the data are reprojected and tiled for the _ease2n_ (EASE GRID 2.0 North) projection system.
