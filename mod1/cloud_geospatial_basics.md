# Cloud Native Geospatial Basics

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

