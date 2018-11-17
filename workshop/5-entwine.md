# Entwine - massive point cloud organisation

Entwine is an application which reorganises data into [https://en.wikipedia.org/wiki/Octree](octree) structures, using the Entwine Point Tile (EPT) is a format for writing out the generated octree structures and metadata to disk. The emerging specification is shown here:

https://github.com/connormanning/entwine/blob/master/doc/entwine-point-tile.md


## EPT or Potree?

Potree is an extremely popular format for creating point cloud octrees, based on the Potree Viewer being almost ubiquitous as the open source webGL based massive point cloud renderer. As of writing this workshop, Potree writes out only LAS 1.2 format 2 - or custom binary files. It is fast for visualisation, but not lossless. In order to **do** anything with the underlying data, you need to keep the original files **and** a potree index in hot storage.

Using EPT, the fundamental idea is that the octree becomes the 'hot' data, and the original LAS/LAZ tiles or flight swaths can be cold stored (or tape archived) - since both visualisation and data querying/analysis/product derivation can be handled from EPT.

Both formats create many very small files - which is OK for object storage, less good for moving data around.

## Creating an EPT index

This is actually really easy using dockerised entwine. Taking our RPAS farm sample, try:

`docker run -it -v $(pwd):/opt/data connormanning/entwine -i APPF-classified.laz -o APPF-classified`

While that's running - entwine builds can also be configured with a simple JSON file, for example:

```
{
    "threads": 6,
    "reprojection": { "out": "EPSG:3857" }
}
```

...saved as `web-mercator.json` and run as:

`docker run -it -v $(pwd):/opt/data connormanning/entwine web-mercator.json -i APPF-classified.laz -o APPF-classified`

...would run entwine using 6 threads, and reproject the index to web mercator (EPSG:3857).



## viewing an Entwine datasource

Once you've created an Entwine index, start a webserver in the directory you're working in:

`python -m ./resources/simplecorsserver.py 9001`

...and navigate to http://potree.entwine.io. Modify the URL to something like:

http://potree.entwine.io/custom.html?r=\"http://localhost:9001/dataset-entwine/\"


## reading an Entwine index with PDAL

A key motivation for using entwine is it's lossless data storage. Let's test that, and PDAL's entwine reader at the same time. So let's set up a pipeline...

```
{
  "pipeline": [
    {
      "type": "readers.ept",
      "filename": "ept://APPF-classified",
    },
    "back-from-entwine.laz"
  ]
}

```

...and a PDAL command line to test the results:

`pdal diff original.laz back-from-entwine.laz`

Did we make it?

Let's try something else - retrieve a single tile from a massive entwine index stored remotely, and check it against
