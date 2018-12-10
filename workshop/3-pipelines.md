# The PDAL pipeline

PDAL's pipeline processing approach is a powerful tool for enabling complex, automated and repeatable workflows. We've already seen it in action exploring command line processing, and the concepts of readers, writers and filters.

What we've typed on the terminal is really invoking a PDAL pipeline in the background - which users a `reader` to ingest data to a `pointview` which can be operated on by `filters`, then writes out using a `writer`.

We can also cast these workflows into JSON configuration snippets, and run them using the `pipeline` application. Using reprojection as an example, we can create the JSON snippet `reprojection.json` as follows:

```
{
  "pipeline":[
    {
        "type":"readers.las",
        "filename":"APPF-farm-sample.laz"
    },
    {
        "type":"filters.reprojection",
        "in_srs":"EPSG:32756",
        "out_srs":"EPSG:28356"
    },
    {
        "type":"writers.las",
        "compression": "laszip",
        "filename":"APPF-farm-sample-reprojected.laz"
    }
  ]
}
```
...and run:

`pdal pipeline reprojection.json`

Just to remind ourselves, the command line equivalent is:

```
pdal translate inputfile.las outputfile.laz --filters.reprojection.in_srs="EPSG:32756" --filters.reprojection.out_srs="EPSG:28356"
```

However - instead of using increasingly long command line processes, the `pipeline` application allows creation of a library of standard processing tasks as easily-readable JSON files.

## Diving right in - labelling ground points

