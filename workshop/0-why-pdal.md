# PDAL and all the other things

Setting out to process point clouds, why would I choose PDAL? What is different from LAStools, or PCL, or libLAS, or LASpy?

My story was ridiculously simple: I needed to process some LAS 1.4 files, had no budget to buy LAStools, and PDAL was there.

PDAL's pipeline functionality was the reason I stayed, along with the emergence of Entwine, a point cloud management approach that is closely integrated with PDAL and provides lossless, visualisation-and-data-query-ready data organisation.

And at the end of the day it's what I know most about how to drive.

## All the other things

### LAStools

LAStools and still is a breakthrough in point cloud processing. It is focussed on speed and convenience, with a model of giving away a bunch of functionality and making users pay for advanced tooling. For a mission critical application, though, it's pretty cheap. It's author, Martin Isenburg, contributes a lot to the LiDAR community and is worth supporting.

### PCL

The Point Cloud Library (PCL) carries incredible functionality, and if you understand it well you should use it. It's less convenient in terms of processing pipelines, and PDAL actually carries some convenience wrappers for PCL functionality.

### LibLAS

libLAS is a free library that can do a lot of basic data interrogation tasking. It is essentially mothballed, but still handy to have in the command line toolkit ( and easy to install on Ubuntu )

### LASpy

Python libraries for reading LAS files into numpy arrays and doing some simple operations.

### PyLIDAR

PyLIDAR wraps around SPDlib, a method for packaging full waveform LiDAR into HDF containers.

### DASOS
...

## FUSION
...

### GRASS-GIS


### lidR

It's R.

### All the other things

Discuss! What's your favourite LiDAR processing tool?

I liked Bentley's ecosystem a lot (using Terrasolid, Descartes, Point ... ), but once I left a workplace with a site license, the entry fee is high!

[next - starting out with PDAL](1-querying-data.md)
