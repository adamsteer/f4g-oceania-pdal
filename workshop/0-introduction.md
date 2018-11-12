# Workshop agenda

In this workshop, we're aiming to introduce you to a point cloud processing and visualisation workflow using open source toolkits.

We'll use the Point Data Abstraction Library (PDAL), Python, Entwine and the Potree WebGL-based viewer to interrogate point cloud data, perform some basic analyses, modify our points and visualise them in a web browser.

On the way we'll learn how to use PDAL's pipeline system, as well as providing parameters to processing pipelines via command line input. We'll drive PDAL using a combination of docker command line invocations, and exploiting it as a Python library.

Finally, we'll use Entwine to create a visualisation-ready version of our data, and explore it in the Potree viewer.

In this workshop we will learn by doing - it'll be a bit of a magical mystery tour in that many concepts will be shown, then discussed.

## Getting started

If you haven't already, open a command line window (if you can use bash, great!) and do:

`git pull https://github.com/adamsteer/f4g-oceania-pdal.git`

...and:

`cd f4g-oceania-pdal`

If you haven't already, please download a few sample datasets from here:

...and move them to the directory `f4g-oceania-pdal/sample-data`

All of our exercises today are run from there.

## Running order

### Session 1 [1:30 - 3:10, 100 minutes]
[10 min] Introducing instructors and helpers, housekeeping.

#### Introductions
[10 min] Who are you? why are you here? what do you want to achieve?

#### Preamble
[20 min] Point clouds, why PDAL, what are some alternatives, room for discussion

#### Starting out with PDAL
[30 min] PDAL on the command line, using conda or docker - to query metadata and points; Introduction to processing pipelines.

#### Processing data with PDAL
[30 min] Using a pipeline to find ground points, and reduce noise

### Break [20 min]

### Session 2 [3:30 - 5:00 pm, 90 minutes]

#### Python and PDAL
[30 min] Using python code in PDAL, using PDAL as a python library with Jupyter notebooks for exploratory analysis

#### Entwine
[30 min] Introduction to Entwine; creating an entwine index; rendering 3D tiles

#### Visualisation using Potree
[20 min] Setting up and viewing entwine datasets on a local web server

#### Formal discussion
[10 min] Wrap up, and feedback

[next - pointclouds](0-pointclouds.md)
