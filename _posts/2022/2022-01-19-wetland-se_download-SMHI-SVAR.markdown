---
layout: post
title: Svenskt Vattenarkiv (SVAR)
categories: wetland-se
excerpt: "Access and download data from the Swedish Meterological and Hydrological Institute (SMHI)"
tags:
  - Sweden
  - SMHI
  - SVAR
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-19'
modified: '2022-01-19'
comments: true
share: true
---

## Introduction

The [Swedish Meterological and Hydrological Institute (SMHI)](https://www.smhi.se) keeps an Open Source database, [Svenskt vattenarkiv, SVAR](https://www.smhi.se/data/hydrologi/svenskt-vattenarkiv). The database includes a [search page](https://www.smhi.se/data/utforskaren-oppna-data/). This post covers how to download and organise some of the SVAR datasets for Karttur's GeoImagine Framework.

## Prerequisites

The download can be done using any internet browser. If you want to use the prepared text files with URLs to the online resources to download and then organise the data you must have [setup Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Svenskt Vattenarkiv (SVAR)

You do not need all the SVAR data for mapping Swedish Wetlands; this post limits the download to data layers covering water surfaces and river basins.

### Search and copy URLs

From the [SVAR Search Open Data (Sök på öppna data) page](https://www.smhi.se/data/utforskaren-oppna-data/) you can either download the layers you want directly, or copy the URLs and paste them in a text file. Each registered layer is well documented, including links to different download formats. The list below (textfile: <span class='file'>SMHI-SVAR-Svenskt_vattenarkiv.txt</span>) links to the URLs of the shape file data for each of the listed layers.

```
https://www.smhi.se/polopoly_fs/1.126762!/Huvudavrinningsomraden_haro_y_2016_3.zip
https://www.smhi.se/polopoly_fs/1.126764!/avrinningsomraden_aro_y_2016_3.zip
https://www.smhi.se/polopoly_fs/1.126766!/Vattenytor_vy_y_2016_3.zip
https://www.smhi.se/polopoly_fs/1.126768!/flodeslinjer_vd_l_2016_3.zip
https://www.smhi.se/polopoly_fs/1.126770!/havsomraden_2016_3.zip
https://www.smhi.se/polopoly_fs/1.126775!/Sj%C3%B6punkter_2016_3.zip
https://www.smhi.se/polopoly_fs/1.134092!/RW_LW_VARO_2016_3c.zip
https://www.smhi.se/polopoly_fs/1.134094!/CW_VARO_2016_3c.zip
https://www.smhi.se/polopoly_fs/1.20772!/Menu/general/extGroup/attachmentColHold/mainCol1/file/ekoreg_2006.zip
https://www.smhi.se/polopoly_fs/1.20770!/Menu/general/extGroup/attachmentColHold/mainCol1/file/damm_p_1995.zip
https://www.smhi.se/polopoly_fs/1.102188!/hela_landet.zip
```

The SVAR data is available as <span class='file'>zip</span> files and organising the data into Karttur's GeoImagine Framework requires three process steps:

- [DownloadAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/),
- [UnZipRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/), and
- [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/).

###  Download SVAR

If you copied and pasted the url's to a text file as described in the previous section, you can use the Framework to download all the layers.

Framework process: [DownloadAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/)

Json command file: [0115-download-SMHI-SVAR.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0115-download-SMHI-SVAR.json/)

```
## Download SMHI SVAR data ##
### REMOVE TO RUN ### 0115-download-SMHI-SVAR.json
```

### UnZip SVAR

As the layers are downloaded as <span class='file'>.zip</span> files, you can use the Framework process [<span class='process'>UnZipRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/) for unzipping.

Framework process: [UnZipRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/)

Json command file: [0120_UnZipRawData_Metria-VMI.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0120_UnZipRawData_Metria-VMI.json/)

```
### Unzip Metria VMI datasets ###
### REMOVE TO RUN ### 0120_UnZipRawData_Metria-VMI.json
```

### Organize SVAR

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160-organize-Metria-NMD.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/projects/projects-0160-organize-Metria-NMD.json/)

```
## Organize SMHI SVAR data ##
### REMOVE TO RUN ### 0160-organize-SMHI-SVAR.json
```
