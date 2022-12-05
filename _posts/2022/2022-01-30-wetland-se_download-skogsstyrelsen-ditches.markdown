---
layout: post
title: Access SLU/Skogsstyrelsen ditches
categories: wetland-se
excerpt: "Access and download data over ditches created by Sveriges Lantbruksuniversitet (SLU) and available from Skogsstyrelsen."
tags:
  - Sweden
  - SLU
  - skogsstyrelsen
  - ditches
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-30'
modified: '2022-01-30'
comments: true
share: true
---

## Introduction

Skogsstyrelsen (The Swedish Forest Agency) hosts a set of spatial datasets, both [open source data](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/oppna-data/) and data that requires registering for accessing. The former includes a [national ditch layer at 0.5 and 1 m spatial resolution developed by Sveriges Lantbruksuniversitet (Swedish University of Agricultural Sciences)](https://www.slu.se/institutioner/skogens-ekologi-skotsel/forskning2/dikeskartor/om-dikeskartorna/). This dataset is a key layer for modelling Swedish wetlands and this post is an instruction for how to retrieve it and get it into Karttur´s GeoImagine Framework.

For details on accessing the data from Skogsstyrelsen FTP site, including how to register a user, please see the [previous post on Access SLU/Skogsstyrelsen soil moisture](../wetland-se_download-skogsstyrelsen-sm/).

## Prerequisites

You do not need any special installations to access and download data from Skogsstyrelsen, but it might facilitate to use an FTP-client, like [FileZilla](https://filezilla-project.org). If you want to organise the dataset for use with Karttur's GeoImagine Framework, you must have [installed the Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Skogsstyrelsen ditch datasets

There are two separate datasets over ditches available from Skogsstyrelsen, a dataset at 1 m spatial resolution covering almost the entire Swedish landmass, and a 0.5 m dataset covering approximately half the country. The latter dataset is reported to have a higher accuracy (86 %) compared to the former (82 %).

### Download ditch datasets

Get the ditch datasets with an FTP client as explained in the post on [Access SLU/Skogsstyrelsen soil moisture](../wetland-se_download-skogsstyrelsen-sm-sm/). To use the prepared scripts and command files of this post, you need to save the downloaded data as follows:

| dataset | path |
| DikenRaster_1m | /DOWNLOADS/skogsstyrelsen/DikenRaster_1m |
| DikenRaster_05m | /DOWNLOADS/skogsstyrelsen/DikenRaster_05m |

### Bounding boxes

The SLU ditch datasets are delivered as tiles. The Framework process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) can be used for retrieving the boundary polygons of tiled raw data. The output of the process is a vector file.
The process does not interact with the Framework database but is merely an opportunity for visually inspecting raw data prior to further processing.

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_slu-diken](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0122_BBoxTiledRawData_slu-diken.json/)

```
## Create vector bounding boxes the ditch tiles ##
0122_BBoxTiledRawData_slu-diken.json
```

The figure below shows the bounding boxes for the SLU ditch datasets.

<figure class="third">
<img src="../../images/bboxtiles_slu-ditches-1m_se.png">
<img src="../../images/bboxtiles_slu-ditches-05m_se.png">
<img src="../../images/bboxtiles_slu-ditches_gotland.png">
<figcaption> Bounding boxes for Swedish national ditch dataset; left: the 75648 tiles composing the 1m national dataset, center: tiles of  the 0.5 m resolution dataset, and right: the tiles of the 1 m dataset for the Baltic Sea island of Gotland.</figcaption>
</figure>

### Mosaic the raw ditches

Because the SLU ditch dataset is delivered as tiles, you need to create virtual mosaics prior to organising them for the Framework.

Framework process: [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)

Json command file: [0125_MosaicAncillary_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0125_MosaicAncillary_slu-diken/)

```
## Mosaic the raw ditch tile datasets ##
0125_MosaicAncillary_slu-diken.json
```

The virtual mosaics will just be used as an intermediate layer for creating the SWEREF tiles to be used for the actual processing later. The next step is to import and register (organise) the virtual mosaics.

### Organize ditches

The layers with the SLU ditches are imported (organized) by using the virtual mosaics created in the previous section. In essence this equals importing a single, virtual layer rather than the original tiles.

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160_Organizeprojsys_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160_Organizeprojsys_slu-diken/)

```
## Import virtual mosaics of the ditch tile datasets ##
0160_Organizeprojsys_slu-diken.json
```

### Tile ditches

The imported ditch datasets are virtual datasets that point towards the original GeoTiff tiles. Applying the process [<span class='process'>TileProjSysRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/) will result in a new set of GeoTiff tiles, but this time fitted to the default tiling system as defined for the Framework projecting system SWEREF99, that was created in the post [Default region Sweden](../wetland-se_defregion).

Framework process: [TileProjSysRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

Json command file: [0180_Tileprojsysregion_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0180_TileProjSysRegion_slu-diken/)

```
## tile ditch datasets to sweref ##
0180_TileProjSysRegion_slu-diken.json
```

And with that you have now added the two ditch datasets as GeoTiff ready to use Karttur's GeoImagine Framework.
