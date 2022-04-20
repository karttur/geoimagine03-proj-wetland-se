---
layout: post
title: Metria thematic map layers
categories: wetland-se
excerpt: "Access and download data from Metria"
tags:
  - sweden
  - metria
  - maps
  - download
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-18'
modified: '2022-01-18'
comments: true
share: true
---

## Introduction

[Metria](https://metria.se) is a company owned by the Swedish government. Metria provides Geospatial data, maps and services both as Open Source and commercially for customized products. For the project of Swedish wetlands we need to access some of the [freely available data](https://gpt.vic-metria.nu/data/), including:

- National Land Cover (Nationella Marktäckesdata, NMD)
- National Wetland Inventory (Våtmarksinveteringen, VMI)

This post covers how to download and organise these datasets for Karttur's GeoImagine Framework.

## Prerequisites

The download can be done using any internet browser. If you want to use the prepared text files with URLs to the online resources to download and then organise the data you must have [setup Karttur's GeoImagine Framework](#).

## Download

Downloading is done separately for the National Land Cover (NMD) and the wetland inventory (VMI).

### National Land Cover (NMD)

The Swedish National Land Cover dataset is a regularly updated and high quality product. It also includes wetlands, and these are generally mapped with a "high" to "very high" accuracy. For the project of mapping Swedish wetlands you need to download 4 layers from NMD:

- base layer (non generalised) [NMD2018_basskikt_ogeneraliserad_Sverige_v1_1.zip]
- Add on land use layer [NMD_Tillaggsskikt_Markanvandning.zip]
- Mountainous forest [NMD_Lag_fjallskog_v1_1.zip]
- Soil moisture

The complete URL to these 4 layers are given in the text file <span class='file'>Metria-Svenska_markdata.txt</span>:

```
https://gpt.vic-metria.nu/data/land/NMD/NMD2018_basskikt_ogeneraliserad_Sverige_v1_1.zip
http://gpt.vic-metria.nu/data/land/NMD/NMD_Tillaggsskikt_Markanvandning.zip
https://gpt.vic-metria.nu/data/land/NMD/NMD_Lag_fjallskog_v1_1.zip
https://gpt.vic-metria.nu/data/land/NMD/Markfuktighetsindex_NMD_Sverige.zip
```

The data can be downloaded using the text file above and Kattur's GeoImagine Framework process <span class='process'>DownloadAncillary</span>. The example below points towards the text file <span class='file'>Metria-Svenska_markdata.txt</span> and downloads the data to a subfolder under the location of the text file.

### Download Ancillary

Framework process: [DownloadAncillary](#)

```
### Download Metria baseline data ###
### REMOVE TO RUN ### 0115-download-Metria-markdata.json
```

As only four (4) files are downloaded it is rather easy to unzip them manually.

### National Wetland Inventory (VMI)

The Swedish National Wetland inventory (Våtmarksinveterningen - VMI) is a polygon map layer of Swedish low-land wetlands. The mapping unit in VMI varies from 2 ha (the Baltic Sea islands of Öland och Gotland), 5 ha  (Blekinge and SW Skåne - former Malmöhus län), 10 ha for the remaining parts of Southern Sweden (Götaland and Svealand), 15, 20, 25 ha for Värmland and Dalarna, Lappland (Norrland) outside the mountainous region, is mapped at 50 ha. The map is available as a national polygon map and more detailed county maps. The complete list of available URLs (February 2022):

```
https://gpt.vic-metria.nu/vmi/VMI_ytor.zip
https://gpt.vic-metria.nu/vmi/AB_Stockholm_VMI.zip
https://gpt.vic-metria.nu/vmi/AC_Vasterbotten_VMI.zip
https://gpt.vic-metria.nu/vmi/BD_Norrbotten_VMI.zip
https://gpt.vic-metria.nu/vmi/C_Uppsala_VMI.zip
https://gpt.vic-metria.nu/vmi/D_Sodermanland_VMI.zip
https://gpt.vic-metria.nu/vmi/E_Ostergotland_VMI.zip
https://gpt.vic-metria.nu/vmi/F_Jonkoping_VMI.zip
https://gpt.vic-metria.nu/vmi/G_Kronoberg_VMI.zip
https://gpt.vic-metria.nu/vmi/H_Kalmar_VMI.zip
https://gpt.vic-metria.nu/vmi/I_Gotland_VMI.zip
https://gpt.vic-metria.nu/vmi/K_Blekinge_VMI.zip
https://gpt.vic-metria.nu/vmi/M_Skane_VMI.zip
https://gpt.vic-metria.nu/vmi/N_Halland_VMI.zip
https://gpt.vic-metria.nu/vmi/O_VastraGotaland_VMI.zip
https://gpt.vic-metria.nu/vmi/S_Varmland_VMI.zip
https://gpt.vic-metria.nu/vmi/T_Orebro_VMI.zip
https://gpt.vic-metria.nu/vmi/U_Vastmanland_VMI.zip
https://gpt.vic-metria.nu/vmi/W_Dalarna_VMI.zip
https://gpt.vic-metria.nu/vmi/X_Gavleborg_VMI.zip
https://gpt.vic-metria.nu/vmi/Y_Vasternorrland_VMI.zip
https://gpt.vic-metria.nu/vmi/Z_Jamtland_VMI.zip
```
### Download Ancillary

Framework process: [DownloadAncillary](#)

```
### Download Metria Svenska Våtmarksinventeringen (VMI) ###
### REMOVE TO RUN ### 0115-download-Metria-VMI.json
```

### UnZip Raw Data

Framework process: [UnZipRawData](#)


With lots of zip files you can use the Framework process <span class='process'>UnZipRawData</span> for unzipping.

```
### Unzip Metria VMI datasets ###
### REMOVE TO RUN ### 0120_UnZipRawData_Metria-VMI.json
```

## Organise
