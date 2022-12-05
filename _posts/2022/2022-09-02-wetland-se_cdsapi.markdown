---
layout: post
title: CDSAPI
categories: wetland-se
excerpt: "Climate Data Service Application Program Interface (CDSAPI)"
tags:
  - copernicus
  - climate data
  - access
  - cdsapi
  - setup
image: ts-mdsl-rntwi_RNTWI_id_2001-2016_AS
date: '2022-09-02'
modified: '2022-11-25'
comments: true
share: true
---

## Introduction

The [Copernicus Climate Change Service (C3S)](https://cds.climate.copernicus.eu/about-c3s) and its [Climate Data Store (CDS)](https://cds.climate.copernicus.eu/cdsapp#!/home) is summarised in the [previous post](../wetland-se_c3s/). This post is a brief introduction to the [CDS Application Program Interface (API), or CDSAPI](https://cds.climate.copernicus.eu/api-how-to), the recommended path to retrieve data from the CDS.

## CDSAPI

CDSAPI is a Python programming package and requires that you have Python installed. If you have no experience with Python, Copernicus provides a page with details on [How to use the CDS API](https://cds.climate.copernicus.eu/api-how-to).

### Install CDSAPI in conda virtual environment

To download the CDS data using Karttur's GeoImagine Framework, you need to install the Python package [<span class='package'>cdsapi</span>](https://github.com/ecmwf/cdsapi) with the <span class='app'>Anaconda</span> virtual environment you use for the Framework. <span class='terminalapp'>activate</span> the Framework virtual environment. You can then either use [<span class='terminalapp'>conda install</span>](https://anaconda.org/conda-forge/cdsapi) or [<span class='terminalapp'>pip install</span>](https://pypi.org/project/cdsapi/) to install the package:

<span class='terminal'>(your-virtual-python-environ) % conda install -c conda-forge cdsapi</span>

<span class='terminal'>(your-virtual-python-environ) % pip install cdsapi</span>

### Create Python Package

To save the download commands for future reference I use Karttur's GeoImagine Framework <span class='app'>Eclipse</span> setup for creating special packages for accessing the ERA5 and CERRA products. Also to allow me to save the different downloads for future reference, but that is not necessary. Once you have created an order for CDS data you can simply run the python script you receive from anywhere - as long as you have activated the correct virtual Python environment.
