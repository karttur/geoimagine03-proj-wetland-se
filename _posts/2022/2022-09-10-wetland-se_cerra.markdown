---
layout: post
title: CERRA-Land
categories: wetland-se
excerpt: "Retrieve Copernicus European Regional ReAnalysis Land (CERRA-Land) dataset"
tags:
  - CERRA
  - climate
  - cdsapi
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-10'
modified: '2022-11-25'
comments: true
share: true
---

## Introduction

The [Copernicus European Regional ReAnalysis Land (CERRA-Land)](https://climate.copernicus.eu/copernicus-regional-reanalysis-europe-cerra) dataset provides spatially and temporally consistent historical reconstructions of climate, Earth surface and soil variables. This post describes how to access CERRA-Land data.

## Prerequisite

You must have a python environment with the package <span class='package'>cdsapi</span> installed and you must have a Copernicus user. See the Copernicus page on [How to use the CDS API](https://cds.climate.copernicus.eu/api-how-to).

## CERRA-Land

The [CERRA system](https://climate.copernicus.eu/copernicus-regional-reanalysis-europe-cerra) is a comparatively high resolution (5.5 km) dataset of climate history forced by the global [ERA5 reanalysis](https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5) and refined using both in-situ observations and satellite information. CERRA-Land is an open dataset available for download under the European Copernicus program (Copernicus Climate Change Service). For details see the page [Copernicus regional reanalysis for Europe (CERRA)](https://climate.copernicus.eu/copernicus-regional-reanalysis-europe-cerra).

CERRA-Land contains more than 30 thematic layers of climate, Earth surface conditions and soil variables, with several of the latter at different depths.

### Access CERRA data

CERRA datasets are accessible via [Copernicus Climate Data Store (CDS)](https://cds.climate.copernicus.eu/#!/home). To get directly to the search page  click [HERE](https://cds.climate.copernicus.eu/cdsapp#!/search?type=dataset&text=cerra). The data that we are using for this project is the [CERRA-Land sub-daily regional reanalysis data for Europe from 1984 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-cerra-land?tab=overview). The CERRA-Land datasets were developed to allow addressing hydrological modelling, water resource management issues and to carry out climate change impact studies.

### CERRA-Land sub-daily regional reanalysis data for Europe from 1984 to present

The following table is a summary of the available data for CERRA-Land.

| **Variable** | **Unit** | **Description** |
| Albedo | % | Reflectance of downward solar radiation |
| Evaporation | kg m^-2 | The amount of evaporated water from the Earth surface |
| Snow cover | fraction | The fraction of the surface covered by snow |
| Lake bottom temperature | K | Temperature of water at the bottom of lakes |
| Lake depth | m | Depth of lakes |
| Lake ice depth | m | Thickness of ice layer on lakes |
| Lake ice temperature | K | Skin temperature of  ice on lakes |
| Lake mix-layer depth | m | Thickness of upper water layer in lakes |
| Lake mix-layer temperature | K | Temperature of upper water layer in lakes |
| Lake shape factor | dimensionless |  Temperature changes with depth in lakes |
| Lake total layer temperature | K | Lake total water column mean temperature |
| Land-sea mask | dimensionless | Proportion of land/sea in each cell |
| Liquid volumetric soil moisture | m^3 m^-3 | Liquid water fraction at 14 soil depths |
| Orography | m | Elevation above sea level |
| Percolation | kg m^-2 | Water drained out of upper soil layers |
| Skin temperature | K | Temperature at the Earth's surface |
| Snow albedo | % | Reflectance of downward solar radiation from snow |
| Snow density | kg m^-3 | Snow thickness on the ground |
| Snow depth water equivalent | kg m^-2 | The mass of liquid water of the snow column |
| Snow melt | kg m^-2 | Melting of snow |
| Soil heat flux | W m^-2 | energy received by the soil converted to heat |
| Soil temperature | K | Temperature at 14 soil depths |
| Surface latent heat flux | J m^-2 | energy received by the soil converted to evaporation |
| Surface net solar radiation | J m^-2 |  accumulated solar short-wave radiation that is absorbed at the surface  |
| Surface net thermal radiation  | J m^-2 | Difference between incoming and outgoing thermal radiation at the surface |
| Surface roughness | m | aerodynamic roughness length |
| Surface runoff | kg m^-2 | Excess water at full soil saturation |
| Surface sensible heat flux | J m^-2 | Energy emitted from the surface as heat |
| Surface solar radiation downwards | J m^-2 | Total solar short-wave radiation at the surface |
| Surface thermal radiation downwards | J m^-2 | Total thermal radiation at the surface  |
| Temperature of snow layer | K | Mean temperature of 12 snow layers |
| Total precipitation | kg m^-2 | Total daily precipitation |
| Volumetric soil moisture | m^3 m^-3 | Liquid + frozen water fraction at 14 soil depths |
| Volumetric transpiration stress-onset  | m^3 m^-3 | Soil water content after gravitational drainage at 14 depths |
| Volumetric wilting point | m^3 m^-3 | Plant inaccessible soil water content at 14 depths |

### Download dataset

![Download-tab](../../images/cerra-download-tab.png)
{: .pull-right}
From the [CERRA-Land sub-daily regional reanalysis data for Europe from 1984 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-cerra-land?tab=overview), select the tab <span class='button'>Download data</span>, as illustrated to the right.

#### Select datasets to download

Select the **Variable** you want to retrieve; _Total precipitation_ in the example below.

<figure>
<img src="../../images/cerra-download-variable.png">
<figcaption> Select the varaible(s) to retrieve. </figcaption>
</figure>

The variable _Total precipitation_  only has one **Level type**, _Surface_, that should be selected.

<figure>
<img src="../../images/cerra-download-level-type.png">
<figcaption> For precipitation data the Level typ is Surface. </figcaption>
</figure>

The options for **Soil layer** are all faded out as there is no such option for _Total precipitation_.

<figure>
<img src="../../images/cerra-download-soil-layer.png">
<figcaption> For precipitation data there are no options for Soil layer. </figcaption>
</figure>

As we are retrieving historical data, select _Analysis_ from the **Product type** options.

<figure>
<img src="../../images/cerra-download-product-type.png">
<figcaption> Historical data is set as the option Analysis from the Product Type options.</figcaption>
</figure>

Set the **Year**, **Month** and **Day** to retrieve.

<figure>
<img src="../../images/cerra-download-year.png">
<img src="../../images/cerra-download-month.png">
<img src="../../images/cerra-download-day.png">
<figcaption> Set the Year, Month and Day to retrieve.</figcaption>
</figure>

You also have to set **Time**, which for _Total precipitation_ is _06:00_;  while **Leadtime hour** is not applicable and thus have no options.

<figure>
<img src="../../images/cerra-download-time.png">
<img src="../../images/cerra-download-leadtime-hour.png">
<figcaption> For daily Total Precipitation time is defaulted at 06:00 and there are no options for leadtime hour.</figcaption>
</figure>

Available **Format** options for downloading are GRIB2 and NetCDF (experimental). In the example below GRIB2 is selected.

<figure>
<img src="../../images/cerra-download-format.png">
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
    'reanalysis-cerra-land',
    {
        'format': 'grib',
        'variable': 'total_precipitation',
        'level_type': 'surface',
        'product_type': 'analysis',
        'year': '2020',
        'month': '01',
        'day': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
            '13', '14', '15',
            '16', '17', '18',
            '19', '20', '21',
            '22', '23', '24',
            '25', '26', '27',
            '28', '29', '30',
            '31',
        ],
        'time': '06:00',
    },
    'download.grib')
```

If you are using Karttur's GeoImagine Framework, with the package <span class='package'>cdsapi</span> installed, you can run the script above from within the Framework. The instructions for how to setup a non-Framework embedded retrieval using cdsapi, see the Copernicus page on [How to use the CDS API](https://cds.climate.copernicus.eu/api-how-to).
