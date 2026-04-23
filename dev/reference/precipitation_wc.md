# Calculate precipitation statistics

This function allows to efficiently calculate precipitation statistics
from Worldclim for polygons. For each polygon, the desired statistic/s
(min, max, sum, mean, median, sd or var) is/are returned.

## Usage

``` r
calc_precipitation_wc(engine = "extract", stats = "mean")
```

## Arguments

- engine:

  The preferred processing functions from either one of "zonal",
  "extract" or "exactextract" as character.

- stats:

  Function to be applied to compute statistics for polygons either
  single or multiple inputs as character. Supported statistics are:
  "mean", "median", "sd", "min", "max", "sum" "var".

## Value

A function that returns an indicator tibble with precipitation
statistics as variable and corresponding values as value.

## Details

The required resources for this indicator are:

- precipitation layer from
  [worldclim_precipitation](https://mapme-initiative.github.io/mapme.biodiversity/dev/reference/worldclim_precipitation.md)

## Examples

``` r
# \dontrun{
library(sf)
library(mapme.biodiversity)

outdir <- file.path(tempdir(), "mapme-data")
dir.create(outdir, showWarnings = FALSE)

mapme_options(
  outdir = outdir,
  verbose = FALSE
)

aoi <- system.file("extdata", "sierra_de_neiba_478140_2.gpkg",
  package = "mapme.biodiversity"
) %>%
  read_sf() %>%
  get_resources(get_worldclim_precipitation(years = 2018)) %>%
  calc_indicators(
    calc_precipitation_wc(
      engine = "extract",
      stats = c("mean", "median")
    )
  ) %>%
  portfolio_long()
#> Error in sf::gdal_utils(util = util, source = source, destination = destination,  : 
#>   gdal_utils translate: an error occured
#> Warning: GDAL Error 1: In file ./port/cpl_vsil_gzip.cpp, at line 1207, decompression failed with z_err = -1, return = 609
#> Warning: GDAL Error 1: TIFFFillStrip:Read error at scanline 460; got 609 bytes, expected 8274
#> Warning: GDAL Error 1: TIFFReadEncodedStrip() failed.
#> Warning: GDAL Error 1: /vsizip//vsicurl/https://geodata.ucdavis.edu/climate/worldclim/2_1/hist/cts4.09//wc2.1_cruts4.09_2.5m_prec_2010-2019.zip/wc2.1_cruts4.09_2.5m_prec_2018-06.tif, band 1: IReadBlock failed at X offset 0, Y offset 461: TIFFReadEncodedStrip() failed.

aoi
#> Simple feature collection with 24 features and 8 fields
#> Geometry type: POLYGON
#> Dimension:     XY
#> Bounding box:  xmin: -71.80933 ymin: 18.57668 xmax: -71.33201 ymax: 18.69931
#> Geodetic CRS:  WGS 84
#> # A tibble: 24 × 9
#>    WDPAID ISO3  assetid indicator       datetime            variable unit  value
#>     <dbl> <chr>   <int> <chr>           <dttm>              <chr>    <chr> <dbl>
#>  1 478140 DOM         1 precipitation_… 2018-01-01 00:00:00 worldcl… mm     26.7
#>  2 478140 DOM         1 precipitation_… 2018-01-01 00:00:00 worldcl… mm     26.7
#>  3 478140 DOM         1 precipitation_… 2018-02-01 00:00:00 worldcl… mm     26.1
#>  4 478140 DOM         1 precipitation_… 2018-02-01 00:00:00 worldcl… mm     26.5
#>  5 478140 DOM         1 precipitation_… 2018-03-01 00:00:00 worldcl… mm     66.7
#>  6 478140 DOM         1 precipitation_… 2018-03-01 00:00:00 worldcl… mm     68.6
#>  7 478140 DOM         1 precipitation_… 2018-04-01 00:00:00 worldcl… mm     82.0
#>  8 478140 DOM         1 precipitation_… 2018-04-01 00:00:00 worldcl… mm     82.1
#>  9 478140 DOM         1 precipitation_… 2018-05-01 00:00:00 worldcl… mm    330. 
#> 10 478140 DOM         1 precipitation_… 2018-05-01 00:00:00 worldcl… mm    338. 
#> # ℹ 14 more rows
#> # ℹ 1 more variable: geom <POLYGON [°]>
# }
```
