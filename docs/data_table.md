## Translating a dplyr pipeline to data table

The problem was that a project had local csv files with many rows. When
cleaning the data using dplyr this would consume a lot of the memory and
take a lot of time to produce a result.

So, an option was to implement the package `data.table` to be able to
handle all this data in a local computer.

This was the original data pipeline:

    pixels_clean <- pixels %>% 
      separate(file_id, into = c("folder", "file_id"), sep = "/") %>% 
      mutate(file_id = str_remove(file_id, ".csv")) %>% 
      mutate(location = str_extract(geo, "\\[(.*?)\\]") ) %>% 
      separate(col = "location", into = c("lat", "long"), sep = ",") %>% 
      mutate(lat = str_extract(lat, "-?[0-9.]+"),
             long = str_extract(long, "-?[0-9.]+")) %>% 
      select(-folder, -geo) %>% 
      separate(file_id, into = c("first", "second", "tnk"), remove = FALSE) %>% 
      select(-second, -tnk) %>% 
      separate(first, into = c("date", "time"), sep = "T") %>% 
      mutate(date = ymd(date))

The translation to data table:

    setDT(pixels_sr)
    pixels_sr[, c("folder_1", "folder_2", "file_id") := tstrsplit(file_id, "/", fixed = TRUE)]
    pixels_sr[ , ":="(file_id = str_remove(file_id, ".csv"))]
    pixels_sr[ , ":="(location = str_extract(geo, "\\[(.*?)\\]"))]
    pixels_sr[,  c("lat", "long") := tstrsplit(location, ",", fixed = TRUE)]
    pixels_sr[ , ":="(lat = str_extract(lat, "-?[0-9.]+"),
                      long = str_extract(long, "-?[0-9.]+"))]
    pixels_sr[, ":="(date = ymd(file_id))]
    pixels_sr[, c("file_id", "system_index", 
                  "folder_1", "folder_2",
                  "geo", "location") := NULL]

## Other tips:

If we have a column with more than ~40 groups, we should create a key:

    setkey(pft, columna_con_muchos_grupos) 

The sintaxis to summarise per groups will be:

    check_mean <- pft[ , .(mean_evi = mean(evi, na.rm = TRUE)), 
                       by = fecha]

Sintaxis for creating a new column with some column formulas:

    pft[ , ":="(evi = (2.5 *(b8 - b4)) /
                  (b8 + (2.4 * b4) + 1000))] 
