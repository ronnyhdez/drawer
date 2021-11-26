## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

    summary(cars)

    ##      speed           dist       
    ##  Min.   : 4.0   Min.   :  2.00  
    ##  1st Qu.:12.0   1st Qu.: 26.00  
    ##  Median :15.0   Median : 36.00  
    ##  Mean   :15.4   Mean   : 42.98  
    ##  3rd Qu.:19.0   3rd Qu.: 56.00  
    ##  Max.   :25.0   Max.   :120.00

## Including Plots

You can also embed plots, for example:

![](check_path_files/figure-markdown_strict/pressure-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.

# Read data

    check <- readr::read_csv("data/Untitled 1.csv")

    ## Rows: 3 Columns: 3

    ## ── Column specification ───────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): A
    ## dbl (2): B, C

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    dplyr::glimpse(check)

    ## Rows: 3
    ## Columns: 3
    ## $ A <chr> "sd", "fd", "er"
    ## $ B <dbl> 34, 56, 87
    ## $ C <dbl> 34, 23, 43
