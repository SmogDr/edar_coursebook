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
##   latitude = col_double(),
##   longitude = col_double(),
##   parameter = col_character(),
##   sample_measurement = col_double(),
##   units_of_measure = col_character(),
##   sample_duration = col_character(),
##   datetime = col_datetime(format = "")
## )
```
