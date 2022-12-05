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

- National Land Cover (Nationella Marktäckesdata, NMD), and
- National Wetland Inventory (Våtmarksinveteringen, VMI)

This post covers how to download and organise these datasets for Karttur's GeoImagine Framework.

## Prerequisites

The download can be done using any internet browser. If you want to use the prepared text files with URLs to the online resources to download and then organise the data you must have [setup Karttur's GeoImagine Framework](https://karttur.github.io/geoimagine03-docs-main/).

## Download

Downloading is done separately for the National Land Cover (NMD) and the wetland inventory (VMI).

### National Land Cover (NMD)

The Swedish National Land Cover dataset is a regularly updated and high quality product. Land cover also includes a wetland class, and wetlands are generally mapped with a "high" to "very high" accuracy. The NMD data also include a separate layer on soil moisture, available for download both as complete map over Sweden and divided into 9 tiles. The same soil moisture data is also [available via the Swedish Environmental Protection Agency (Naturvårdsverket)](https://metadatakatalogen.naturvardsverket.se/metadatakatalogen/GetMetaDataById?id=cae71f45-b463-447f-804f-2847869b19b0) (this said in order to avoid processing duplicates of the same layer). For the project of mapping Swedish wetlands you need to download 4 layers from NMD:

- Base layer (non generalised) [NMD2018_basskikt_ogeneraliserad_Sverige_v1_1.zip]
- Add on land use layer [NMD_Tillaggsskikt_Markanvandning.zip]
- Mountainous forest [NMD_Lag_fjallskog_v1_1.zip]
- Soil moisture [Markfuktighetsindex_NMD_Sverige.zip]

The complete URL to these 4 layers are given in the text file <span class='file'>Metria-Svenska_markdata.txt</span>:

```
https://gpt.vic-metria.nu/data/land/NMD/NMD2018_basskikt_ogeneraliserad_Sverige_v1_1.zip
http://gpt.vic-metria.nu/data/land/NMD/NMD_Tillaggsskikt_Markanvandning.zip
https://gpt.vic-metria.nu/data/land/NMD/NMD_Lag_fjallskog_v1_1.zip
https://gpt.vic-metria.nu/data/land/NMD/Markfuktighetsindex_NMD_Sverige.zip
```

The data can be downloaded using the text file above and Kattur's GeoImagine Framework process [<span class='process'>DownloadAncillary</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/). The example below points towards the text file <span class='file'>Metria-Svenska_markdata.txt</span> and downloads the data to a subfolder under the location of the text file. Once downloaded the retrieved data can be organized into Karttur's GeoImagine Framework with the process [<span class='process'>OrganizeProjSysData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/).

#### Download NMD

Framework process: [DownloadAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/)

Json command file: [0115-download-Metria-markdata](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0115-download-Metria-markdata/)

```
## Download Metria land cover (NMD) ##
0115-download-Metria-markdata.json
```

As only four (4) files are downloaded it is rather easy to unzip them manually. If you want to automate, please just use the unzipping of the ZMI data (next section) as a template.

The zip archive _NMD_Tillaggsskikt_Markanvandning.zip_ contains three supplementary layers:

- bete (grazing land) [NMD_markanv_bete_v1],
- kraftledning (power lines) [NMD_markanv_kraftledning_v1], and
- anlagda_omr (constructed surfaces) [NMD_markanv_anlagda_omr_v1.]

All three are included in the subsequent processing.

#### Organise NMD

The NMD core data from Metria is delivered as GeoTiff files, but the soil moisture (markfuktighet) is delivered in a proprietary (Erdas imagine) format - with a header file (<span class='file'>.img</span>) and a datafile (<span class='file'>.ige</span>). The organisation of the downloaded NMD data is thus divided into two separate json command files, but using the same framework process ([<span class='process'>OrganizeProjSysData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)).

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file for NMD tif (landcover) files: [0160-organize-Metria-NMD.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160-organize-Metria-NMD/)

```
## Organize Metria NMD ##
0160-organize-Metria-NMD.json
```

Json command file for NMD img/ige (soil moisture) file: [0160-organize-Metria-markfuktighetsindex.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160-organize-Metria-markfuktighetsindex/)

```
## Organize Metria soil moisture (markfuktighetsindex) ##
0160-organize-Metria-markfuktighetsindex.json
```

Once organised, the downloaded NMD data will be copied as regional layers as defined in the json command file, resulting in the following Framework compliant datasets.

| **Dataset** | **Framework path** |
| NMD basskikt | /sweref/metria/region/landcover/sweref/2018/nmd-base_nmd_sweref_2018_v1-1.tif |
| NMD Lag fjallskog | /sweref/metria/region/landcover/sweref/2018/nmd-fjaellskog_nmd_sweref_2018_v1.tif |
| NMD bete | /sweref/metria/region/landcover/sweref/2018/nmd-bete_nmd_sweref_2018_v1.tif |
| NMD kraftledning | /sweref/metria/region/landcover/sweref/2018/nmd-kraftled_nmd_sweref_2018_v1.tif |
| NMD anlagdaomr | /sweref/metria/region/landcover/sweref/2018/nmd-anlagda-ytor_nmd_sweref_2018_v1.tif |
| NMD Markfuktighetsindex | /sweref/metria/region/soilmoisture/sweref/2018/soilmoisture-nmd_markfuktighetsindex_sweref_2018_v1-10m.tif |

#### Tile NMD layers

The organised (imported and registered) files are very large, and for subsequent processing they need to be tiled. The tiling of regional data for customised projection systems is done using the process ([<span class='process'>TileProjSysRegion</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)).

Framework process: [TileProjSysRegion](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-TileProjSysRegion/)

Json command file for NMD landcover:
[0180_TileProjSysRegion_nmd-landcover](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0180_TileProjSysRegion_nmd-landcover/)

Json command file for NMD soilmoisture: [0180_TileProjSysRegion_nmd-soilmoisture](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0180_TileProjSysRegion_nmd-soilmoisture/)

```
## tile Metria NMD landcover to sweref ##
0180_TileProjSysRegion_nmd-landcover.json

## tile Metria NMD soilmoisture to sweref ##
0180_TileProjSysRegion_nmd-soilmoisture.json
```

The tiled layers are organised similar to the regional data, but in a sub-folder called _tiles_ and with each tile as a separate "region". Each tiled dataset listed below consists of the tiles illustrated in the post [Default region Sweden](../wetland-se_defregion/).

| **Dataset** | **Framework path** |
| NMD basskikt | /sweref/metria/tiles/landcover/xXXyYY/2018/nmd-base_nmd_xXXyYY_2018_v1-1.tif |
| NMD Lag fjallskog | /sweref/metria/tiles/landcover/sweref/2018/nmd-fjaellskog_nmd_xXXyYY_2018_v1.tif |
| NMD bete | /sweref/metria/region/landcover/tiles/2018/nmd-bete_nmd_sweref_2018_v1.tif |
| NMD kraftledning | /sweref/metria/region/landcover/sweref/2018/nmd-kraftled_nmd_xXXyYY_2018_v1.tif |
| NMD anlagdaomr | /sweref/metria/region/landcover/tiles/2018/nmd-anlagda-ytor_nmd_xXXyYY_2018_v1.tif |
| NMD Markfuktighetsindex | /sweref/metria/tiles/soilmoisture/sweref/2018/soilmoisture-nmd_markfuktighetsindex_xXXyYY_2018_v1-10m.tif |

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

Each county dataset containing two shape files, one with polygons and one with points.

As the wetland data is delivered as <span class='file'>zip</span> files, there is an extra process step required in between downloading and organising. To get the wetland data organised in Karttur's GeoImagine Framework thus requires three processes:

- [DownloadAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/),
- [UnZipRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/), and
- [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/).

#### Download VMI

Framework process: [DownloadAncillary](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-DownloadAncillary/)

Json command file: [0115-download-Metria-VMI.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0115-download-Metria-VMI)

```
### Download Metria Svenska Våtmarksinventeringen (VMI) ###
0115-download-Metria-VMI.json
```

#### UnZip Raw Data

With lots of zip files to explode you can use the Framework process [<span class='process'>UnZipRawData</span>](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/) for unzipping.

Framework process: [UnZipRawData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-UnZipRawData/)

Json command file: [0120_UnZipRawData_Metria-VMI.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewtland/projects-0120_UnZipRawData_Metria-VMI/)

```
### Unzip Metria VMI datasets ###
0120_UnZipRawData_Metria-VMI.json
```

As stated above, each regional (county) dataset contains two vector files, one with polygons and one with points. As the point map contains additional attribute data you need to handle both. As an example, the Wetland Inventory for Stockholm (AB_Stockholm_VMI.zip) contains the following two shape layers:

- AB_Stockholm_VMI_Punkter
- AB_Stockholm_VMI_Ytor

All other county datasets are structured in the same way.

#### Organise VMI

The VMI dataset is composed of 21 regional county maps (See table above). Rather than organising (importing and registering) 21 individual layers, all 21 county regions can be imported in a single command - by setting a variable file (_replace_) identification structure. Please see the [Framework process](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/) and the [Json command file](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160-organize-Metria-VMI_replace/) for details on how this is accomplished.

Framework process: [OrganizeProjSysData](https://karttur.github.io/geoimagine03-docs-procpack/subprocess/subprocid-OrganizeProjSysData/)

Json command file: [0160-organize-Metria-VMI_replace.json](https://karttur.github.io/geoimagine03-proj-wetland-se-json/sewetland/projects-0160-organize-Metria-VMI_replace/)

```
## Organize Metria VMI ##
0160-organize-Metria-VMI_replace.json
```

The organized VMI data will be copied as regional layers as defined in the json command file, resulting in the following Framework compliant datasets.

| **Dataset** | **Framework path** |
| VMI_ytor | /sweref/metria/region/wetland/sweref/0 |
| AB_Stockholm_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-AB-pt_vmi_sweref_0_v0pt |
| AB_Stockholm_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-AB-poly_vmi_sweref_0_v0poly |
| AC_Vasterbotten_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-AC-pt_vmi_sweref_0_v0pt |
| AC_Vasterbotten_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-AC-poly_vmi_sweref_0_v0poly |
| BD_Norrbotten_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-BD-pt_vmi_sweref_0_v0pt |
| BD_Norrbotten_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-BD-poly_vmi_sweref_0_v0poly |
| C_Uppsala_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-C-pt_vmi_sweref_0_v0pt |
| C_Uppsala_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-C-poly_vmi_sweref_0_v0poly |
| D_Södermanland_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-D-pt_vmi_sweref_0_v0pt |
| D_Södermanland_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-D-poly_vmi_sweref_0_v0poly |
| E_Östergötland_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-E-pt_vmi_sweref_0_v0pt |
| E_Östergötland_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-E-poly_vmi_sweref_0_v0poly |
| F_Jönköping_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-F-pt_vmi_sweref_0_v0pt |
| F_Jönköping_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-F-poly_vmi_sweref_0_v0poly |
| G_Kronoberg_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-G-pt_vmi_sweref_0_v0pt |
| G_Kronoberg_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-G-poly_vmi_sweref_0_v0poly |
| H_Kalmar_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-H-pt_vmi_sweref_0_v0pt |
| H_Kalmar_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-H-poly_vmi_sweref_0_v0poly |
| I_Gotland_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-I-pt_vmi_sweref_0_v0pt |
| I_Gotland_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-I-poly_vmi_sweref_0_v0poly |
| K_Blekinge_VMI_Punkter | /sweref/metria/region/wetland/sweref/0/VMI-K-pt_vmi_sweref_0_v0pt |
| K_Blekinge_VMI_Ytor | sweref/metria/region/wetland/sweref/0/VMI-K-poly_vmi_sweref_0_v0poly |














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
