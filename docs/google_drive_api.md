A mockup documented function to connect with the google drive API
through R, check the files that exist in the drive folder, compare to
what you have in your local folder and download just those that you
don’t have locally.

Could be part of some workflow in an organization that uses google drive
as their site to keep their data files (like a research lab)

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

After the function is loaded in your Global environment, coupled with a
`map()` and if all the data files have the same variables, we can read
everything together in just one data frame:

    # Check drive and download data ------------------------------
    get_drive_data(drive_path = "data_workflows/data", 
                   local_directory = "datos")
    # Read data --------------------------------------------------
    # Create object with files of interest
    files <- dir_ls(path = "datos", glob = "datos/principe_*")

    principe <- files %>% 
      map_dfr(read_csv, .id = "file_id")
