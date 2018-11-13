# The PDAL pipeline

PDAL's pipeline processing approach is a powerful tool for enablign complex, automated and repeatable workflows. We've already seen it in action exploring command line processing, and the concepts of readers, writers and filters.

What we've typed on the terminal is really invoking a PDAL pipeline.

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
        "in_srs":"EPSG:4326",
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

`pdal translate inputfile.las outputfile.laz --filters.reprojection.in_srs="EPSG:4326" --filters.reprojection.out_srs="EPSG:28356"`



##



##
