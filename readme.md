# Processing and visualising point clouds with the PDAL/Entwine/Potree stack

### A workshop designed for [FOSS4G SotM Oceania 2018](http://fossf4g-oceania.org)

**Dr. Adam Steer**  
http://spatialised.net  
@adamdsteer  
adam@spatialised.net

## Introduction

This repository contains materials for a point cloud processing workshop developed by Dr. Adam Steer.

It uses the [Point Data Abstraction Library](http://pdal.io) (PDAL), [Entwine](http://entwine.io) Python, Numpy, Jupyter notebooks, and the [Potree point cloud visualiser](http://potree.org) to develop some concepts about processing point clouds and visualising results.

![PDAL logo](https://pdal.io/_images/pdal_logo.png)  
![Entwine logo](https://entwine.io/_images/entwine_logo_2-color-small.png)

## Materials

Text relating to the workshop is contained in [workshop](./workshop). Jupyter notebooks used in the workshop are contained in [notebooks](./notebooks). Sample workflows as PDAL pipelines are contained in [resources](./resources).

## Preparation

This workshop requires a PDAL installation with python bindings. Using Conda is convenient, but a native installation should work just fine as well. However PDAL is installed, it needs to be visible to a Jupyter notebook.

Using Conda, create a new virtual environment like:

`conda create -n f4g-pdal-workshop python=3.6 pdal numpy jupyter python-pdal pandas matplotlib -c conda-forge`

This creates a virtual environment with PDAL and numpy installed, using the `conda-forge` channel. Head to a command line interface, switch to the exercises directory in this repository and type:

`source activate f4g-pdal-workshop`

..and then:

`jupyter notebook`

...to get access to the processing notebooks.

You'll still need to run entwine via docker, so:

`docker pull connormanning/entwine`

If you don't want to use Jupyter, no drama - most processes can be done at the command line using dockerised PDAL, which you can get via:

`docker pull pdal/pdal:1.8`

...but stick with us - instructions for dockerised processing are included in the notebook!

To visualise some data products, please ensure you have:
- [QGIS](http://qgis.org) or an alternate viewer for geoJSON and georeferenced raster data
- [CloudCompare](https://www.danielgm.net/cc/) or an alternate LAS/LAZ file viewer

...especially if you're not using Jupyter notebooks to run tasks.

### Sample data

For command line processing and demonstrating PDAL in Python we'll use:
https://s3.amazonaws.com/f4g-pdal-workshop-sampledata/T_316000_235500.laz

This data sample comes from the New York University LiDAR survey of Dublin, Ireland: https://geo.nyu.edu/catalog/nyu-2451-38684

For PDAL pipelines and Entwine we'll use:
https://s3.amazonaws.com/f4g-pdal-workshop-sampledata/APPF-farm-sample.laz

This data sample is a photogrammetric point cloud collected by the Australian Plant Phenomics Facility (http://appf.edu.au).

Please download these ahead of the workshop.

## Usage

Feel free to explore and use the workshop materials as you see fit, acknowledgment is appreciated. All data samples used in this workshop are licensed CCBY4 and available in public repositories.

I'd also be very happy to run this workshop (or similar ones) for your organisation - please contact me for details.
