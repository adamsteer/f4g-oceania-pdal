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

The list of standard PDAL dimensions is here: https://pdal.io/dimensions.html - again, many of these are derived from ASPRS LAS formats. Primarily because it's a data exchange format we can't ignore, and second - it's a standard, and while never perfect, standards make implementation easier.

## Stages

A stage is the PDAL way of describing a processing step which operates on points. That's it!

## Readers

Readers are a stage which reads in data from a filesystem into a `pointview` - which can be operated on by `filters`. PDAL includes a range of 'built in' readers, and others can be added or removed as plugins as build time. You can also write your own.

Reference: https://pdal.io/stages/readers.html

## Filters

Once you've read data into a `pointview`, filters are where the magic happens - they're methods for point data processing! YOu can stack filters together to form complex 'one liners' - with the `pointview` modified by one `filter` being passed to the next. The main caveat here is to be aware of how each filter modifies the view, in order to get the results you expect.

Reftererence: https://pdal.io/stages/filters.html

## Writers

As the name suggests, writers are a stage which writes out a `pointview` to a file! As for readers, PDAL includes a range of 'built in' writers, and others can be added or removed as plugins as build time. You can also write your own.

Reference: https://pdal.io/stages/writers.html

## Convenience applications

PDAL uses convenience applications to wrap stages into one-liners. For example:

`pdal translate -i in.las -o out.las`

...is shorthand for expressing a reader, filter, and writer to translate point data from one format to another.

Reference: https://pdal.io/apps/

## Summary

PDAL thinks in stages and point views. Readers to create views, filters to operate on views, and writers to write views out. Keep that in mind... everything else operates around that!

[next - command line processes](2-command-line-processes.md)
