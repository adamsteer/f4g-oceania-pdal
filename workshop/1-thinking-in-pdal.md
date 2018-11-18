# Thinking in PDAL

PDAL has a pattern you'll repeat all the time when working with it: operations happen on `dimensions` in `stages` which create `point views`.

Reading metadata is a special case, which doesn't create a `point view`, but does make use of `readers`.

These operational components are explained below - we'll keen coming back to this pattern as we process data throughout the workshop. The canonical reference on [PDAL architecture](https://pdal.io/development/overview.html#) is a handy guide.

## Dimensions

In PDAL, `dimensions` are the set of things described in the point cloud data schema. For LAS format files, dimensions are set within ASPRS LAS standard bounds. Some common dimensions are:

|Name | Description |
|-----|-------------|
|X | 'X' coordinate (longitude in EPSG:3246)|
|Y | 'Y' coordinate (latitude in EPSG:4326)|
|Z | 'up' coordinate |
|Intensity | Laser return intensity |
|Red | Red channel in RGB colour |
|Green | Green channel in RGB colour |
|Blue | Blue channel in RGB colour |

The list of standard PDAL dimensions is here: https://pdal.io/dimensions.html - again, many of these are derived from ASPRS LAS formats. Primarily because it's a data exchange format we can't ignore!



## Stages

A stage is the PDAL way of describing a thing which operates on points. That's it!

## Readers


Reference: https://pdal.io/stages/readers.html

## Filters


Reference: https://pdal.io/stages/filters.html

## Writers


Reference: https://pdal.io/stages/writers.html


[next - command line processes](2-command-line-processes.md)
