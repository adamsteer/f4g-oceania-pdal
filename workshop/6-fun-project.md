# A fun project

This week the City of Melbourne released a new point cloud dataset! It's available here:

...but don't download it! It's 4 GB of uncompressed LAS tiles, roughly 200 of them.

Instead, check out the entwine resource here: http://melbourne-2018-ept.s3.amazonaws.com/

The data are generated from a photogrammetric model, and are very pretty! However - none of the points are labelled. This mini hack is aimed at fixing that.

## The plan

1. Grab a small area (maybe around the university) from the EPT datasource.

2. Since we've focussed a lot on finding ground points, let's try that first and generate ground labels for points from the dataset

3. We'd also like to label buildings. We'll hack this up using building polygons from OpenStreetMap, some clipping functionality we've explored already, and some point label assignment filters

4. Trees? any ideas about trees? We could assess points which are not ground and not buildings and call them trees.

5. Any other ideas?

6. Make a web-viewable EPT dataset out of our new labelled points

7. anything else? what about selecting and meshing a single building? Or creating a raster DEM for melbourne?

[next - wrapping up](7-wrapup.md)
