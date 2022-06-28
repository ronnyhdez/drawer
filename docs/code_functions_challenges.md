Quick notes about short R code and functions things that usually I don’t
do and I tend to forget.

## Change specific values in all columns for NA’s

A data frame with `-9999` values in many columns that need to be replace
with `NA`’s values:

    # libraries
    library(dplyr)

    # Test data frame
    test <- tribble(~a, ~b, ~c,
                    "a", 2, -9999,
                    "b", 3, 5,
                    "c", -9999, 6,
                    "d", -9999, -9999)

    # Solution
    test %>% 
      mutate_all(~na_if(., -9999))

    ## # A tibble: 4 × 3
    ##   a         b     c
    ##   <chr> <dbl> <dbl>
    ## 1 a         2    NA
    ## 2 b         3     5
    ## 3 c        NA     6
    ## 4 d        NA    NA
