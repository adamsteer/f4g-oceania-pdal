# Summary, boundaries and futures

This has been a really fast run through a lot of capabilities and concepts.

The primary aim of this workshop was to show you how PDAL sees the world, and some ways of interacting with it. You should walk away with some idea about the concept of PDAL pipelines:

- `readers`
- `filters` on point `views`
- `writers`

...regardless of how you apply them (using Python, command line, docker, something else>).

I hop you've also got some ideas about employing those concepts from the set of exercises and discussion here.

You should also know something about entwine - and how it can be used as both a visualisation and processing data source for point clouds.

And you should have had some opportunity to discuss all of this with your facilitator and colleagues.

## Boundaries

PDAL doesn't do everything for you. We did no parallel tasking here - PDAL is single-threaded, and you need to figure out how to divide-and-conquer. Many utilities exist to help with this, which make for another workshop! PDAL also focusses on flexibility. If you're after pure speed, another way might work better for you.

...and there's no GUI.

## Futures

What do you see? Discussion time...

[Acknowledgements](7-acknowledgements.md)
