---
title: "CH-3_exercise"
author: "John V"
date: "9/16/2020"
output: html_document
---



## Data Import

Read in the ozone dataset.  *This is a small dataset taken from the city of Fort Collins Ozone monitoring station*.


```r
raw_data <- read_csv("./data/ftc_o3.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   site_number = col_double(),
##   parameter_code = col_double(),
##   poc = col_double(),
##   latitude = col_double(),
##   longitude = col_double(),
##   date_local = col_date(format = ""),
##   time_local = col_time(format = ""),
##   date_gmt = col_date(format = ""),
##   time_gmt = col_time(format = ""),
##   sample_measurement = col_double(),
##   sample_duration_code = col_double(),
##   detection_limit = col_double(),
##   uncertainty = col_logical(),
##   date_of_last_change = col_date(format = ""),
##   cbsa_code = col_double(),
##   datetime = col_datetime(format = "")
## )
```

```
## See spec(...) for full column specifications.
```
