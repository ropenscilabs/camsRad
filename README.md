
<!-- README.md is generated from README.Rmd. Please edit that file -->
camsRad
=======

[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active) [![Travis-CI Build Status](https://travis-ci.org/ropensci/camsRad.svg?branch=master)](https://travis-ci.org/ropensci/camsRad) [![codecov.io](https://codecov.io/gh/ropenscilabs/camsRad/coverage.svg?branch=master)](https://codecov.io/gh/ropenscilabs/camsRad) [![](https://badges.ropensci.org/72_status.svg)](https://github.com/ropensci/onboarding/issues/72)

`camsRad` is a R client for [CAMS Radiation Service](http://www.soda-pro.com/web-services/radiation/cams-radiation-service). CAMS Radiation Service provides time series of global, direct, and diffuse irradiations on horizontal surface, and direct irradiation on normal plane for the actual weather conditions as well as for clear-sky conditions. The geographical coverage is the field-of-view of the Meteosat satellite, roughly speaking Europe, Africa, Atlantic Ocean, Middle East (-66° to 66° in both latitudes and longitudes). The time coverage of data is from 2004-02-01 up to 2 days ago. Data are available with a time step ranging from 15 min to 1 month. Target audience are researchers, developers and consultants in need of high resolution solar radiations time series.

Quick start
-----------

### Install

Dev version from GitHub.

``` r
# CRAN version
install.packages("camsRad")

# Or Github version
if (!require('devtools')) install.packages('devtools')
devtools::install_github("ropensci/camsRad")
```

``` r
library("camsRad")
```

### Authentication

To access the CAMS Radiation Service you need to register at <http://www.soda-pro.com/web-services/radiation/cams-radiation-service>. The email you use at the registration step will be used for authentication, and need to be set with `cams_set_user()`.

``` r
# Authentication
cams_set_user("your@email.com") # An email registered at soda-pro.com
```

### Example 1

Get hourly CAMS solar data into a R data frame. For the location 60° latitude and 15° longitude, and for period 2016-01-01 to 2016-01-15.

``` r

df <- cams_get_radiation(
  lat=60, lng=15, 
  date_begin="2016-07-01", 
  date_end="2016-07-01")
print(df)
```

### Example 2

Retrieve daily CAMS solar data in netCDF format. You need to have the `ncdf4` package installed.

``` r
library(ncdf4)

filename <- paste0(tempfile(), ".nc")

r <- cams_api(
  60, 15, "2016-06-01", "2016-06-10", 
  format="application/x-netcdf",
  time_step = "P01D",
  filename=filename)

# Access the on disk stored ncdf4 file 
nc <- nc_open(filename)

# list names of available variables
names(nc$var)

# create data.frame with timestamp and global horizontal irradiation and plot it
df <- data.frame(
  timestamp = as.POSIXct(nc$dim$time$vals, "UTC", origin="1970-01-01"),
  GHI = ncvar_get(nc, "GHI"))

plot(df, type="l")

nc_close(nc)
```

Meta
----

-   This package and functions herein are provided as is, without any guarantee.
-   Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.
-   Please [report any issues or bugs](https://github.com/ropensci/camsRad/issues).

<!--[![ropensci_footer](http://ropensci.org/public_images/github_footer.png)](https://ropensci.org) 
doesn´t knit. add following to the .md file
[![ropensci\_footer](http://ropensci.org/public_images/github_footer.png)](https://ropensci.org)
-->
[![ropensci\_footer](http://ropensci.org/public_images/github_footer.png)](https://ropensci.org)
