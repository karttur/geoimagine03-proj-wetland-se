---
layout: post
title: Default region Sweden
categories: wetland-se
excerpt: "Create custom projection for SWEREF99 and set a default region for the Swedish land area"
tags:
  - Sweden
  - default region
  - projection system
  - SWEREF99
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-09'
modified: '2022-01-09'
comments: true
share: true
---

## Introduction

If you want to work with a specific projection system (typically a national system) and a specific region (typically a nation) in Karttur's GeoImagine Framework, you must first define the projection system, then the geographical region and a user with a project that have access to the defined region. Optionally you can define a geographic sub-region (e.g. national land mass). All processes involving geospatial data in the Framework require that the process itself is opened up for handling that particular system. Thus you must also add any additional projection system to the database that recognise the system as accessibility for the processes required. The first actual spatial process to run is defining the tiles for your newly created projection system. For more details on setting up a custom projection system see the blog on [Karttur projects: Setup custom projection system](https://karttur.github.io/geoimagine03-docs-setup-projsys/). Different posts in that blog are referred to in the instructions below.

## Setup database

For a projection system to be applicable in the Framework, the data (regions, tiles, compositions, layers etc) belonging to the system must be defined in its own database _schema_ and all the system specific processes must be opened up for the novel projection system. The post [Projection system setup:
Part 1 setup_db](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup/setup-projsys-sweref-db/) is a detailed manual on how that is done, using the national Swedish projection system, SWEREF99, as an example.

## Setup processes

With the required schema and tables added to the postgreSQL database, you must run a set of Framework processes for completing the definition of the projection system, create a tiling system, add a default region and create a user project based on the new projection system. The post [Projection system setup:
Part 2 setup_processes](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup/setup-projsys-sweref-processes/) outlines the processes in detail and also links to the actual command files for completing the setup of SWEREF99.

## Update database

The functions of Karttur’s GeoImagine Framework are built up using processes. All processes are linked to specific types of data or data-sources. When creating a new projection system you must thus update the Framework postgres database to allow selected processes to access the datasets belonging to the new projection system. To update the Framework to allow processing SWEREF99 data, follow the instructions in the post on [Projection system setup: Part 3 update db](https://karttur.github.io/geoimagine03-docs-setup-projsys/setup/setup-projsys-sweref-db-update/). it uses SWEREF99 as an example.

## Define boundary region and tiles

The region covered by the national Swedish projection and grid system (SWEREF99) is rectangular in shape. Not all tiles in the grid system intersect with the Swedish national boundary (see figure below). The datasets that you will later use thus do not intersect with all the tiles. To avoid working with a lot of no-data tiles, you need to create a default region for the system sweref that only represent the national land mass of Sweden. From that region you then need to extract only those tiles that intersect with it.

You can use any dataset representing the Swedish national boundary for defining the default region. But the simplest is to use the global national boundary dataset that is included by default in Karttur's Geoimagine Framework. If you did [setup the complete Framework](https://karttur.github.io/geoimagine03-docs-main/), the national boundary of Sweden is already available within the Framework, albeit projected to geographical coordinates (ESPG: 4326). Import the national boundary to the sweref projection system with process [<span class='process'>ReprojectDefaultRegionProjSys</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-ReprojectDefaultRegionProjSys/), then identify the intersecting tiles with the process [<span class='process'>LinkDefaultRegionTiles</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-LinkDefaultRegionTiles/).

### Reproject Default Region to Projection System

Framework process: [ReprojectDefaultRegionProjSys](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-ReprojectDefaultRegionProjSys/)

Json command file: [0193-ReprojectDefaultRegionProjSys-se.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0193-ReprojectDefaultRegionProjSys-se/)

```
## Import Sweden national boundaries from system default **
0193-ReprojectDefaultRegionProjSys-se.json
```

### Link Default Region Tiles

Framework process: [LinkDefaultRegionTiles](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-LinkDefaultRegionTiles/)

Json command file: [0199-Linkdefaultregiontiles-sweref.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0199-Linkdefaultregiontiles-sweref/)

```
## Link Sweden national boundary to project default tiles
0199-Linkdefaultregiontiles-sweref.json
```

<figure>
<img src="../../images/sweref_epsg3006_boundary_se.png">
<figcaption>Boundary of the sweref projection system compared with Sweden national terrestrial and maritime boundaries.</figcaption>
</figure>
