# Cloud Native Geospatial Basics

## Table of Contents
1. [Principles of Cloud Native Geospatial](#principles-of-cloud-native-geospatial)
2. [Key Open-Source Technologies](key-open-source-technologies)
3. [XArray](#xarray-1)
4. [Spatio-Temporal Asset Catalog (STAC)](#spatio-temporal-asset-catalog-stac)
5. [Dask](#dask-1)
6. [Coiled](#coiled)

## Cloud Native Remote Sensing

Traditional remote sensing methods require **downloading and processing individual scenes locally**.

Modern cloud-based approaches enable the use of **analysis-ready data hosted in the cloud** that can be **analyzed by many machines in parallel**.

Development of new data formats and advances in open-source technologies now allows us to build cloud-native geospatial workflows that are **open, scalable, and cloud-agnostic**.

## Principles of Cloud Native Geospatial

1. ### Co-located data and computation

> Maximize the efficiency by location the data close to compute resources

2. ### Analysis-ready data

> Provide datasets in formats that can be streamed and used directly

3. ### Lazy loading (or deferred execution)

> Compute the results only when required

4. ### Distributed (parallel) computation

> Divide the computation tasks into smaller chunks and distribute them over a pool of compute resources

## Key Open-Source Technologies

1. ### Cloud-Optimized GeoTiffs (COGs)

> - Allows streaming specific parts of images directly without downloading the entire file
> - Extends a regula GeoTiff file by adding overviews and a header

2. ### Spatio-Temporal Asset Catalogs (STAC)

> - An open standard to describe and query geospatial data
> - Ecosystem of tools and APIs to query any catalog of geospatial data

3. ### XArray

> - Allows processing multidimensional gridded rasters easily with built-in functions for time-series analysis
> - Orders of magnitude faster than alternatives (e.g., `rasterio`, `terra`, etc.)
> - Growing ecosystem (`rioxarray`, `xr-spatial`, `xr-scipy`, `XEE`, etc.)

4. ### Dask

> - Python library to run your computation in parallel across many machines
> - Built-in support for many Python libraries (`pandas`, `scikit-learn`, `xarray`, etc.)
> - Allows you to run your code on any cluster (laptop, local cluster, cloud cluster, etc.)

5. ### Jupyter

> - Web-based interactive development environment
> - Support for deploying custom computing environments for multiple users

## XArray

### What is XArray?

XArray is a Python package for working with multidimensional arrays. It supports vectorized operations on arrays, resulting in magnitudes of faster processing over iteration. It is particularly suited for working with multi-band and/or time-series rasters (earth observation and climate datasets).

XArray integrates tightly with `dask`, which allows one to scale raster data processing using parallel computing. It is also at the center of a fast-evolving ecosystem around spatial extensions (`rioxarray`, `xarray-spatial`, `xr-scipy`, etc.).

### Basic Terminology

- #### Variables

> This is similar to a band in a raster dataset. Each variable contains an array of values.

- #### Dimensions

> This is similar to the number of array axes. A grid of pixels (lat and lon) at multiple time internals with multiple variables is a 4D dataset.

- #### Coordinates

> These are the labels for values in each dimension. We have labels for lat, lon, and time.

- #### Attributes

> This is the metadata associated with the dataset.

Below is the diagram for a multi-band raster data at a single time interval.

![A single multi-band image](imgs/multiband_image.png)

Let's expand the example to include a time-series of images:

![A time-series of images](imgs/time_series.png)

For this time-series of multi-band images, the  `dimensions` are: lon, lat, time, and variables. The `variables` correspond to the bands of each image (blue, green, red, and NIR), while the rest of the dimensions are `coordinates` indicating location across space (lat and lon) and time.

![Dimensions, variables, and coordinates](imgs/dims_vars_coords.png)

## Spatio-Temporal Asset Catalog (STAC)

### What is STAC?

Spatio-Temporal Asset Catalog (STAC) is an open standard for specifying and querying geospatial data.

Data providers can share catalogs of satellite imagery, climate datasets, LIDAR data, vector data, etc., and specify asset metadata according to the STAC specifications. All STAC catalogs can be queried to find matching assets by time, location, or metadata.

There are many open-source packages available in Python to query STAC catalogs.

### STAC Catalogs

All available catalogs can be browsed at https://stacindex.org/

There are two types of STAC catalogs:

1. #### Static Catalogs

> - Browsable catalog that can be easily created by the providers to expose cloud-hosted datasets.
> - STAC Browser is a great way to explore static catalogs

2. #### STAC API Catalogs

> - Searchable catalogs that support advanced features (search, filter, etc.)
> - These catalogs are able to index large volumes of data (e.g., EO or climate data)
> - The Python package `pystac-client` can query STAC API catalogs using `query` or `filter` extensions.

### Reading Data from Static Catalogs

One can iterate through a static `Collection` and find `STAC Items`. Each `STAC Item` has references to `Assets`, each of which is typically a set of cloud-optimized GeoTIFFs (COGs) when working with EO data. These assets can be directly read using `rioxarray`, which creates an XArray dataset.

### Reading Data from STAC API Catalogs

Meanwhile, query results from STAC API catalogs can be loaded directly to XArray. There are two popular options for this: `stackstac` and `odc-stac`. There are many small differences in how these two packages load data. 

First, `odc-stac` returns an `xarray.Dataset`, with each band in a different variable. Meanwhile, `stackstac` returns an `xarray.DataArray`, with all the bands combined into a single array. Moreover, `odc-stac` has sensible defaults and more tooling available to make data processing easier.

A deeper discussion is available at [comparing odc-stac vs stackstack](https://discourse.pangeo.io/t/comparing-odc-stac-load-and-stackstac-for-raster-composite-workflow/4097/8). This workshop will use `odc-stac`, but either option is fine.

## Dask

Dask is an open-source Python package for parallel computing. It allows the processing of big data by implementing distributed computation for widely used Python libraries (`pandas`, `geopandas`, `xarray`, etc.).

Dask is flexible and can be run on many environments:
- On your own computer, leveraging one, multiple, or all processing cores
- On your own private cluster
- On a cloud cluster

## Coiled

Coiled is a platform that allows you to easily set up cloud infrastructure for distributed processing. It provides a Python package and a notebook service, making it very easy to scale `xarray` + `dask` workflows to process large volumes of data.

It suupports all major cloud platforms (GCP, AWS, Azure), and seamlessly sets up a Dask cluster that you can access via a Jupyter notebook from your own machine.