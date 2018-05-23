<!-- README.md is generated from README.Rmd. Please edit that file -->
[![Build
Status](http://badges.herokuapp.com/travis/hypertidy/vapour?branch=master&env=BUILD_NAME=trusty_clang&label=trusty_clang)](https://travis-ci.org/hypertidy/vapour)
[![Build
Status](http://badges.herokuapp.com/travis/hypertidy/vapour?branch=master&env=BUILD_NAME=osx&label=osx)](https://travis-ci.org/hypertidy/vapour)
[![Build
Status](http://badges.herokuapp.com/travis/hypertidy/vapour?branch=master&env=BUILD_NAME=mingw_w64&label=mingw_w64)](https://travis-ci.org/hypertidy/vapour)
[![Coverage
Status](https://img.shields.io/codecov/c/github/hypertidy/vapour/master.svg)](https://codecov.io/github/hypertidy/vapour?branch=master)
[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/vapour)](https://cran.r-project.org/package=vapour)

vapour
======

We aim to provide access to the basic functions available in GDAL, which
loosely provides both a
[raster](http://www.gdal.org/gdal_datamodel.html) and a
[vector](http://www.gdal.org/ogr_arch.html) data model.

The workflows available are intended to support development of
applications in R for these vector and [raster
data](https://en.wikipedia.org/wiki/Raster_data) without being
constrained to any particular data model.

Installation
------------

The package can be installed from Github.

``` r
devtools::install_github("hypertidy/vapour")
```

You will need development tools for building R packages.

On Linux and MacOS you’ll need an available GDAL installation, but on
Windows the ROpenSci rwinlib tools are used and the required GDAL will
be downloaded and compiled against when building the package.

Purpose
-------

The goal of vapour is to provide a basic **GDAL API** package for R. The
priority is to give low-level access to key functionality, not
comprehensive coverage of the library. There is only read-only
facilities, and no conversion to specialist types in R. Ideally, this
could become a common foundation for other packages to specialize, but
at the very least provides minimal examples to use GDAL for specialist
needs.

A parallel goal is to be freed from the powerful but sometimes limiting
high-level data models of GDAL itself, specifically these are *simple
features* and *affine-based regular rasters composed of 2D slices*.
(GDAL will possibly remove these limitations over time but still there
will always be value in having modularity in an ecosystem of tools. )

This partly draws on work done in [the sf
package](https://github.com/r-spatial/sf) and in packages `rgdal` and
`rgdal2`. I’m amazed that something as powerful and general as GDAL is
still only available through these lenses, but recentish improvements
make things much easier touse and share. Specifically `Rcpp` means that
access to external libs is simplified, easier to learn and easier to get
started and make progress. The other part is that cross-platform support
is now much better, with more consistency on the libraries available on
the CRAN machines and in other contexts.

Examples of packages that use vapour are in development,
[RGDALSQL](https://github.com/mdsumner/RGDALSQL) and
[lazyraster](https://github.com/hypertidy/lazyraster). `RGDALSQL` aims
to leverage the facilities of GDAL to provide data *on-demand* for many
sources *as if* they were databases. `lazyraster` uses the
level-of-detail facility of GDAL to read just enough resolution from a
raster source using traditional window techniques.

GDAL for data
=============

There are tools for reading from both vector and raster data sources.

Vector data
-----------

-   read attributes only
-   read geometry only
-   read geometry as raw binary, or various text forms
-   read geometry bounding box only
-   optionally, apply [OGRSQL](http://www.gdal.org/ogr_sql.html) to a
    layer prior to above read calls

Raster data
-----------

-   find structural metadata for a single raster source
-   find available single rasters within a complex source
-   read from a raster arbitrarily using GDAL very flexible [IO
    framework](http://www.gdal.org/classGDALRasterBand.html#a30786c81246455321e96d73047b8edf1).

Limitations, work-in-progress and other discussion are active here:
<https://github.com/hypertidy/vapour/issues/4>

Warnings
========

It’s possible to give problematic “SELECT” statements via the `sql`
argument. Note that the geometry readers `vapour_read_geometry`,
`vapour_read_geometry_text`, and `vapour_read_extent` will strip out the
`SELECT ... FROM` clause and replace it with `SELECT * FROM` to ensure
that the geometry is accessible, though the attributes are ignored. This
means we can allow the user or `dplyr` to create any `SELECT` statement.
The function `vapour_read_geometry_what` will return a list of NULLs, in
this case.

I haven’t tried this against a real database, I’m not sure if we need
`AsBinary()` around EWKB geoms, for example - but at any rate these can
be ingested by `sf`.

Examples
--------

The package documentation page gives an overview of available functions.

``` r
help("vapour-package")
```

See the vignettes and documentation for examples.

Set up
------

I’ve kept a record of a minimal GDAL wrapper package here:

<https://github.com/mdsumner/gdalmin>

This must be run when your function definitions change:

Context
-------

My first real attempt at DBI abstraction is here, this is still an
aspect that is desperately needed in R to help bring tidyverse attention
to spatial:

<https://github.com/mdsumner/RGDALSQL>

Before that I had worked on getting sp and dplyr to at least work
together <https://github.com/dis-organization/sp_dplyrexpt> and recently
rgdal was updated to allow tibbles to be used, something that spbabel
and spdplyr really needed to avoid friction.

Early exploration of allow non-geometry read with rgdal was tried here:
<https://github.com/r-gris/gladr>

Big thanks to Edzer Pebesma and Roger Bivand and Tim Keitt for prior art
that I crib and copy from. Jeroen Ooms helped the R community hugely by
providing an automatable build process for libraries on Windows. Mark
Padgham helped kick me over a huge obstacle in using C++ libraries with
R. Simon Wotherspoon and Ben Raymond have endured my ravings about
wanting this level of control for many years.

Code of conduct
===============

Please note that this project is released with a [Contributor Code of
Conduct](CONDUCT.md). By participating in this project you agree to
abide by its terms.
