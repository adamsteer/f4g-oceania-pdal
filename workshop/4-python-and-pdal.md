# PDAL + python

PDAL can run python code, or be run *as* python code. Why Python? Because thats what I use! PDAL also has Java and Julia bindings, which you can explore as you need to.

In order to make it go, PDAL needs to be compiled with Python bindings. Fortunately that happens already for both Conda and Docker installation. If you're building PDAL from scratch, make sure you include Python.

## Using Python functions in pipelines

Python functions can be run as part of a `pipeline` using `filters.python`. This is handy for customised processing - for example reducing colour bitness, or inverting a dimension. Here's an example pipeline calling a Python function to invert the Z Dimension - which might be useful if you're dealing with bathymetry data. To keep you on your toes, I've introduced a new `readers.text` here - and show a way to go from a text file to LAZ (or any PDAL writable format) as we go.

The input file for this example is in the resources directory for this repository: [../resources/bathymetry-example.txt](../resources/bathymetry-example.txt)

```
[
  {
      "type" : "readers.text",
      "filename" : "bathymetry-example.txt",
      "separator" : " ",
      "header" : "X Y Z Intensity",
      "skip": 1
    },
    {
      "type":"filters.python",
      "script":"invert_z.py",
      "function":"invert_z",
      "module":"anything"
    },
    {
      "type":"writers.las",
      "filename":"inverted-bathymetry.laz"
      "compression":"laszip"
    }
]
```

Copy and paste the pipeline into something like `invert-z.json`

The python function `invert_z` looks like this:

```
def invert_z(Ins, Outs):
    Z = Ins['Z']
    Outs['Z'] = -Z
```

Save this in your working directory as `invert-z.py`.

Run this pipeline like:

`pdal pipeline invert-z.json`

Pretty simple, right? Your Python function can get as complex as you like. Just remember that PDAL's `read, process, write` strategy extends to its external language bindings as well.

You can also declare Python functions entirely within a pipeline:
```
[
  {
      "type": "readers.text",
      "filename": "bathymetry-example.txt",
      "separator": " ",
      "header": "X Y Z Intensity",
      "skip": 1
    },
    {
      "type": "filters.python",
      "function": "invert_z",
      "source": "def invert_z(Ins, Outs):\n\tZ = Ins['Z']\n\tOuts['Z'] = -Z\n\treturn True",
      "module": "anything"
    },
    {
      "type":"writers.las",
      "filename":"inverted-bathymetry.laz",
      "compression":"laszip"
    }
]
```

You can see the results using `pdal info` - all the Z values in your new LAZ file should be below 0!

While it is possible to do this, it is likely a lot better for your sanity to use an external library of functions that are referred to in a pipeline, rather than declaring functions in the pipeline itself.

## PDAL as a python library

PDAL can also be driven completely within a Python environment. This is best demonstrated in a Jupyter notebook. Head to the command line prompt, and in this repository switch to the 'notebooks' folder.

Start your virtual environment:

`conda activate f4g-pdal-workshop`

..and start Jupyter:

`jupyter notebook`

...which should give you access to [the notebook](../notebooks/PDAL-python.ipynb). It's a fully worked example of classifying ground in a subset of the Dublin lidar dataset, and introduces a sorting filter for space filling curves: `filters.mortonorder`. We'll step through  the notebook, and feel free to experiment with it!

## Summary

Python functionality can be added to PDAL using a pipeline `filter` option. PDAL functionality can be added to python using PDAL as a module with few dependencies (primarily numpy). The canonical reference is here: https://pdal.io/python.html

[next - entwine](5-entwine.md)
