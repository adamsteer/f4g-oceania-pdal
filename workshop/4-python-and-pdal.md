# PDAL + python

PDAL can run python code, or be run *as* python code. Why Python? That's a user demand question for it's developers!

In order to make it go, PDAL needs to be compiled with Python bindings. Fortunately that happens already for both Conda and Docker installation. If you're building PDAL from scratch, make sure you include Python.

## Using Python functions in pipelines

Python functions can be run as part of a `pipeline` using `filters.python`. This is handy for customised processing - for example reducing colour bitness, or inverting a dimension. Here's an example pipeline calling a Python function to invert the Z Dimension for a multibeam SONAR dataset. To keep you on your toes, I've introduced a new `readers.text` here - and show a way to go from XYZ to LAZ (or any PDAL writable format)

```
[
  {
      "type" : "readers.text",
      "filename" : "inputfile.txt",
      "separator" : " ",
      "header" : "X Y Z Intensity",
      "skip": 0
    },
    {
      "type":"filters.python",
      "script":"invert_z.py",
      "function":"invert_z",
      "module":"anything"
    },
    {
      "type":"writers.las",
      "filename":"file-filtered.laz"
      "compression":"laszip"
    }
  ]
```

The python function looks like this:

```
def invert_z(Ins, Outs):
    Z = Ins['Z']
    Outs['Z'] = -Z
```

...and is saved in the working directory as `invert-z.py`

Pretty simple, right? Your Python function can get as complex as you like. Just remember that PDAL's 'read, process, write' strategy extends to it's Python API also.

## PDAL as a python library

This is best demonstrated in a Jupyter notebook. Head to the command line prompt, and in this repository switch to the 'notebooks' folder.

Start your virtual environment:

`conda activate f4g-pdal-workshop`

..and start Jupyter:

`jupyter notebook`

...which should give you access to [the notebook](../notebooks/PDAL-python.ipynb). It's a fully worked example of classifying ground in a subset of the Dublin lidar dataset, and introduces a sorting filter for space filling curves: `filters.mortonorder`. We'll step through  the notebook, and feel free to experiment with it!

## Summary

Python functionality can be added to PDAL using a pipeline `filter` option. PDAL functionality can be added to python using PDAL as a module with few dependencies (primarily numpy). The canonical reference is here: https://pdal.io/python.html

[next - entwine](5-entwine.md)
