# R spatial notes

All notes related to experiments to deal and learn about using spatial
data with R.

## Exporting a COG (Cloud Optimized Geotiff) file from GEE

The idea is to document steps to export from Google Earth Engine a COG
file (or several) to Google Drive to be imported latter in R for
analysis.

As a first approach, I followed a tutorial from the [Google
documentation:](https://developers.google.com/earth-engine/guides/exporting).

The code below is a copy/paste from the GEE IDE:

    // Load a landsat image and select three bands.
    var landsat = ee.Image('LANDSAT/LC08/C01/T1_TOA/LC08_123032_20140515')
      .select(['B4', 'B3', 'B2']);

    // Create a geometry representing an export region.
    var geometry = ee.Geometry.Rectangle([116.2621, 39.8412, 116.4849, 40.01236]);

    // Retrieve the projection information from a band of the original image.
    // Call getInfo() on the projection to request a client-side object containing
    // the crs and transform information needed for the client-side Export function.
    var projection = landsat.select('B2').projection().getInfo();

    // Export a cloud-optimized GeoTIFF.
    Export.image.toDrive({
      image: landsat,
      description: 'imageToCOGeoTiffExample',
      crs: projection.crs,
      crsTransform: projection.transform,
      region: geometry,
      folder: "GEE_testing",
      fileFormat: 'GeoTIFF',
      formatOptions: {
        cloudOptimized: true
      }
    });

After the COG file was exported to my Google Drive, I downloaded to my
data folder. Then, this file was imported to my R session with the help
of the `terra` package:

    library(terra)

    check <- rast("data/imageToCOGeoTiffExample.tif")

And, to validate it, I plotted the file using the `terra` function:

    plot(check)

    plotRGB(check, axes = TRUE, stretch = "lin", main = "Landsat True Color Composite")

### References

-   <https://rspatial.org/terra/pkg/1-introduction.html>
-   <https://geocompr.robinlovelace.net/read-write.html#raster-data-read>
-   <https://rspatial.org/raster/rs/2-exploration.html>
-   <https://developers.google.com/earth-engine/guides/exporting>

## Updating GDAL in Ubuntu

For using the `terra` package or other spatial packages in R, it’s
necessary to install in your system [GDAL](https://gdal.org/index.html).
There are several steps to follow to do this.

In my case, I had already GDAL on my system but it was outdated.

    $ gdalinfo --version
    GDAL 3.0.4, released 2020/01/28

So, I removed first the system packages:

    sudo apt remove libgdal-dev
    sudo apt remove libproj-dev
    sudo apt remove gdal-bin

Then I proceed to add the [PPA
ubuntugis](https://launchpad.net/~ubuntugis/+archive/ubuntu/ppa)

    sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable

Finally, I installed the necessary packages:

    sudo apt-get install gdal-bin
    sudo apt-get install libgdal-dev libgeos-dev libproj-dev 
    sudo apt update
    sudo apt upgrade
    sudo apt autoremove

And now, my GDAL version is:

    $ gdalinfo --version
    GDAL 3.3.2, released 2021/09/01

## `terra` package tutorial

All the code and examples here were created using the information in
[rspatial.org](https://rspatial.org/terra/rs/1-introduction.html)

## Downloading example data

    if (!fs::dir_exists("data")) {
      dir.create("data", showWarnings = FALSE)
    }

    if (!file.exists("data/rs/samples.rds")) {
        download.file("https://biogeo.ucdavis.edu/data/rspatial/rs.zip", dest = "data/rs.zip")
        unzip("data/rs.zip", exdir="data")
    }

## Working with MODIS data

The documentation states that we should work with the `luna` package,
but it’s not on CRAN, so I downloaded directly from the
[GitHub](https://github.com/rspatial/luna)

    # remotes::install_github("rspatial/luna")
    library(terra)
    library(luna)

    # Check products available through package API's
    prod <- getProducts()
    head(prod)

    # Check just the MODIS products
    modis <- getProducts("^MOD|^MYD|^MCD")
    head(modis)


    # To redirect to MODIS info webpage
    ## Examples comes with MOD09A1
    ## I'm interested in MOD09GA
    productInfo("MOD09GA")

    # Queremos descargar los datos
    ## Necesitamos area de interes
    ## Fecha de inicio y de fin

    nicoya <- geodata::gadm("Costa Rica", level = 1, path = ".")
    nicoya

    guanacaste <- nicoya[nicoya$NAME_1 == "Guanacaste", ] 

    # Plot area
    plot(nicoya, col = "light gray")
    lines(guanacaste, col = "red", lwd = 2)

    # Check MODIS data available for the area of interest
    mf <- luna::getModis(product = "MOD09GA", 
                         start_date = "2015-01-01", 
                         end_date = "2015-01-07", 
                         aoi = guanacaste,
                         download = FALSE)
    mf

    # Let's download some data
    username <- Sys.getenv("EARTHDATA_USER")
    passwd <- Sys.getenv("EARTHDATA_PASSWD")

    mf <- luna::getModis(product = "MOD09GA", 
                         start_date = "2015-01-01", 
                         end_date = "2015-01-07", 
                         aoi = guanacaste,
                         download = TRUE,
                         path = here::here("data"),
                         username = username,
                         password = passwd)
    mf

    # At this point, we should have the data in the path indicated

    # Explore the downloaded data:
    library(terra)

    file <- file.path(here::here(), "data/MOD09GA.A2015006.h09v07.006.2015295062629.hdf")
    r <- rast(file)


    datadir <- file.path(dirname(tempdir()), "_modis")
    mf <- file.path(datadir, "MOD09A1.A2009361.h21v08.006.2015198070255.hdf")
    r <- rast(mf[1])
    r

I have the problem that I cannot read the downloaded files

    # with same steps as tutorial
    product <- "MOD09A1"
    start <- "2010-01-01"
    end <- "2010-01-07"


    ken <- geodata::gadm("Kenya", level=1, path=".")
    ken

    i <- ken$NAME_1 == "Marsabit"
    aoi <- ken[i,]

    plot(ken, col="light gray")
    lines(aoi, col="red", lwd=2)

    mf <- luna::getModis(product, start, end, aoi=aoi, download = FALSE)
    mf

    datadir <- file.path(dirname(tempdir()), "_modis")
    dir.create(datadir, showWarnings=FALSE)

    mf <- luna::getModis(product, start, end, aoi=aoi, download=TRUE,
                         path=datadir, username = username,
                         password = passwd)
    mf

    library(terra)

    datadir <- file.path(dirname(tempdir()), "_modis")

    mf <- file.path(datadir, "MOD09A1.A2009361.h21v08.006.2015198070255.hdf")

    r <- terra::rast(mf[1])
    r

-   # <https://rspatial.org/terra/modis/4-quality.html>
