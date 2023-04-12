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

# Build package from the terminal

clean and install package from the command line:

``` bash
R CMD REMOVE docmaker
R CMD INSTALL docmaker .
```

# Code functions challenges

Quick notes about short R code and functions things that usually I don’t
do and I tend to forget.

## Change specific values in all columns for NA’s

A data frame with `-9999` values in many columns that need to be replace
with `NA`’s values:

``` r
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
  mutate_all(~replace(., . == -9999, NA))
```

    # A tibble: 4 × 3
      a         b     c
      <chr> <dbl> <dbl>
    1 a         2    NA
    2 b         3     5
    3 c        NA     6
    4 d        NA    NA

## A function to play with google drive

A mock-up documented function to connect with the google drive API
through R, check the files that exist in the drive folder, compare to
what you have in your local folder and download just those that you
don’t have locally.

Could be part of some workflow in an organization that uses google drive
as their site to keep their data files (like a research lab)

``` r
#' @import dplyr
NULL

#' @title Download data from google drive
#' 
#' @author Ronny Alexander Hernández Mora
#' 
#' @description This function will check in a local folder, the existing files
#' and compare which ones are missing from an specific folder in google drive.
#' It will download the missing files
#' 
#' @param drive_path The folder in google drive containing the files
#' @param local_directory The local folder in which we want to download the 
#' files from google drive.
#' 
#' @example 
#' \dontrun{
#' get_drive_data(drive_path = "data_workflows/data", 
#'                local_directory = "datos")
#'}
#'
get_drive_data <- function(drive_path, local_directory) {
  
  options(gargle_oauth_email = "my_email@gmail.com")
  
  # Revisar archivos locales
  archivos_existentes <- fs::dir_ls(local_directory) %>% 
    stringr::str_remove(paste0(local_directory, "/"))
  
  # Revisar archivos en drive
  camino <- drive_path
  
  # Check data available
  archivos_drive <- googledrive::drive_ls(path = camino)
  
  # archivos_drive <- archivos %>% 
  #   select(name)
  
  # Obtener nombres de archivos faltantes
  # Suponiendo que siempre tenemos mas archivos en drive
  archivos_faltantes <- archivos_drive %>% 
    filter(!name %in% archivos_existentes)
  
  # Loop para traerse archivos que estan en drive pero no locales
  for (i in archivos_faltantes$name) {
    archivos_faltantes %>%
      select(id) %>%
      slice(1) %>%
      pull() %>%
      googledrive::drive_download(
        path = paste0("datos/", i), overwrite = TRUE
      )
  }
}
```

After the function is loaded in your Global environment, coupled with a
`map()` and if all the data files have the same variables, we can read
everything together in just one data frame:

``` r
# Check drive and download data ------------------------------
get_drive_data(drive_path = "data_workflows/data", 
               local_directory = "datos")
# Read data --------------------------------------------------
# Create object with files of interest
files <- dir_ls(path = "datos", glob = "datos/principe_*")

principe <- files %>% 
  map_dfr(read_csv, .id = "file_id")
```

## Create questions with `yesno` package

If we have functions that require the confirmation of the user, we can
use the `yesno` package to create questions and the answer options:

``` r
library(yesno)
publicar <- function(){
  respuesta <- yesno::yesno("¿Desea publicar las notas?",
                            yes = "Estoy seguro de publicarlas",
                            no = "No, es un error",
                            no = "NOOO, yo no quiero publicar nada")

  if (respuesta == TRUE) {
    print("Los correos han sido enviados")
  } else {
    print("No se envio nada")
  }
}

#publicar()
```

# `stringr` tips

How many hours have I spent looking how to solve a regex?

A lot!

So here I have some quick notes on things that I have solved before and
forget about it pretty often.

## How to extract numbers from a string

Sometimes, I need to extract just the numbers that I can find in a
string. To achieve this, I can use the following function:

``` r
library(stringr)

string_with_numbers <- c("01 uno", "02 dos")

str_extract(string_with_numbers , "\\d+")
```

    [1] "01" "02"

## Extract string between brackets

``` r
library(stringr)
library(tibble)
library(dplyr)
library(tidyr)

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
```

    # A tibble: 6 × 1
      test 
      <chr>
    1 [    
    2 [    
    3 [    
    4 [    
    5 [    
    6 [    

``` r
# Solution 2 (This one works!)
check %>% 
  mutate(test = str_extract(geo, "\\[(.*?)\\]") ) %>% 
  select(test) %>% 
  separate(col = "test", into = c("lat", "long"), sep = ",") %>% 
  mutate(lat = str_extract(lat, "-?[0-9.]+"),
         long = str_extract(long, "-?[0-9.]+"))
```

    # A tibble: 6 × 2
      lat                long              
      <chr>              <chr>             
    1 -79.94739867829001 44.3105986723403  
    2 -79.94714795170373 44.310596361431216
    3 -79.9468972251475  44.31059404997191 
    4 -79.9466464986213  44.31059173796237 
    5 -79.94639577212517 44.3105894254026  
    6 -79.9461450456591  44.310587112292595
