---
layout: post
title: Svenskt Vattenarkiv (SVAR)
categories: wetland-se
excerpt: "Access and download data from the Swedish Meterological and Hydrological Institute (SMHI)"
tags:
  - sweden
  - SMHI
  - SVAR
  - daownload
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-01-19'
modified: '2022-01-19'
comments: true
share: true
figure1: machinelarning_histo_housing
figure3A: machinelarning_linregnaive
figure3B: machinelarning_linregmodel
---

## Introduction

The [Swedish Meterological and Hydrological Institute (SMHI)](https://www.smhi.se) keeps an Open Source database, [Svenskt vattenarkiv, SVAR](https://www.smhi.se/data/hydrologi/svenskt-vattenarkiv). The dataabse includes a [search page](https://www.smhi.se/data/utforskaren-oppna-data/).

## Search and not URLs

From the [Search Open Data (Sök på öppna data) page](https://www.smhi.se/data/utforskaren-oppna-data/) you can either download the layers you want directly, or copy the URLs and paste them in a text file. Each registered layer is well documented, including links to different download formats. The list below (textfile: <span class='file'>SMHI-SVAR-Svenskt_vattenarkiv.txt</span>) links to the URLs of the shape file data for each of the listed layers.

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

The data can be downloaded using the text file above and Kattur's GeoImagine Framework process <span class='process'>DownloadAncillary</span>. The example below points towards the text file <span class='file'>SMHI-SVAR-Svenskt_vattenarkiv.txt</span> and downloads the data to a subfolder under the location of the text file.

```
{
  "userproject": {
    "userid": "karttur",
    "projectid": "karttur",
    "tractid": "karttur",
    "siteid": "*",
    "plotid": "*",
    "system": "ancillary"
  },
  "period": {
    "timestep": "static"
  },
  "process": [
    {
      "processid": "DownloadAncillary",
      "overwrite": false,
      "parameters": {
        "downloadcode": "filelist",
        "path": "DOWNLOADS/SMHI/SVAR/SMHI-SVAR-Svenskt_vattenarkiv.txt",
        "datadir": "DOWNLOADS/SMHI/SVAR/data"
      },
      "srcpath": {
        "volume": "GeoImg2021"
      },
      "dstpath": {
        "volume": "GeoImg2021"
      }
    }
  ]
}
```
