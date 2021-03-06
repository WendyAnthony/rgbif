rgbif
=====



[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![cran checks](https://cranchecks.info/badges/summary/rgbif)](https://cranchecks.info/pkgs/rgbif)
[![Build Status](https://travis-ci.org/ropensci/rgbif.svg?branch=master)](https://travis-ci.org/ropensci/rgbif)
[![codecov.io](https://codecov.io/github/ropensci/rgbif/coverage.svg?branch=master)](https://codecov.io/github/ropensci/rgbif?branch=master)
[![rstudio mirror downloads](http://cranlogs.r-pkg.org/badges/rgbif)](https://github.com/metacran/cranlogs.app)
[![cran version](http://www.r-pkg.org/badges/version/rgbif)](https://cran.r-project.org/package=rgbif)
[![DOI](https://zenodo.org/badge/2273724.svg)](https://zenodo.org/badge/latestdoi/2273724)


`rgbif` gives you access to data from [GBIF][] via their REST API. GBIF versions their API - we are currently using `v1` of their API. You can no longer use their old API in this package - see `?rgbif-defunct`.

Tutorials:

* [rgbif vignette - the intro to the package](vignettes/rgbif_vignette.Rmd)
* [issues vignette - how to clean GBIF data](vignettes/issues_vignette.Rmd)
* [taxonomic names - examples of some confusing bits](vignettes/taxonomic_names.Rmd)

Check out the rgbif [paper][] for more information on this package and the sister [Python][pygbif] and [Ruby][gbifrb] clients.

## Package API

The `rgbif` package API follows the GBIF API, which has the following sections:

* `registry` (<https://www.gbif.org/developer/registry>) - Metadata on datasets, and
contributing organizations, installations, networks, and nodes
    * `rgbif` functions: `dataset_metrics()`, `dataset_search()`, `dataset_suggest()`,
    `datasets()`, `enumeration()`, `enumeration_country()`, `installations()`, `networks()`,
    `nodes()`, `organizations()`
    * Registry also includes the GBIF OAI-PMH service, which includes GBIF registry
    data only. `rgbif` functions: `gbif_oai_get_records()`, `gbif_oai_identify()`,
    `gbif_oai_list_identifiers()`, `gbif_oai_list_metadataformats()`,
    `gbif_oai_list_records()`, `gbif_oai_list_sets()`
* `species` (<https://www.gbif.org/developer/species>) - Species names and metadata
    * `rgbif` functions: `name_backbone()`, `name_lookup()`, `name_suggest()`, `name_usage()`
* `occurrences` (<https://www.gbif.org/developer/occurrence>) - Occurrences, both for
the search and download APIs
    * `rgbif` functions: `occ_count()`, `occ_data()`, `occ_download()`, `occ_download_cancel()`,
    `occ_download_cancel_staged()`, `occ_download_get()`, `occ_download_import()`,
    `occ_download_list()`, `occ_download_meta()`, `occ_get()`, `occ_issues()`,
    `occ_issues_lookup()`, `occ_metadata()`, `occ_search()`
* `maps` (<https://www.gbif.org/developer/maps>) - Map API
    * `rgbif` functions: `map_fetch()`
    * Note: we used to have a function `gbifmap()` that used `ggplot2` to plot data from the
    occurrence API, but it's been removed - see package [mapr][]

## Installation


```r
install.packages("rgbif")
```

Alternatively, install development version


```r
install.packages("devtools")
devtools::install_github("ropensci/rgbif")
```


```r
library("rgbif")
```

> Note: Windows users have to first install [Rtools](https://cran.r-project.org/bin/windows/Rtools/) to use devtools

Mac Users:
(in case of errors)

Terminal:

Install gdal : https://github.com/edzer/sfr/blob/master/README.md#macos


```r
brew install openssl
```
R terminal:

```r
install.packages('openssl')
install.packages('rgeos')
install.packages('rgbif')
```

## Search for occurrence data


```r
occ_search(scientificName = "Ursus americanus", limit = 50)
#> Records found [10467]
#> Records returned [50]
#> No. unique hierarchies [1]
#> No. media records [49]
#> No. facets [0]
#> Args [limit=50, offset=0, scientificName=Ursus americanus, fields=all]
#> # A tibble: 50 x 68
#>    name        key decimalLatitude decimalLongitude issues  datasetKey
#>    <chr>     <int>           <dbl>            <dbl> <chr>   <chr>
#>  1 Ursus …  1.84e9            49.4           -123.  cdroun… 50c9509d-22c7…
#>  2 Ursus …  1.81e9            37.7           -120.  cdroun… 50c9509d-22c7…
#>  3 Ursus …  1.80e9            30.0            -84.3 cdroun… 50c9509d-22c7…
#>  4 Ursus …  1.81e9            42.0           -124.  cdroun… 50c9509d-22c7…
#>  5 Ursus …  1.81e9            25.4           -101.  gass84  50c9509d-22c7…
#>  6 Ursus …  1.81e9            40.8            -81.7 cdroun… 50c9509d-22c7…
#>  7 Ursus …  1.80e9            29.3           -103.  cdroun… 50c9509d-22c7…
#>  8 Ursus …  1.81e9            34.4           -119.  gass84  50c9509d-22c7…
#>  9 Ursus …  1.84e9            44.9           -110.  cdroun… 50c9509d-22c7…
#> 10 Ursus …  1.84e9            34.0           -117.  gass84  50c9509d-22c7…
#> # ... with 40 more rows, and 62 more variables: publishingOrgKey <chr>,
#> #   publishingCountry <chr>, protocol <chr>, lastCrawled <chr>,
#> #   lastParsed <chr>, crawlId <int>, extensions <chr>,
#> #   basisOfRecord <chr>, taxonKey <int>, kingdomKey <int>,
#> #   phylumKey <int>, classKey <int>, orderKey <int>, familyKey <int>,
#> #   genusKey <int>, speciesKey <int>, scientificName <chr>, kingdom <chr>,
#> #   phylum <chr>, order <chr>, family <chr>, genus <chr>, species <chr>,
#> #   genericName <chr>, specificEpithet <chr>, taxonRank <chr>,
#> #   dateIdentified <chr>, year <int>, month <int>, day <int>,
#> #   eventDate <chr>, modified <chr>, lastInterpreted <chr>,
#> #   references <chr>, license <chr>, identifiers <chr>, facts <chr>,
#> #   relations <chr>, geodeticDatum <chr>, class <chr>, countryCode <chr>,
#> #   country <chr>, rightsHolder <chr>, identifier <chr>,
#> #   verbatimEventDate <chr>, datasetName <chr>, gbifID <chr>,
#> #   collectionCode <chr>, verbatimLocality <chr>, occurrenceID <chr>,
#> #   taxonID <chr>, catalogNumber <chr>, recordedBy <chr>,
#> #   http...unknown.org.occurrenceDetails <chr>, institutionCode <chr>,
#> #   rights <chr>, eventTime <chr>, occurrenceRemarks <chr>,
#> #   identificationID <chr>, infraspecificEpithet <chr>,
#> #   coordinateUncertaintyInMeters <dbl>, informationWithheld <chr>
```

Or you can get the taxon key first with `name_backbone()`. Here, we select to only return the occurrence data.


```r
key <- name_backbone(name='Helianthus annuus', kingdom='plants')$speciesKey
occ_search(taxonKey=key, limit=20)
#> Records found [40800]
#> Records returned [20]
#> No. unique hierarchies [1]
#> No. media records [15]
#> No. facets [0]
#> Args [limit=20, offset=0, taxonKey=9206251, fields=all]
#> # A tibble: 20 x 90
#>    name        key decimalLatitude decimalLongitude issues   datasetKey
#>    <chr>     <int>           <dbl>            <dbl> <chr>    <chr>
#>  1 Helian…  1.81e9            52.6             10.1 cdround… 6ac3f774-d9f…
#>  2 Helian…  1.84e9             0                0   cucdmis… d2470ef8-edf…
#>  3 Helian…  1.81e9            32.0           -102.  cdround… 50c9509d-22c…
#>  4 Helian…  1.84e9            33.9           -117.  cdround… 50c9509d-22c…
#>  5 Helian…  1.82e9            56.6             16.4 cdround… 38b4c89f-584…
#>  6 Helian…  1.84e9            34.1           -116.  gass84   50c9509d-22c…
#>  7 Helian…  1.81e9            25.7           -100.  cdround… 50c9509d-22c…
#>  8 Helian…  1.82e9            56.6             16.6 cdround… 38b4c89f-584…
#>  9 Helian…  1.81e9            25.6           -100.  cdround… 50c9509d-22c…
#> 10 Helian…  1.82e9            59.8             17.5 gass84,… 38b4c89f-584…
#> 11 Helian…  1.83e9            34.0           -117.  cdround… 50c9509d-22c…
#> 12 Helian…  1.83e9            58.6             16.2 gass84,… 38b4c89f-584…
#> 13 Helian…  1.81e9            25.7           -100.  cdround… 50c9509d-22c…
#> 14 Helian…  1.83e9            58.4             14.9 gass84,… 38b4c89f-584…
#> 15 Helian…  1.84e9            26.2            -98.3 cdround… 50c9509d-22c…
#> 16 Helian…  1.84e9            25.8           -100.  cdround… 50c9509d-22c…
#> 17 Helian…  1.84e9           -43.6            173.  cdround… 50c9509d-22c…
#> 18 Helian…  1.84e9            23.8           -107.  cdround… 50c9509d-22c…
#> 19 Helian…  1.84e9            23.9           -107.  cdround… 50c9509d-22c…
#> 20 Helian…  1.84e9            26.2            -98.3 cdround… 50c9509d-22c…
#> # ... with 84 more variables: publishingOrgKey <chr>,
#> #   publishingCountry <chr>, protocol <chr>, lastCrawled <chr>,
#> #   lastParsed <chr>, crawlId <int>, extensions <chr>,
#> #   basisOfRecord <chr>, taxonKey <int>, kingdomKey <int>,
#> #   phylumKey <int>, classKey <int>, orderKey <int>, familyKey <int>,
#> #   genusKey <int>, speciesKey <int>, scientificName <chr>, kingdom <chr>,
#> #   phylum <chr>, order <chr>, family <chr>, genus <chr>, species <chr>,
#> #   genericName <chr>, specificEpithet <chr>, taxonRank <chr>,
#> #   coordinateUncertaintyInMeters <dbl>, year <int>, month <int>,
#> #   day <int>, eventDate <chr>, lastInterpreted <chr>, license <chr>,
#> #   identifiers <chr>, facts <chr>, relations <chr>, geodeticDatum <chr>,
#> #   class <chr>, countryCode <chr>, country <chr>, recordedBy <chr>,
#> #   catalogNumber <chr>, institutionCode <chr>, locality <chr>,
#> #   gbifID <chr>, collectionCode <chr>, elevation <dbl>,
#> #   elevationAccuracy <dbl>, continent <chr>, stateProvince <chr>,
#> #   rightsHolder <chr>, recordNumber <chr>, identifier <chr>,
#> #   municipality <chr>, datasetName <chr>, language <chr>,
#> #   occurrenceID <chr>, type <chr>, ownerInstitutionCode <chr>,
#> #   occurrenceRemarks <chr>, dateIdentified <chr>, modified <chr>,
#> #   references <chr>, verbatimEventDate <chr>, verbatimLocality <chr>,
#> #   taxonID <chr>, http...unknown.org.occurrenceDetails <chr>,
#> #   rights <chr>, eventTime <chr>, identificationID <chr>,
#> #   individualCount <int>, county <chr>,
#> #   identificationVerificationStatus <chr>, occurrenceStatus <chr>,
#> #   vernacularName <chr>, taxonConceptID <chr>, informationWithheld <chr>,
#> #   endDayOfYear <chr>, startDayOfYear <chr>, datasetID <chr>,
#> #   accessRights <chr>, higherClassification <chr>, habitat <chr>,
#> #   identifiedBy <chr>
```

## Search for many species

Get the keys first with `name_backbone()`, then pass to `occ_search()`


```r
splist <- c('Accipiter erythronemius', 'Junco hyemalis', 'Aix sponsa')
keys <- sapply(splist, function(x) name_backbone(name=x)$speciesKey, USE.NAMES=FALSE)
occ_search(taxonKey=keys, limit=5, hasCoordinate=TRUE)
#> Occ. found [2480598 (16), 9362842 (3799701), 2498387 (1243118)]
#> Occ. returned [2480598 (5), 9362842 (5), 2498387 (5)]
#> No. unique hierarchies [2480598 (1), 9362842 (1), 2498387 (1)]
#> No. media records [2480598 (1), 9362842 (5), 2498387 (4)]
#> No. facets [2480598 (0), 9362842 (0), 2498387 (0)]
#> Args [hasCoordinate=TRUE, limit=5, offset=0,
#>      taxonKey=2480598,9362842,2498387, fields=all]
#> 3 requests; First 10 rows of data from 2480598
#>
#> # A tibble: 5 x 80
#>   name         key decimalLatitude decimalLongitude issues   datasetKey
#>   <chr>      <int>           <dbl>            <dbl> <chr>    <chr>
#> 1 Accipit…  1.00e9          -27.6             -58.7 bri,cud… ad43e954-dd7…
#> 2 Accipit…  1.00e9          -27.9             -59.1 bri,cud… ad43e954-dd7…
#> 3 Accipit…  6.86e8            5.27            -60.7 cdround  e635240a-3cb…
#> 4 Accipit…  1.00e9          -27.6             -58.7 bri,cud… ad43e954-dd7…
#> 5 Accipit…  1.00e9          -27.6             -58.7 bri,cud… ad43e954-dd7…
#> # ... with 74 more variables: publishingOrgKey <chr>,
#> #   publishingCountry <chr>, protocol <chr>, lastCrawled <chr>,
#> #   lastParsed <chr>, crawlId <int>, extensions <chr>,
#> #   basisOfRecord <chr>, taxonKey <int>, kingdomKey <int>,
#> #   phylumKey <int>, classKey <int>, orderKey <int>, familyKey <int>,
#> #   genusKey <int>, speciesKey <int>, scientificName <chr>, kingdom <chr>,
#> #   phylum <chr>, order <chr>, family <chr>, genus <chr>, species <chr>,
#> #   genericName <chr>, specificEpithet <chr>, taxonRank <chr>, year <int>,
#> #   month <int>, day <int>, eventDate <chr>, modified <chr>,
#> #   lastInterpreted <chr>, license <chr>, identifiers <chr>, facts <chr>,
#> #   relations <chr>, geodeticDatum <chr>, class <chr>, countryCode <chr>,
#> #   country <chr>, identifier <chr>, created <chr>, gbifID <chr>,
#> #   associatedSequences <chr>, occurrenceID <chr>, taxonID <chr>,
#> #   higherClassification <chr>, sex <chr>, establishmentMeans <chr>,
#> #   continent <chr>, references <chr>, institutionID <chr>,
#> #   dynamicProperties <chr>, fieldNumber <chr>, language <chr>,
#> #   type <chr>, preparations <chr>, occurrenceStatus <chr>,
#> #   catalogNumber <chr>, institutionCode <chr>, verbatimEventDate <chr>,
#> #   higherGeography <chr>, nomenclaturalCode <chr>, endDayOfYear <chr>,
#> #   georeferenceVerificationStatus <chr>, datasetName <chr>,
#> #   locality <chr>, collectionCode <chr>, verbatimLocality <chr>,
#> #   recordedBy <chr>, otherCatalogNumbers <chr>, startDayOfYear <chr>,
#> #   accessRights <chr>, collectionID <chr>
```

## Maps

We've removed `gbifmap()` which helped users plot data from functions `occ_search()`/`occ_data()` - instead we strongly recommend using our other package [mapr][].

As of `rgibf v1`, we have integration for GBIF's mapping API, which lets you get raster images of
occurrences of taxa of interest. For example:


```r
x <- map_fetch(search = "taxonKey", id = 3118771, year = 2010)
x
#> class       : RasterLayer
#> dimensions  : 512, 512, 262144  (nrow, ncol, ncell)
#> resolution  : 0.703125, 0.3515625  (x, y)
#> extent      : -180, 180, -90, 90  (xmin, xmax, ymin, ymax)
#> coord. ref. : +init=epsg:4326 +proj=longlat +datum=WGS84 +no_defs +ellps=WGS84 +towgs84=0,0,0
#> data source : in memory
#> names       : layer
#> values      : 0, 1  (min, max)
```


```r
library(raster)
plot(x, axes = FALSE, box = FALSE)
```

![map](tools/map.png)

## Meta

* Please [report any issues or bugs](https://github.com/ropensci/rgbif/issues).
* License: MIT
* Get citation information for `rgbif` in R doing `citation(package = 'rgbif')`
* Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

- - -

This package is part of a richer suite called [spocc - Species Occurrence Data](https://github.com/ropensci/spocc), along with several other packages, that provide access to occurrence records from multiple databases.

- - -

[mapr]: https://github.com/ropensci/mapr
[paper]: https://doi.org/10.7287/peerj.preprints.3304v1/
[GBIF]: https://www.gbif.org/
[pygbif]: https://github.com/sckott/pygbif
[gbifrb]: https://github.com/sckott/gbifrb
