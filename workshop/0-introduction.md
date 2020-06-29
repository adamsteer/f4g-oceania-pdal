# Workshop agenda

In this workshop, we're aiming to introduce you to a point cloud processing and visualisation workflow using the Point Data Abstraction Library (PDAL).

On the way we will also use Python, Entwine and the Potree WebGL-based viewer to interrogate point cloud data, perform some basic analyses, modify our points and visualise them in a web browser.

A key component of this workshop is explaining PDAL's pipeline system, as well as providing parameters to processing pipelines via command line input. We'll drive PDAL using a combination of docker command line invocations, and exploiting it as a Python library.

Finally, we'll use Entwine to create a visualisation-ready version of our data, and explore it in the Potree viewer.

In this workshop we will learn by doing - it'll be a bit of a magical mystery tour in that many concepts will be shown, then discussed.

## Getting started

If you haven't already, open a command line window (if you can use bash, great!) and do:

`git pull https://github.com/adamsteer/f4g-oceania-pdal.git`

...then:

`cd f4g-oceania-pdal`

If you haven't already, please download the sample datasets listed [here](../README.md)

...and move them to the directory `f4g-oceania-pdal/sample-data`

All of our exercises today are run from there.

## Questions and feedback

Feel free to ask questions anytime. Be aware you may be asked to wait for an answer if it is coming later in the session! If you are nervous about asking questions or drawing attention to yourself in the group, please place a single sticky note somewhere a helper will notice it on your laptop as we wander around. Or, for online delivery, please feel free to send private chat queries to the facilitator.

Please give feedback! In person we'll use software carpentry style 1 up, 1 down anonymous feedback on sticky notes. At break time stick them to the wall, and I'll read them. If anything can be improved for the second session, we'll try to do it.

For online delivery, anonymous feedback will be ...

A feedback session will be held at the end. Don't be shy here - the longer you wait to provide input the harder your task will be!

## Running order

### Session 1 [100 minutes]
[10 min] Introducing instructors and helpers, housekeeping, code of conduct.

#### Introductions
[15 min] Who are you? why are you here? what do you want to achieve?

#### Preamble
[15 min] Point clouds, why PDAL, what are some alternatives, room for discussion

#### Starting out with PDAL
[30 min] Thinking in PDAL, PDAL on the command line, using conda or docker - to query metadata and points; Introduction to processing pipelines.

#### Processing data with PDAL
[30 min] Using a pipeline to find ground points, and reduce noise

### Break [20 min]

### Session 2 [90 minutes]

#### Python and PDAL
[30 min] Using python code in PDAL, using PDAL as a python library with Jupyter notebooks for exploratory analysis

#### Entwine
[20 min] Introduction to Entwine; creating an entwine EPT index; Setting up and viewing entwine datasets on a local web server

#### Future visions and discussion

[10 min] Where I see this going, and where do **you** see this going?

#### Formal feedback discussion
[10 min] Wrap up; one up, one down feedback

[next - pointclouds](0-pointclouds.md)
