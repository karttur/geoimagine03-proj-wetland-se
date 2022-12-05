---
layout: post
title: Access SLU/Skogsstyrelsen soil moisture
categories: wetland-se
excerpt: "Access and download data over soil moisture created by Sveriges Lantbruksuniversitet (SLU) and available from Skogsstyrelsen."
tags:
  - Sweden
  - SLU
  - skogsstyrelsen
  - soil moisture
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-24'
modified: '2022-11-10'
comments: true
share: true
---

## Introduction

[The Swedish University of Agricultural Sciences (SLU)](https://www.slu.se) has developed a series of maps related to [soil moisture](https://doi.org/10.1016/j.geoderma.2021.115280) and [ditches](https://meetingorganizer.copernicus.org/EGU21/EGU21-2226.html) at very high spatial resolution. The maps are available for ordering from the [SLU digital map archive](https://www.slu.se/site/bibliotek/soka-och-lana/soka/digitala-kartor/). The same datasets, however, are more easily and direct accessed using FTP from [Skogsstyrelsen (The Swedish Forest Agency)](https://www.skogsstyrelsen.se).

The organisation of the data at both SLU and Skogsstyrelsen has been changing often during the past year. In October 2022, the useful sites describing the data and how to access it include:

- [Skogsstyrelsen Open data source](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/oppna-data/)
- [Skogsstyrelsen Geodata to use in your own GIS](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/)
- [Skogsstyrelsen FTP - download geodata](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/ftp/)

The latter states the url and credentials (user and password) to use for accessing the data using FTP. As the access, as well as content and organisation have been changing so frequently I am not giving any details in this text. The associated scripts and files listing url.s reflect the situation in October 2022.

This post focuses on the soil moisture data available via Skogsstyrelsens FTP site; ditches are covered in a [separate post]()

## Prerequisites

You do not need any special installations to access and download data from Skogsstyrelsen, but it might facilitate to use an FTP-client, like [FileZilla](https://filezilla-project.org). If you want to organise the dataset for use with Karttur's GeoImagine Framework, you must have [installed the Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Skogsstyrelsen soil moisture and streams datasets

The openly accessible datasets from Skogsstyrelsen are (October 2022) available via FTP. The following soil moisture products are available:

- /Markfuktighet_mlwam/Demoområden
- /Markfuktighet_mlwam/Vattendragsnätverk/Shape
- /SLUMarkfuktighet/SLUMarkfuktighetskarta
- /SLUMarkfuktighet/SLUMarkfuktighetskartaKlassad
- /SLUMarkfuktighet/Streams/10ha
- /SLUMarkfuktighet/Streams/2ha
- /SLUMarkfuktighet/Streams/30ha


At time of writing this update (October 2022), there is one more soil moisture dataset available, under the old FTP site:
```
ftps://ftpsks.skogsstyrelsen.se
```
The soil moisture dataset at this ftp site covers [depth to water (DTW)](https://www.skogsstyrelsen.se/globalassets/sjalvservice/karttjanster/geodatatjanster/teknisk-beskrivning/raster-markfuktighetskarta-dtw---teknisk-beskrivning.pdf).
I have included it in the script and parameter files as _soilmoisture-DTW_.

There are also datasets on ditches ("diken" in Swedish), but they are handled in the post on [Ditches](../wetland-se_download-ditches).

There is no particular process for FTPS access included in Karttur's GeoImagine Framework (the older FTP standard, however works). Instead you have to use an FTP-client, like [FileZilla](https://filezilla-project.org).

### Create user account (maybe)

When the original post was written [you had to request a user account](https://www.skogsstyrelsen.se/sjalvservice/karttjanster/geodatatjanster/skaffa-anvandarkonto/) with Skogsstyrelsen to access the data. But when updating that was not required. The page for requesting an account, however, was still active.

### FTP access

Please look at the links linked above to find out the actual active FTP sites.

Dependent on your operating system your file browser (like <span class='app'>Finder</span> in MacOS) might be able to open the FTP server site or not. You will have to give some credentials (user and password). If you can not access FTP sites using the file browser, try the free version of the FTP-client [FileZilla](https://filezilla-project.org).

Get the soil-moisture data you are interested in with the FTP client. To use the prepared scripts and command files of this post, you need to save the downloaded data as follows:

| dataset | path |
| SLUMarkfuktighetskarta | /DOWNLOADS/skogsstyrelsen/SLUMarkfuktighetskarta |
| SLUMarkfuktighetskartaKlassad | /DOWNLOADS/skogsstyrelsen/SLUMarkfuktighetskartaKlassad |
| SLUMarkfuktighetskarta-DTW | /DOWNLOADS/skogsstyrelsen/SLUMarkfuktighetskarta-DTW |
| SLUstreams-2ha |  /DOWNLOADS/skogsstyrelsen/Streams/2ha |
| SLUstreams-10ha | /DOWNLOADS/skogsstyrelsen/Streams/10ha |
| SLUstreams-30ha | /DOWNLOADS/skogsstyrelsen/Streams/30ha |

### Organise and tile soil moisture

The soil moisture and stream data from SLU/Skogsstyrelsen are delivered as either complete raster layer for the whole of Sweden, or tiles.

| dataset | deliver format |
| SLUMarkfuktighetskarta | single layer GeoTiff |
| SLUMarkfuktighetskartaKlassad | single layer GeoTiff |
| SLUMarkfuktighetskarta-DTW | tiles |
| SLUstreams-2ha | tiles |
| SLUstreams-10ha | tiles |
| SLUstreams-30ha | tiles |

To get the data prepared for Karttur's GeoImagine Framework the following steps are required:

- [BBoxTiledRawData (for tile delivered data)](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)
- [MosaicAncillary (for tile delivered data)](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-MosaicAncillary/)
- [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)
- [TileProjSysRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

### Bounding boxes for tiled datasets

For the datasets delivered as tiles (see table above), the Framework process [<span class='process'>BBoxTiledRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/) can be used for retrieving the boundary polygons of the tiles. The output of the process is a vector file.
The process does not interact with the Framework database but is merely an opportunity for visually inspecting raw data prior to further processing.

Framework process: [BBoxTiledRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-BBoxTiledRawData/)

Json command file: [0122_BBoxTiledRawData_slu-diken](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0122_BBoxTiledRawData_slu-diken/)

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

At 1 m spatial resolution, consisting of 75648 tiles source tiles and covering the whole of Sweden, the mosaic is too heavy to view. It will just be used as an intermediate layer for creating the SWEREF tiles to be used for the actual processing later. The next step is thus to import the virtual mosaic into the Framework.

# MOVE TO CORRECT POST
### Organize SLU ditches

The layer with the SLU ditches is imported (organized) by using the virtual mosaic created in the previous section. In essence this equals importing a single, virtual layer rather than the 75648 original tiles.

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160_Organizeprojsys_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160_Organizeprojsys_slu-diken/)

```
## Import 1 m ditch tiles ##
### REMOVE TO RUN ### 0160_Organizeprojsys_slu-diken.json
```

### Tile SLU ditches

The virtual mosaic of the SLU ditches is represented as an _ancillary_ composition and layer in the Framework database. but we want to use it within the projection system SWEREF (that we created in the post [Default region Sweden](../wetland-se_defregion)). In the jargon of Karttur´s GeoImagine Framework we want to tile to a project system region ([<span class='process'>TileProjSysRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)).

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

Json command file: [0180_Tileprojsysregion_slu-diken.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0180_Tileprojsysregion_slu-diken/)

```
## tile 1 m ditch to sweref ##
### REMOVE TO RUN ### 0180_Tileprojsysregion_slu-diken.json
```
