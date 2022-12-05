---
layout: post
title: ERA5-Land
categories: wetland-se
excerpt: "ERA5-Land monthly averaged data from 1950 to present"
tags:
  - wetland
  - dem
  - Sweden
  - organize
  - tiling
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-03'
modified: '2022-11-25'
comments: true
share: true
---

## Introduction

[ERA5](https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5) is a global product and widely used for analysing past climate conditions and trends from the 1950s to the present. There are many different datasets and variables produced under the umbrella of ERA5, this post only deals with [ERA5-Land](https://www.ecmwf.int/en/era5-land) monthly averaged data from 1950 to present. The monthly data is a compilation of ERA5-Land hourly data.

## ERA5-Land

ERA5 is produced with hourly estimates of a large number of atmospheric, land and oceanic climate variables. A complete list of the main variables (of which some are available at vertical sections) is available [HERE](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview). As mentioned above, this post focuses on the ERA5-Land compiled to monthly averaged data from 1950 to present.

The native spatial resolution of the ERA5-Land reanalysis dataset is 9km on a reduced Gaussian grid (TCo1279). The ERA5-land data available via CDS has been regridded to a regular lat-lon grid of 0.1x0.1 degrees. While the core ERA5 data is available from 1959 and is updated within 3 months or real time, the ERA5-Land is extended back to 1950, but with the first years considered preliminary. For details on ERA5, see the page on [ERA5: data documentation](https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation).

**NOTE** that ERA5 data is very popular and the queueing time might be exorbitant. Try to download over weekends instead. And chose datasets that cling together as outlined in the troubleshooting page [My request is queued for a long time - Web API FAQ](https://confluence.ecmwf.int/display/UDOC/My+request+is+queued+for+a+long+time+-+Web+API+FAQ) and elsewhere.

### Access ERA5

If you want to jump right in and check out the [ERA5 datasets at CDS](https://cds.climate.copernicus.eu/#!/search?text=ERA5&type=dataset).

### ERA5-Land ERA5-Land monthly averaged data

The [overview tab at the ERA5-Land monthly averaged data from 1950 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-land-monthly-means?tab=overview) page contains a complete list of the available data variables.

### Download dataset

![Download-tab](../../images/cerra-download-tab.png)
{: .pull-right}
From the overview page [ERA5-Land monthly averaged data from 1950 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-land-monthly-means?tab=overview), select the tab <span class='button'>Download data</span>, as illustrated to the right.

**Note** that it is easier to download a single variable in each "order". If you download multiple variables you need to keep track of which variables you did choose in order to disentangle the file you will receive.

#### Select datasets to download

Select the **Product type** to download.

<figure>
<img src="../../images/era5-download-product-type.png">
<figcaption> Select the Product type to retrieve. </figcaption>
</figure>

Select the **Variable** you want to retrieve; _Total precipitation_ in the example below. As mentioned above there are some advantages with selecting a single variable with each order.

<figure>
<img src="../../images/era5-download-variable.png">
<figcaption> Select the variable(s) to retrieve. </figcaption>
</figure>

Set the **Year** and **Month** to retrieve. Here it is better to select the full time span and full temporal resolution with each order.

<figure>
<img src="../../images/era5-download-year.png">
<img src="../../images/era5-download-month.png">
<figcaption> Set the Year and Month to retrieve.</figcaption>
</figure>

You also have to set **Time**, which for monthly averade data is defaulted to _00:00_.

<figure>
<img src="../../images/era5-download-time.png">
<figcaption> For monthly datasets time is defaulted at 00:00.</figcaption>
</figure>

You can set the **Geographical area** or accept the default global region.

<figure>
<img src="../../images/era5-download-geographical-area.png">
<figcaption> Select Geographical area or accept Global default.</figcaption>
</figure>

Available **Format** options for downloading are GRIB, NetCDF (experimental) and NetCDF-3 (experimental, not recommended). In the example below GRIB is selected.

<figure>
<img src="../../images/era5-download-format.png">
<figcaption> Options for downloaded file Format.</figcaption>
</figure>

Accept the **Terms of use** if you agree to them.

<figure>
<img src="../../images/cerra-download-terms-of-use.png">
<figcaption> To retrieve to the data you must accept the Terms of use.</figcaption>
</figure>

#### API

With all the required fields selected, the button <span class='button'>Show API request</span> should turn green. Click on it to see the API python code:

```
import cdsapi

c = cdsapi.Client()

c.retrieve(
    'reanalysis-era5-land-monthly-means',
    {
        'product_type': 'monthly_averaged_reanalysis',
        'variable': 'total_precipitation',
        'year': [
            '2010', '2011',
        ],
        'month': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
        ],
        'time': '00:00',
        'format': 'grib',
    },
    'download.grib')
```

If you are using Karttur's GeoImagine Framework, with the package <span class='package'>cdsapi</span> installed, you can run the script above from within the Framework. The instructions for how to setup a non-Framework embedded retrieval using cdsapi, see the Copernicus page on [How to use the CDS API](https://cds.climate.copernicus.eu/api-how-to).

Some of the ERA5 data is preprocessed and available online, and some need to be processed prior to downloading. In case you order data that needs to be processed you can check the status at [https://cds.climate.copernicus.eu/live/queue](https://cds.climate.copernicus.eu/live/queue).
