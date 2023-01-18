How many hours have I spent looking how to solve a regex?

A lot!

So here I have some quick notes on things that I have solved before and
forget about it pretty often.

## How to extract numbers from a string

Sometimes, I need to extract just the numbers that I can find in a
string. To achieve this, I can use the following function:

    library(stringr)

    string_with_numbers <- c("01 uno", "02 dos")

    str_extract(string_with_numbers , "\\d+")

    ## [1] "01" "02"

## Extract string between brackets

    library(stringr)
    library(tibble)
    library(dplyr)
    library(tidyr)

    ## 
    ## Attaching package: 'tidyr'

    ## The following object is masked from 'package:terra':
    ## 
    ##     extract

    ## The data frames with the column that I need
    check <- tribble(
      ~geo,
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.94739867829001,44.3105986723403]}" , 
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.94714795170373,44.310596361431216]}",
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.9468972251475,44.31059404997191]}" , 
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.9466464986213,44.31059173796237]}" , 
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.94639577212517,44.3105894254026]}" , 
      "{\"geodesic\":false,\"type\":\"Point\",\"coordinates\":[-79.9461450456591,44.310587112292595]}" 
    )
     
    ## Solution 1 (Didn't work)
    check %>% 
      mutate(test = str_extract(geo, "\\[|\\]") ) %>% 
      select(test)

    ## # A tibble: 6 × 1
    ##   test 
    ##   <chr>
    ## 1 [    
    ## 2 [    
    ## 3 [    
    ## 4 [    
    ## 5 [    
    ## 6 [

    # Solution 2 (This one works!)
    check %>% 
      mutate(test = str_extract(geo, "\\[(.*?)\\]") ) %>% 
      select(test) %>% 
      separate(col = "test", into = c("lat", "long"), sep = ",") %>% 
      mutate(lat = str_extract(lat, "-?[0-9.]+"),
             long = str_extract(long, "-?[0-9.]+"))

    ## # A tibble: 6 × 2
    ##   lat                long              
    ##   <chr>              <chr>             
    ## 1 -79.94739867829001 44.3105986723403  
    ## 2 -79.94714795170373 44.310596361431216
    ## 3 -79.9468972251475  44.31059404997191 
    ## 4 -79.9466464986213  44.31059173796237 
    ## 5 -79.94639577212517 44.3105894254026  
    ## 6 -79.9461450456591  44.310587112292595