Classifying ground points is a fundamental task for point cloud processing. LiDAR data can often exploit 'last returns' for ground classification - and usually any LiDAR you come across in public repositories already has ground points labelled (it's often a requirement in acquisition contracts).

However, sometimes the classification is not amazing - it's hard, especially if the surveyed area contains a mixture of terrain types and objects. Further, many photogrammetric point clouds won't have ground labels attached to points.

We'll demonstrate ground labelling for RPAS data using the sample `APPF-farm-sample.laz`. Here's what the data look like:

![Farm sample](../images/appf-sample.jpg)

...and coloured by classification:

![Farm sample](../images/appf-sample-classes.jpg)

It has no classification labels! Let's try to fix that. Create a file 'rpas-ground.json' and populate it with:
```
{
  "pipeline":[
    {
      "type":"readers.las",
      "filename":"APPF-farm-sample.laz"
    },
    {
      "type":"filters.assign",
      "assignment":"Classification[:]=0"
    },
    {
      "type":"filters.elm",
      "cell":20.0,
      "class": 7,
      "threshold": 0.5
    },
    {
      "type":"filters.outlier"
    },
    {
      "type":"filters.smrf",
      "ignore":"Classification[7:7]",
      "slope":0.2,
      "window":20,
      "threshold":0.1
    },
    {
      "type":"filters.range",
      "limits":"Classification[2:2]"
    },
    {
      "type":"writers.las",
      "filename":"APPF-ground-20.laz"
    }
  ]
}
```

...save it, then run:

`pdal pipeline rpas-ground.json`

...but wait! there's a whole lot of new there. We've stacked a whole lot of filters together. Here we see the convenience of the pipeline approach. Starting from the top, we are:

- using `filters.assign` to label all points as unclassified
- applying `filters.elm` (extended local minimum) to label 'low points' as noise (ASPRS LAS class 7)
- then applying `filters.outlier`, using a statisical approach label remaining outlying points as noise (ASPRS LAS class 7)
- next, using `filters.smrf` (Simple Morphological Filter) to label points as 'ground', ignoring any points already labelled as 'noise'
- ...then finally, removing any points *not* labelled as ground from the output and writing them out to `APPF-ground-20.laz`

Once you've got an output file, if you have CloudCompare (or another LAS/LAZ viewer), open `APPF-ground-20.laz` and check the results:

![Farm sample](../images/appf-ground-smrf-20.jpg)

You'll see here only points labelled as 'ground' are returned - we've dropped any noise and unclassified points using a range filter. It's also not the best segmentation of 'ground' points - a stand of trees has been mislabelled!

We've also leaped right into the deep end with a long chain of processing. The point here is showing how it's actually pretty easy - once you know what it is you need to do. We've used a `reader`, a bunch of `filters` chained together to operate on a `pointview`, and exported the result using a `writer`

Try pulling apart the pipeline and running parts of it, or removing some of the filters and see what happens by viewing results in CloudCompare. If you haven't already done so, that's the next step

## Overriding options, and making better ground

In the example above, input and output filenames are fixed, and if we want to alter parameters we need to go edit a JSON file and re run everything. That's a little painful, especially if you have a thousand tiles to process!

We can fix that - using either command line overrides, or for clever folks, templating in JSON (we'll get to that shortly using Python).

Let's modify our pipeline a little to remove the final filter, and write out the entire dataset with noise and ground points labelled. We'll also try another built-in ground segmentation method, the Progressive Morphological Filter: `filters.pmf`. Write this out as `rpas-ground-pmf.json`

```
{
  "pipeline":[
    {
        "type":"readers.las",
      "filename":"APPF-farm-sample.laz"
    },
    {
        "type":"filters.assign",
        "assignment":"Classification[:]=0"
    },
    {
        "type":"filters.elm",
        "cell": 10.0,
        "class": 7,
        "threshold": 0.5
    },
    {
        "type":"filters.outlier"
    },
    {
        "type":"filters.pmf",
        "ignore":"Classification[7:7]",
        "initial_distance":0.3,
    },
    {
        "type":"writers.las",
        "filename":"APPF-ground-pmf.laz"
    }
  ]
}
```

...and for argument's sake, write out a different filename:

```
pdal pipeline rpas-ground-pmf.json --writers.las.filename="APPF-ground-allthepoints-pmf.laz"
```

...which looks like:

![RPAS classification](../images/appf-sample-pmf.jpg)

![RPAS classification](../images/appf-sample-pmf-classes.jpg)

We can also modify filter parameters, to tune how points are labelled:

```
pdal pipeline rpas-ground.json --filters.elm.cell=20.0 --filters.pmf.initial_distance=0.4 --writers.las.filename="APPF-ground-allthepoints-pmf-elm20.laz"
```

...which results in:

!(RPAS classification)[../images/rpas-pmf-pass2-classes.jpg]

In short, any option from the stages used in the pipeline can be over-ridden by passing equivalent command line options. H

When designing pipelines, try to optimise them such that options which *need* to change often are as few as possible.

## I want to make a product from my data

Many end uses of point cloud data are not points at all - but rasters or other data products based on the points. We can string together more! In this example we'll create a DTM from the ground points we just created:

```
{
  "pipeline":[
    {
        "type":"readers.las",
        "filename":"APPF-farm-sample.laz"
    },
    {
        "type":"filters.assign",
        "assignment":"Classification[:]=0"
    },
    {
        "type":"filters.elm",
        "cell": 10.0,
        "class": 7,
        "threshold": 0.5
    },
    {
        "type":"filters.outlier"
    },
    {
        "type":"filters.pmf",
        "ignore":"Classification[7:7]",
        "initial_distance":0.3,
        "cell_size": 2
    },
    {
        "type":"filters.range",
        "limits":"Classification[2:2]"
    }
    {
        "type":"writers.gdal",
        "filename":"APPF-ground.tiff",
        "resolution":1
        "output_type":"idw"
    }
  ]
}
```
...you can open the result in QGIS and take a look. Here's a preview:

(PDALDTM in QGIS)[../images/QGIS-dtm.jpg]

Note that PDAL does not handle DTM filling - that's left to tools which do the job better (for example GDAL)

Meshes can be made the same way, try replacing the `writers.gdal` block with:
```
    {
        "type":"filters.poisson"
    },
    {
        "type":"writers.ply",
        "filename":"APPF-ground.ply"
    }
```

...to make a ground mesh - and then removing the range filter to create a surface mesh (this might take a while). Again, here's a sample, viewed in meshlab:

(Meshlab mesh)[../images/Meshlab-mesh.jpg]


## Summary

Pipelines are the bread and butter of PDAL as a command line application. Re-usable code chunks which are easily readable, and reconfigurable on the fly.

[next - python and PDAL](4-python-and-pdal.md)
