---
layout: post
title: Lantmaeteriet Base maps
categories: wetland-se
excerpt: "Access and download Swedish vector base maps from Lantmäteriet"
tags:
  - sweden
  - basemap
  - lantmäteriet
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-17'
modified: '2022-01-17'
comments: true
share: true
---

## Introduction

For navigation and adding place names a set of base maps is a starting point. This post is a tutorial for accessing, downloading and organising data from Lantmäteriet (the Swedish mapping, cadastral and land registration authority).

## Prerequisites

You do not any any special installations to access and download data from Lantmäteriet, but it might facilitate to use an FTP-client, like [FileZilla](https://filezilla-project.org). If you want to organise the dataset for use with Karttur's GeoImagine Framework, you must have [installed the Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Download

The openly accessible datasets from Lantmeateriet are (January 2022) available via FTP. There is no particular process for FTP access included in Karttur's GeoImagine Framework. Instead you have to use an FTP-client, like [FileZilla](https://filezilla-project.org).

### Create user account

Before you can access data from Lantmäteriet, you need to [create a user account](https://opendata.lantmateriet.se/#register) also for accessing the [opendata](https://www.lantmateriet.se/sv/Kartor-och-geografisk-information/oppna-data/).

### FTP access

At time of writing this post (January 2022), the open data from Lantmäteriet is accessed using FTP: [ftp://download-opendata.lantmateriet.se](ftp://download-opendata.lantmateriet.se). Dependent on your operating system your file browser (like <span class='app'>Finder</span> in MacOS) might be able to open the FTP server site or not. You will have to give your credentials (user and password). If you can not access FTP sites using the file browser, try the free version of the FTP-client [FileZilla](https://filezilla-project.org).

<figure>
<img src="../../images/filezilla_lmv-opendata.png">
<figcaption> Filezilla connection to ftp://download-opendata.lantmateriet.se</figcaption>
</figure>

## Organise

The dataset from Lantmeateriet is rather disorganized and the file naming does not really reveal the content. For this project I chose to organise the following layers:

| source file name (*.shp) | path | prefix |
| rutnat | ok_riks_Sweref_99_TM_shape/oversikt/riks | runat |
| nv_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | naturereservat |
| ns_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | naturereservat-mindre |
| nl_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | fagelskyddsomrade |
| my_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks |  marktaecke |
| ms_riks | ok_riks_Sweref_99_TM_shape/oversikt/riks | sjoar |

### Organise Projection System Data

Framework process: [OrganizeProjSysData](#)

```
## Organize Lantmaeteriet data ##
### REMOVE TO RUN ### 0160-organiseprojsys-lantmateriet.json
```
