R notes
================

# Include ifelse within a pipeline

In case we are working in a data pipeline, but in the middle we need to
include a condiciontal statement, we can do it in the following way:

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
library(janitor)
```


    Attaching package: 'janitor'

    The following objects are masked from 'package:stats':

        chisq.test, fisher.test

``` r
status <- "most liked"

iris %>% 
  clean_names() %>% 
  {if (status == "most liked") {
    filter(., species == "setosa")
  } else {
    filter(., species %in% c("versicolor", "virginica") )
  }} %>% 
  group_by(species) %>% 
  tally()
```

    # A tibble: 1 × 2
      species     n
      <fct>   <int>
    1 setosa     50
