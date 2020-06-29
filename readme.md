# Processing and visualising point clouds with the PDAL/Entwine/Potree stack

**Dr. Adam Steer**  
https://spatialised.net  
@adamdsteer  
adam@spatialised.net

## Introduction

This repository contains materials for a point cloud processing workshop developed by Dr. Adam Steer.

It uses the [Point Data Abstraction Library](http://pdal.io) (PDAL), [Entwine](http://entwine.io) Python, Numpy, Jupyter notebooks, and the [Potree point cloud visualiser](http://potree.org) to develop some concepts about processing point clouds and visualising results.

![PDAL logo](https://pdal.io/_images/pdal_logo.png)  
![Entwine logo](https://github.com/connormanning/entwine/blob/master/doc/logo/color/entwine_logo_2-color-small.png)

## What this workshop will deliver

This workshop uses common point cloud data analysis tasks to demonstrate 'thinking in PDAL'. Using both lidar and photogrammetric data, users are shown how to apply a range of simple strategies to construct complex workflows. These are demonstrated using a command line interface, then using configuration files to hold common parameters, then using Python for running processes and exploratory visualisation. The workshop also shows how to run tasks using PDAL in a docker container.

At the end of the workshop users should have a greater understanding of how PDAL's pipeline architecture works, and some ideas about how to apply it in their own data analysis tasks.

## Materials

Text relating to the workshop is contained in [workshop](./workshop). Jupyter notebooks used in the workshop are contained in [notebooks](./notebooks). Sample workflows as PDAL pipelines are contained in [resources](./resources).

## Preparation

This workshop requires a PDAL installation with python bindings. Using the minified [Conda package manager](https://docs.conda.io/en/latest/miniconda.html) is convenient and works across Linux, MacOS and Windows. A native installation should work just fine as well. However PDAL is installed, it needs to be at least release 2.1, and be visible to a Jupyter notebook.

Using Conda, create a new virtual environment like:

`conda create -n f4g-pdal-workshop python pdal python-pdal entwine jq numpy pandas matplotlib jupyter scipy seaborn -c conda-forge`

This creates a virtual environment with PDAL, PDAL Python bindings, and Entwine installed, using the `conda-forge` channel. It also installs the `jq` command line utility, for parsing PDAL's metadata responses. Numpy, Pandas, Matplotlib and Jupyter are all Python packages for data manipulation and visualisation.

To use the new environment head to a command line interface, switch to the exercises directory in this repository and type:

`conda activate f4g-pdal-workshop`

..and then:

`jupyter notebook`

...to get access to the processing notebooks.

PDAL uses a modular filter system, and some things we want to use are not included in Conda builds. We can also use dockerised PDAL, which you can get via:

`docker pull pdal/pdal`

Instructions for dockerised processing are included in the notebooks.

To visualise some data products, please ensure you have:
- [QGIS](http://qgis.org) or an alternate viewer for geoJSON and georeferenced raster data
- [CloudCompare](https://www.danielgm.net/cc/) or an alternate LAS/LAZ file viewer

...especially if you're not using Jupyter notebooks to run tasks.

### Sample data

For command line processing and demonstrating PDAL in Python we'll use airborne lidar data:
https://s3.amazonaws.com/f4g-pdal-workshop-sampledata/T_316000_235500.laz

This data sample comes from the New York University LiDAR survey of Dublin, Ireland: https://geo.nyu.edu/catalog/nyu-2451-38684

For PDAL pipelines and Entwine we'll work on drone data, processed into a point cloud using structure-from-motion techniques:
https://s3.amazonaws.com/f4g-pdal-workshop-sampledata/APPF-farm-sample.laz

This data sample was collected by the Australian Plant Phenomics Facility (http://appf.edu.au).

Please download these ahead of the workshop.

## Usage

Feel free to explore and use the workshop materials as you see fit, acknowledgment is appreciated. All data samples used in this workshop are licensed CCBY4 and available in public repositories.

I'd also be very happy to run this workshop and customised variations of it for your organisation - please contact me for details and pricing.
