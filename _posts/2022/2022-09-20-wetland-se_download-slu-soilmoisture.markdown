---
layout: post
title: SLU soil moisture
categories: wetland-se
excerpt: "Access and download data over soil moisture created by Sveriges Lantbruksuniversitet (SLU) and available from Skogsstyrelsen."
tags:
  - Sweden
  - SLU
  - skogsstyrelsen
  - soil moisture
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-20'
modified: '2022-09-20'
comments: true
share: true
---

## Introduction

The Swedish Agricultural University (SLU) has both produced high resolution maps over Sweden, and have an [access point](https://www.slu.se/site/bibliotek/soka-och-lana/soka/digitala-kartor/) for Swedish map data from other government institutions. The maps produced by SLU include [soil moisture datasets](https://www.slu.se/institutioner/skogens-ekologi-skotsel/forskning2/markfuktighetskartor/om-slu-markfuktighetskarta/) created by  by combining digital terrain models, climate and soil maps with Artificial Intelligence.

The soil moisture datasets are available from Skogsstyrelsen (The Swedish Forest Agency). Skogsstyrelsen hosts both[open source data](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/oppna-data/) and data that requires registering for accessing. The former includes the [soil moisture datasets](https://www.slu.se/institutioner/skogens-ekologi-skotsel/forskning2/markfuktighetskartor/om-slu-markfuktighetskarta/). The same datasets are also available from [Metria](https://metria.se/kunskap/markfuktighetskarta-hela-sverige/).

Soil moisture is a key layer for modelling Swedish wetlands and this post is an instruction for how to retrieve it and get it into Karttur´s GeoImagine Framework.

## Prerequisites

You do not need any special installations to access and download data from Lantmäteriet, but it might facilitate to use an FTP-client, like [FileZilla](https://filezilla-project.org). If you want to organise the dataset for use with Karttur's GeoImagine Framework, you must have [installed the Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Skogsstyrelsen datasets

The openly accessible datasets from Skogsstyrelsen are (January 2022) available via FTP. There is no particular process for FTP access included in Karttur's GeoImagine Framework. Instead you have to use an FTP-client, like [FileZilla](https://filezilla-project.org).

### Create user account

To access all the spatial datasets available from Skogsstyrelsen you need to [request a user account](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/skaffa-anvandarkonto/).

### FTP access

At time of writing this post (January 2022), the open data from Skogsstyrelsen are accessed using FTP: [https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/ftp/](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/ftp/). Downloading requires a login and password, but for the open data these are given in the url link.

When updating this in October 2022, the actual url and credentials are:

```
Adress: ftps://ftpsks.skogsstyrelsen.se
Användarnamn: SGD
Lösenord: 0N!nd=I9EJ
```

Dependent on your operating system your file browser (like <span class='app'>Finder</span> in MacOS) might be able to open the FTP server site or not. You will have to give your credentials (user and password). If you can not access FTP sites using the file browser, try the free version of the FTP-client [FileZilla](https://filezilla-project.org).

The datasets available from Skogsstyrelsen that are of direct interest for mapping wetlands are primarily the ditches and the depth to groundwater table. There are several datasets of ditches available of which the best is the SLU version.

### Bounding boxes for SLU ditch Tiles

The SLU ditches dataset is delivered as tiles. The Framework process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) can be used for retrieving the boundary polygons of tiled raw data. The output of the process is a vector file.
The process does not interact with the Framework database but is merely an opportunity for visually inspecting raw data prior to further processing.

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_slu-diken](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0122_BBoxTiledRawData_slu-diken.json/)

```
## Create vector bounding boxes for the 1 m ditch tiles
### REMOVE TO RUN ### 0122_BBoxTiledRawData_slu-diken.json
```

The figure below shows the bounding boxes for the SLU ditch dataset; national coverage to the left and the coverage for the Baltic Sea island of Gotland to the right.

<figure class="half">
<img src="../../images/bboxtiles_slu-ditches_se.png">
<img src="../../images/bboxtiles_slu-ditches_gotland.png">
<figcaption> Bounding boxes for Swedish national ditch dataset; left: the 75648 tiles composing the national dataset, right: the same dataset for the Baltic Sea island of Gotland.</figcaption>
</figure>

### Mosaic the raw SLU ditches

Because the SLU ditch dataset is delivered as tiles, you need to create a virtual mosaic prior to organizing it for the Framework database.

Framework process: [MosaicAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)

Json command file: [0125_MosaicAncillary_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0125_MosaicAncillary_slu-diken.json/)

```
## Mosaic 1 m ditch tiles ##
### REMOVE TO RUN ### 0125_MosaicAncillary_slu-diken.json
```

At 1 m spatial resolution, consisting of 75648 tiles source tules and covering the whole of Sweden, the mosaic is too heavy to view. It will just be used as an intermediate layer for creating the SWEREF tiles to be used for the actual processing later. The next step is thus to import the virtual mosaic into the Framework.

### Organize SLU ditches

The layer with the SLU ditches is imported (organized) by using the virtual mosaic created in the previous section. In essence this equals importing a single, virtual layer rather than the 75648 original tiles.

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160_Organizeprojsys_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0160_Organizeprojsys_slu-diken.json/)

```
## Import 1 m ditch tiles ##
### REMOVE TO RUN ### 0160_Organizeprojsys_slu-diken.json
```

### Tile SLU ditches

The virtual mosaic of the SLU ditches is represented as an _ancillary_ composition and layer in the Framework database. but we want to use it within the projection system SWEREF (that we created in the post [Default region Sweden](../wetland-se_defregion)). In the jargon of Karttur´s GeoImagine Framework we want to tile to a project system region ([<span class='process'>TileProjSysRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)).

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

Json command file: [0180_Tileprojsysregion_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0180_Tileprojsysregion_slu-diken.json/)

```
## tile 1 m ditch to sweref ##
### REMOVE TO RUN ### 0180_Tileprojsysregion_slu-diken.json
```

## References

Ågren, A. M., J. Larson, S. S. Paul, H. Laudon, and W. Lidberg (2021) Use of multiple LIDAR-derived digital terrain indices and machine learning for high-resolution national-scale soil moisture mapping of the Swedish forest landscape. Geoderma, 404, 115280, https://doi.org/10.1016/j.geoderma.2021.115280
