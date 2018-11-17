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

`pdal info sample-data/lasfile.laz --query "637301.20, 851217.57, 496.49/3"``

...this will return the three nearest points to the coordinates provided (`/3`). You might use this if you want to find a set of points near ground control, and assess how close they are.

## Simple transformations

PDAL can do straightforward data translation as a one liner using the [translate](https://pdal.io/apps/translate.html) convenience application . We'll inspect some use cases here. The basic pattern

`pdal translate infile outfile --operation(s)`

To compress a file to LAZ format, try:

`pdal translate -i infile.las -o outfile.laz`

...which can be more fully expressed as:

`pdal translate -i infile.las -o outfile.laz --writers.las.compression=true`

Another super useful convenience application is `density`, although it is aimed at 2.5D data (eg airborne LiDAR). This returns a hexbinned representation of point density in your data, which is easily visualised by QGIS. It's a great and quick QA/QC assessment tool:

`pdal density -i infile.las -o lasdensity.sqlite -f sqlite --smooth --sample-size 5`

## Translating files




## Filtering points




[next - pipelines](3-pipelines.md)
