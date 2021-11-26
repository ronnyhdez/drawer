# How to extract numbers from a string

Sometimes, I need to extract just the numbers that I can find in a
string. To achieve this, I can use the following function:

    library(stringr)

    string_with_numbers <- c("01 uno", "02 dos")

    str_extract(string_with_numbers , "\\d+")

    ## [1] "01" "02"
