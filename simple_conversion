library(parallel) # install.packages(parallel) if needed

data_directory <- "your/directory/here"


crsp_files <- list.files(path = data_directory, pattern = "^(patterned text here).*\\.txt$", full.names = TRUE)
usar_files <- list.files(path = data_directory, pattern = "^(patterned text here).*\\.txt$", full.names = TRUE)
usrr_files <- list.files(path = data_directory, pattern = "^(patterned text here).*\\.txt$", full.names = TRUE)


a_colnames <- c("several", "columns", "names", "here")

b_colnames <- c("several", "columns", "names", "here")

c_colnames <- c("several", "columns", "names", "here")


process_file_set <- function(file, colnames) {
  data <- read.table(
    file = file,
    sep = "",        
    header = FALSE,  
    stringsAsFactors = FALSE,
    fill = TRUE,
    strip.white = TRUE
  )
  
  actual_columns <- ncol(data)
  expected_columns <- length(colnames)
  
  
  if (actual_columns < expected_columns) {
    
    data <- cbind(data, matrix(NA, nrow = nrow(data), ncol = expected_columns - actual_columns)) 
    # Above adds additional columns if new columns begin to appear mid set.
  } else if (actual_columns > expected_columns) {
    
    data <- data[, 1:expected_columns]
  }
  colnames(data) <- colnames
  return(data)
}


num_cores <- detectCores() - 1  # Leave one core free


cl <- makeCluster(num_cores) # Opens cluster for processing

# Parallel function
process_file_parallel <- function(files, colnames, output_file) {
  
  data_list <- parLapply(cl, files, process_file_set, colnames = colnames)
  
  
  combined_data <- do.call(rbind, data_list)
  
  
  write.csv(combined_data, output_file, row.names = FALSE)
}

process_file_parallel(a_files, a_colnames, "path directory/yourcsvanamehere.csv")


process_file_parallel(b_files, b_colnames, "path directory/yourcsvbnamehere.csv")


process_file_parallel(c_files, c_colnames, "path directory/yourcsvcnamehere.csv")


stopCluster(cl)
