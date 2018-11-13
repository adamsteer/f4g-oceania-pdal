# Entwine - massive point cloud organisation

Entwine is an application which reorganises data into octree structures. It writes out data as ASPRS LAZ, or binary files with customised schemas.

The Entwine Point Tile (EPT) is a format for writing out octree structures to disk, which can be served statically. The emerging specification is shown here:

https://github.com/connormanning/entwine/blob/master/doc/entwine-point-tile.md

## EPT or Potree

Potree is a popular format for creating point cloud octrees

## creating an entwine index

This is actually really easy using docker:

`docker run -it -v $(pwd):/opt/data connormanning/entwine -i dataset.laz -o dataset-entwine`

## viewing an Entwine datasource

Once you've created an Entwine index, start a webserver in the directory you're working in:

`python -m http.server 9001`

...and navigate to http://potree.entwine.io. Modify the URL to something like:

http://potree.entwine.io/custom.html?r=\"http://localhost:9001/dataset-entwine/\"
