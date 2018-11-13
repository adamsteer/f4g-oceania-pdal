# Starting out with PDAL command lines

Like GDAL, PDAL is a c++ library, but also a set of command line utilities. In this section we'll explore some common PDAL one-liners for inspection and transforming data.

## Querying data

One of the most fundamental tasks we can do is find out about our data - what's in it, where it is, and whether there are any problems with it.

We'll dive straight in and use PDAL to run a query on a .LAS file we've obtained.

`pdal info sample-data/lasfile.laz`

...gives you a huge JSON spew of a bunch of file attributes, including summary statistics for each dimension in the file.

## A quick segue about Dimensions

In PDAL, *dimensions* are the set of things described in the point cloud data schema. For LAS format files, dimensions are set within ASPRS LAS standard bounds. Some common dimensions are:

|Name | Description |
|-----|-------------|
|X | 'X' coordinate (longitude in EPSG:3246)|
|Y | 'Y' coordinate (latitude in EPSG:4326)|
|Z | 'up' coordinate |
|Intensity | Laser return intensity |
|Red | Red channel in RGB colour |
|Green | Green channel in RGB colour |
|Blue | Blue channel in RGB colour |

The list of standard PDAL dimensions is here: https://pdal.io/dimensions.html - again, many of these are derived from ASPRS LAS formats. Primarily because it's a data exchange format we can't ignore!

## Moar metadata!

Let's look at some different metadata queries. What can you see from:

`pdal info sample-data/lasfile.laz --stats`
`pdal info sample-data/lasfile.laz --metadata`
`pdal info sample-data/lasfile.laz --summary`
`pdal info sample-data/lasfile.laz --boundary`



## Simple transformations

PDAL can do straightforward data translation as a one liner. We'll inspect some use cases jere. The basic operation is  

`pdal translate inffle outfile --operation(s)`
