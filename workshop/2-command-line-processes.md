# Command line processing

## Querying data

One of the most fundamental tasks we can do is find out about our data - what's in it, where it is, and whether there are any problems with it.

We'll dive straight in and use PDAL to run a query on a .LAS file we've obtained.

`pdal info sample-data/lasfile.laz`

...gives you a huge JSON spew of a bunch of file attributes, including summary statistics for each dimension in the file.

Let's look at some different metadata queries. What can you see from:

`pdal info sample-data/lasfile.laz --stats`  
`pdal info sample-data/lasfile.laz --metadata`  
`pdal info sample-data/lasfile.laz --summary`  
`pdal info sample-data/lasfile.laz --boundary`  

## Searching for points

We can look for information about points. Try:

`pdal info sample-data/lasfile.laz -p 0`

...it should tell you all about the first point (0-indexed) in the file.

More useful is finding points by geographic location. the `query` option to `info` can help:

`pdal info sample-data/lasfile.laz --query "637301.20, 851217.57, 496.49/3"`

...this will return the three nearest points to the coordinates provided (`/3`). You might use this if you want to find a set of points near ground control, and assess how close they are.

Another super useful convenience application is `density`, although it is aimed at 2.5D data (eg airborne LiDAR). This returns a hex-binned representation of point density in your data, which is easily visualised by QGIS. It's a great and quick QA/QC assessment tool:

`pdal density -i infile.las -o lasdensity.sqlite -f sqlite --sample_size 2`

What does it tell you? For LIDAR - it shows where you have overlapping flight swaths. It also lets you easily see whether your data collection is relatively uniform.

## Simple transformations

PDAL can do straightforward data translation as a one liner using the [translate](https://pdal.io/apps/translate.html) convenience application . We'll inspect some use cases here. The basic pattern

`pdal translate infile outfile --operation(s)`

To compress a file to LAZ format, try:

`pdal translate -i infile.las -o outfile.laz`

...which can be more fully expressed as:

`pdal translate -i infile.las -o outfile.laz --writers.las.compression=true`

Reprojecting data is also a useful tool. This moves data from [UTM zone 55](http://spatialreference.org/ref/epsg/wgs-84-utm-zone-55s/) to [MGA zone 55 (GDA94)](http://spatialreference.org:8092/ref/epsg/gda94-mga-zone-55/):

`pdal translate -i .laz -o outfile.laz --filters.reprojection.in_srs=EPSG:32755 --filters.reprojection.out_srs=EPSG:28355`

## Filtering points

Points can be filtered many ways. We can restrict ranges point dimensions. If points carry labels (for example ASPRS classifications) we can use PDAL to exclude classes. We can also try to label 'noise' or points we don't think we need to pay attention to. Here, we'll limit 

`pdal translate -i `

## Summary

PDAL can run as a command line application - using options to specify and modify stages and point views. This is really powerful, but can get unwieldy for long operations.


[next - pipelines](3-pipelines.md)
