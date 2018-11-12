# Starting out with PDAL

In this workshop we will learn by doing - it'll be a bit of a magical mystery tour, in that many concepts will be shown, then discussed.



## Querying data

One of the most fundamental tasks we can do is find out about our data - what's in it, where it is, and whether there are any problems with it.

We'll dive straight in and use PDAL to run a query on a .LAS file we've obtained.

`pdal info sample-data/lasfile.laz`

...gives you a huge JSON spew of a bunch of file attributes, including summary statistics for each dimension in the file.


## Dimensions

In PDAL, *dimensions* are the set of things described in the point cloud data schema. For LAS format files, dimensions are set within ASPRS LAS standard bounds. Some common dimensions are:

|Name | Description |
|-----|-------------|
|X | |
|Y | |
|Z | |
|Intensity | |
|Red | |
|Green | |
|Blue | |
