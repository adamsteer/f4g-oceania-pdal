# Command line processing




## Simple transformations

PDAL can do straightforward data translation as a one liner using the [translate](https://pdal.io/apps/translate.html) convenience application . We'll inspect some use cases here. The basic pattern

`pdal translate infile outfile --operation(s)`

To compress a file to LAZ format, try:

`pdal translate -i infile.las -o outfile.laz`

...which can be more fully expressed as:

`pdal translate -i infile.las -o outfile.laz --writers.las.compression=true`

Another super useful convenience application is `density`, although it is aimed at 2.5D data (eg airborne LiDAR). This returns a hexbinned representation of point density in your data, which is easily visualised by QGIS. It's a great and quick QA/QC assessment tool:

`pdal density -i infile.las -o lasdensity.sqlite -f sqlite --smooth --sample-size 5`


## Readers and writers




## Filters


[next - pipelines](3-pipelines.md)
