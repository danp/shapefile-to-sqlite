# shapefile-to-sqlite

[![PyPI](https://img.shields.io/pypi/v/shapefile-to-sqlite.svg)](https://pypi.org/project/shapefile-to-sqlite/)
[![CircleCI](https://circleci.com/gh/simonw/shapefile-to-sqlite.svg?style=svg)](https://circleci.com/gh/simonw/shapefile-to-sqlite)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com/simonw/shapefile-to-sqlite/blob/master/LICENSE)

Load shapefiles into a SQLite (optionally SpatiaLite) database.

Project background: [Things I learned about shapefiles building shapefile-to-sqlite](https://simonwillison.net/2020/Feb/19/shapefile-to-sqlite/)

## How to install

    $ pip install shapefile-to-sqlite

## How to use

You can run this tool against a shapefile file like so:

    $ shapefile-to-sqlite my.db features.shp

This will load the geometries as GeoJSON in a text column.

## Using with SpatiaLite

If you have [SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite/index) available you can load them as SpatiaLite geometries like this:

    $ shapefile-to-sqlite my.db features.shp --spatialite

The data will be loaded into a table called `features` - based on the name of the shapefile. You can specify an alternative table name using `--table`:

    $ shapefile-to-sqlite my.db features.shp --table=places

The tool will search for the SpatiaLite module in the following locations:

- `/usr/lib/x86_64-linux-gnu/mod_spatialite.so`
- `/usr/local/lib/mod_spatialite.dylib`

If you have installed the module in another location, you can use the `--spatialite_mod=xxx` option to specify where:

    $ shapefile-to-sqlite my.db features.shp \
        --spatialite_mod=/usr/lib/mod_spatialite.dylib

## Projections

By default, this tool will attempt to convert geometries in the shapefile to the WGS 84 projection, for best conformance with the [GeoJSON specification](https://tools.ietf.org/html/rfc7946).

If you want it to leave the data in whatever projection was used by the shapefile, use the `--crs=keep` option.

You can convert the data to another output projection by passing it to the `--crs` option. For example, to convert to [EPSG:2227](https://epsg.io/2227) (California zone 3) use `--crs=espg:2227`.

The full list of formats accepted by the `--crs` option is [documented here](https://pyproj4.github.io/pyproj/stable/api/crs.html#pyproj.crs.CRS.__init__).
