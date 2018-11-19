# The PDAL pipeline

PDAL's pipeline processing approach is a powerful tool for enabling complex, automated and repeatable workflows. We've already seen it in action exploring command line processing, and the concepts of readers, writers and filters.

What we've typed on the terminal is really invoking a PDAL pipeline in the background - which users a `reader` to ingest data to a `pointview` which can be operated on by `filters`, then writes out using a `writer`.

We can also cast these workflows into JSON configuration snippets, and run them using the `pipeline` application. Using reprojection as an example, we can create the JSON snippet `reprojection.json` as follows:

```
{
  "pipeline":[
    {
        "type":"readers.las",
        "filename":"inputfile.las"
    },
    {
        "type":"filters.reprojection",
        "in_srs":"EPSG:32756",
        "out_srs":"EPSG:28356"
    },
    {
      "type":"writers.las",
      "compression": "laszip",
      "filename":"outputfile.laz"
    }
  ]
}
```
...and run:

`pdal pipeline reprojection.json`

Just to remind ourselves, the command line equivalent is:

`pdal translate inputfile.las outputfile.laz --filters.reprojection.in_srs="EPSG:32756" --filters.reprojection.out_srs="EPSG:28356"`


However - instead of using increasingly long command line processes, the `pipeline` application allows creation of a library of standard processing tasks as easily-readable JSON files.

## Diving right in - labelling ground points

Classifying ground points is a fundamental task for point cloud processing. LiDAR data can often exploit 'last returns' for ground classification - and usually any LiDAR you come across in public repositories already has ground points labelled (it's often a requirement in acquisition contracts).

However, sometimes the classification is not amazing - it's hard, especially if the surveyed area contains a mixture of terrain types and objects. Further, if you've just collected an incredible dataset from your RPAS and pushed it through OpenDroneMap, it won't have ground classifications attached to points. So let's makes some, using the RPAS-derived sample `APPF-farm.laz`.

Create a file 'rpas-ground.json' and populate it with:
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
    },
    {
      "type":"writers.las",
      "filename":"APPF-ground-20-pmf.laz"
    }
  ]
}
```

...save it, then run:

`pdal pipeline rpas-ground.json`

...but wait! there's a whole lot of new there. We've stacked a whole lot of filters together. Here we see the convenience of the pipeline approach. Starting from the top, we are:

- using `filters.assign` to label all points as unclassified
- applying `filters.elm` (extended local minimum) to label 'low points' as noise (ASPRS LAS class 7)
- then applying `filters.outlier`, using a statisical approach label remaining outlying points as noise
- next, using `filters.smrf` (Simple Morphological Filter) to label points as 'ground'
- ...then finally, removing any points *not* labelled as ground from the output and writing them out to `APPF-ground.laz`

Once you've got an output file, if you have CloudCompare (or another LAS/LAZ viewer), open `APPF-ground.laz` and check the results.

Here we've leaped right into the deep end with a long chain of processing. The point here is showing how it's actually pretty easy - once you know what it is you need to do. We've used a `reader`, a bunch of `filters` chained together to operate on a `pointview`, and exported the result using a `writer`

Try pulling apart the pipeline and running parts of it, or removing some of the filters and see what happens


## Overriding it all

In the example above, input and output filenames are fixed, and if we want to alter parameters we need to go edit a JSON file and re run everything. That's a little painful, especially if you have a thousand tiles to process!

We can fix that - using either command line overrides, or for clever folks, templating in JSON (we'll get to that shortly using Python).

Let's modify our pipeline a little to remove the final filter, and write out the entire dataset with noise a ground points labelled:

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
      "type":"writers.las",
      "filename":"APPF-ground-pmf.laz"
    }
  ]
}
```

...and for argument's sake, write out a different filename:

`pdal pipeline rpas-ground.json --writers.las.filename="some-different-file.laz"`

We can also modify filter parameters, to tune how points are labelled:

`pdal pipeline rpas-ground.json --filters.elm.cell=20.0 --filters.pmf.initial_distance=0.4`

## I want to make a product from my data

Something about creating DSMs and meshes etc here.


## Summary

[next - python and PDAL](4-python-and-pdal.md)
