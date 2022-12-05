---
layout: post
title: Lantmäteriet Base maps
categories: wetland-se
excerpt: "Access and download Swedish vector base maps from Lantmäteriet"
tags:
  - sweden
  - basemap
  - lantmäteriet
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-17'
modified: '2022-10-17'
comments: true
share: true
---

## Introduction

For navigation and adding place names a set of base maps is a starting point. This post is a tutorial for accessing, downloading and organising data from Lantmäteriet (the Swedish mapping, cadastral and land registration authority). [Lantmäteriet's goedataportal](https://www.geodata.se/geodataportalen/srv/swe/catalog.search;jsessionid=931BBE27DE4DF2E5E47B8536A683D3F6#/search?resultType=swe-details&_schema=iso19139*&type=dataset%20or%20series&from=1&to=20) contain the available datasets, including datasets from other government institutions.

## Prerequisites

You do not need any special installations to access and download data from Lantmäteriet, but it might facilitate to use an FTP-client, like [FileZilla](https://filezilla-project.org). If you want to organise the dataset for use with Karttur's GeoImagine Framework, you must have [installed the Framework](https://karttur.github.io/geoimagine03-docs-main/). if you have the Framework installed you can use it for automatic download of the data.

## Lantmäteriet datasets

The openly accessible datasets from Lantmäteriet are (October 2022) available via FTP.

### Create user account

Before you can access data from Lantmäteriet, you need to [create a user account](https://opendata.lantmateriet.se/#register).

### FTP access via Apps

At time of writing this post (January 2022, updated October 2022), the open data from [Lantmäteriet open data](https://www.lantmateriet.se/oppnadata) are accessed using FTP. Dependent on your operating system your file browser (like <span class='app'>Finder</span> in MacOS) might be able to open the FTP server site or not. You will have to give your credentials (user and password). If you can not access FTP sites using the file browser, try the free version of the FTP-client [FileZilla](https://filezilla-project.org).

<figure>
<img src="../../images/filezilla_lmv-opendata.png">
<figcaption> Filezilla connection to ftp://download-opendata.lantmateriet.se</figcaption>
</figure>

### FTP access via Karttur's GeoImagine Framework

Karttur's geoImagine Framework contains a simple FTP process [<span class='process'>AncillaryFtp</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/AncillaryFtp/), you give the host and your credentials as parameters and then a link to a textfile that lists the files you want to download from that FTP site.

Json command file: [0115-download-lantmateriet-FTP.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0115-download-lantmateriet-FTP/)

```
## FTP Download Lantmateriet vector data ##
0115-download-lantmateriet-FTP.json
```

### Organise

After downloading the next step is to organise (import and register) the data in the Framework database. The downloaded data can be saved as any path on your local machine. If you used the prepared command file, the path were to save the data is predefined and the corresponding path is set in the command files below.

The dataset from Lantmäteriet is rather disorganized and the file naming does not really reveal the content. For this project I chose to organise the following layers:

| **source file name (*.shp)** | **path** | **Framework prefix** |
| rutnat | ok_riks_Sweref_99_TM_shape/oversikt/riks | rutnat |
| nv_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | naturereservat |
| ns_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | naturereservat-mindre |
| nl_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | fagelskyddsomrade |
| my_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks |  marktaecke |
| ms_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | sjoar |

to import and register ancillary data in a customised projection system in Karttur's GeoImagine Framework, use the process [<span class='process'>OrganizeProjSysData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/).

#### Organise Lantmäteriet Data

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160-organiseprojsys-lantmateriet.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160-OrganizeProjSysData-lantmateriet/)

```
## Organize Lantmaeteriet data ##
0160-organiseprojsys-lantmateriet.json
```

Running this process will copy the source data defined in the json command file to the predefined structure of the Framework.

| **Dataset** | **Framework path** |
| rutnat | /sweref/lantmateriet/region/rutnat/sweref/0/grid_sweref_sweref_0_riks |
| nv_riks | /sweref/lantmateriet/region/naturskydd/sweref/0/naturereservat_oversikt_sweref_0_riks |
| ns_riks | /sweref/lantmateriet/region/naturskydd/sweref/0/naturereservat-mindre_oversikt_sweref_0_riks |
| nl_riks | /sweref/lantmateriet/region/naturskydd/sweref/0/fagelskyddsomrade2_oversikt_sweref_0_riks |
| my_riks | /sweref/lantmateriet/region/markyta/sweref/0/marktaecke_oversikt_sweref_0_riks |
| ms_riks | /sweref/lantmateriet/region/markyta/sweref/0/sjoar_oversikt_sweref_0_riks |
