---
layout: post
title: Depth To Water (DTW)
categories: wetland-se
excerpt: "Organize and tile the Depth To Water soil moisture layer from Skogsstyrelsen."
tags:
  - sweden
  - metria
  - maps
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-26'
modified: '2022-01-26'
comments: true
share: true
---

## Introduction

This post covers how to import the soil moisture / Depth To Water layer available via [Skogsstyrelsen FTP site](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/ftp/). If you followed the steps in the previous post on [SLU/Skogsstyrelsen soil moisture](../wetland-se_download-skogsstyrelsen-sm-sm) you should have the tiles of the Depth To Water layer downloaded on your local machine.

## Prerequisites

To follow the processing in this post you must have [installed Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/) and downloaded the Depth To Water tile dataset as described in the [previous post](../wetland-se_download-soil-moisture).

## Processes

To get the Depth To Water dataset prepared for Karttur's GeoImagine Framework the following steps are required:

- [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)
- [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)
- [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)
- [TileProjSysRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

### Bounding boxes for tiled datasets

For datasets delivered as tiles, like the Depth To Water dataset, the Framework process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) can be used for retrieving the boundary polygons of the tiles. The output of the process is a vector file.
The process does not interact with the Framework database but is merely an opportunity for visually inspecting raw data prior to further processing.

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_slu-dtw](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0122_BBoxTiledRawData_slu-dtw/)

```
## Create vector bounding boxes for the depth to groundwater (dtw) tiles ##
0122_BBoxTiledRawData_slu-dtw.json
```

### Mosaic the raw tiles

Because the Depth To Water dataset is delivered as tiles, you need to create a virtual mosaic prior to importing and registering it for the Framework database.

Framework process: [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)

Json command file: [0125_MosaicAncillary_slu-dtw.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0125_MosaicAncillary_slu-dtw.json/)

```
## Mosaic depth to water tiles ##
0125_MosaicAncillary_slu-dtw.json
```

The mosaic will just be used as an intermediate layer for creating the SWEREF tiles to be used for the actual processing later. The next step is thus to import the virtual mosaic into the Framework.

### Import and register

The layer with Depth To Water is imported and registered (organized) by using the virtual mosaic created in the previous section. In essence this equals importing a single, virtual layer rather than the original tiles.

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160_Organizeprojsys_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160_Organizeprojsys_slu-dtw/)

```
## Organise depth to water ##
0160_Organizeprojsys_slu-dtw.json
```

### Tile to SWEREF99 system

The virtual mosaic of Depth To Water is represented as an _ancillary_ composition and layer in the Framework database. but we want to use it within the projection system SWEREF (that we created in the post [Default region Sweden](../wetland-se_defregion)). In the jargon of Karttur´s GeoImagine Framework we want to tile to a project system region ([<span class='process'>TileProjSysRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)).

Framework process: [TileProjSysRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

Json command file: [0180_TileProjSysRegion_slu-dtw.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0180_TileProjSysRegion_slu-dtw/)

```
## tile depth to water ##
0180_TileProjSysRegion_slu-dtw.json
```

The data is now ready for further processing in Karttur's GeoImagine Framework.
