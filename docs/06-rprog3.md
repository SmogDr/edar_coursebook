# Dates, Strings, and Tidying {#rprog3}




```r
library("dplyr")
daily_show <- read_csv(file = "data/daily_show_guests.csv", skip = 4)
```

## Objectives

After this chapter, you should (know / understand / be able to ):

- Convert a column to a date format using `lubridate` functions
- Extract information from a date object (e.g., month, year, day of week) using `lubridate` functions
- Search, organize, and visualize data that are linked according to date objects
- Manipulate character strings using the `stringr` and `tidyr` packages of functions
- Parse strings using regular expressions (regex)
- Define the main characteristics of "Tidy Data"
- Apply the `tidyr` package of functions to create tidy data


## Dates
## Dates in R

As part of the data cleaning process, you may want to change the class of some
of the columns in the dataframe. For example, you may want to change a column
from a character to a date.

Here are some of the most common vector classes in R:

Class        | Example
------------ | -----------------------------------
`character`  | "Chemistry", "Physics", "Mathematics"
`numeric`    | 10, 20, 30, 40
`factor`     | Male [underlying number: 1], Female [2]
`Date`       | "2010-01-01" [underlying number: 14,610]
`logical`    | TRUE, FALSE

To find out the class of a vector (including a column in a dataframe -- remember
each column can be thought of as a vector), you can use `class()`:


```r
class(daily_show$date)
```

```
## Warning: Unknown or uninitialised column: `date`.
```

```
## [1] "NULL"
```

It is especially common to need to convert dates during the data cleaning
process, since date columns will usually be read into R as characters or
factors---you can do some interesting things with vectors that are in a Date
class that you cannot do with a vector in a character class.

To convert a vector to the `Date` class, if you'd like to only use base R, you
can use the `as.Date` function. I'll walk through how to use `as.Date`, since
it's often used in older R code. However, I recommend in your own code that you
instead use the `lubridate` package, which I'll talk about later in this
section.

To convert a vector to the `Date` class, you can use functions in the
`lubridate` package. This package has a series of functions based on the order
that date elements are given in the incoming character with date information.
For example, in "12/31/99", the date elements are given in the order of month
(**m**), day (**d**), year (**y**), so this character string could be converted
to the date class with the function `mdy`. As another example, the `ymd`
function from lubridate can be used to parse a column into a Date class,
regardless of the original format of the date, as long as the date elements are
in the order: year, month, day. For example:


```r
library("lubridate")
ymd("2008-10-13")
```

```
## [1] "2008-10-13"
```

```r
ymd("'08 Oct 13")
```

```
## [1] "2008-10-13"
```

```r
ymd("'08 Oct 13")
```

```
## [1] "2008-10-13"
```

To convert the `date` column in the `daily_show` data into a Date
class, then, you can run:


```r
library(package = "lubridate")

class(x = daily_show$date) # Check the class of the 'date' column before mutating it
```

```
## Warning: Unknown or uninitialised column: `date`.
```

```
## [1] "NULL"
```

```r
daily_show <- rename(.data = daily_show,
                     year = YEAR,
                     job = GoogleKnowlege_Occupation, 
                     date = Show, 
                     category = Group,
                     guest_name = Raw_Guest_List)

daily_show <- mutate(.data = daily_show,
                     date = mdy(date))
head(x = daily_show, n = 3)
```

```
## # A tibble: 3 x 5
##    year job                date       category guest_name     
##   <dbl> <chr>              <date>     <chr>    <chr>          
## 1  1999 actor              1999-01-11 Acting   Michael J. Fox 
## 2  1999 Comedian           1999-01-12 Comedy   Sandra Bernhard
## 3  1999 television actress 1999-01-13 Acting   Tracey Ullman
```

```r
class(x = daily_show$date) # Check the class of the 'date' column after mutating it
```

```
## [1] "Date"
```

Once you have an object in the `Date` class, you can do things like plot by
date, calculate the range of dates, and calculate the total number of days the
dataset covers:


```r
range(daily_show$date)
diff(x = range(daily_show$date))
```

We could have used these to transform the date in `daily_show`, using the following pipe chain: 


```r
daily_show <- read_csv(file = "data/daily_show_guests.csv",
                       skip = 4) %>%
  rename(job = GoogleKnowlege_Occupation, 
         date = Show,
         category = Group,
         guest_name = Raw_Guest_List) %>%
  select(-YEAR) %>%
  mutate(date = mdy(date)) %>%
  filter(category == "Science")
head(x = daily_show, n = 2)
```

```
## # A tibble: 2 x 4
##   job          date       category guest_name     
##   <chr>        <date>     <chr>    <chr>          
## 1 neurosurgeon 2003-04-28 Science  Dr Sanjay Gupta
## 2 scientist    2004-01-13 Science  Catherine Weitz
```

The `lubridate` package also includes functions to pull out certain elements of a date, including: 

- `wday`
- `mday`
- `yday`
- `month`
- `quarter`
- `year`

For example, we could use `wday` to create a new column with the weekday of each show: 


```r
mutate(.data = daily_show,
       show_day = wday(x = date, label = TRUE)) %>%
  select(date, show_day, guest_name) %>%
  slice(1:5)
```

```
## # A tibble: 5 x 3
##   date       show_day guest_name            
##   <date>     <ord>    <chr>                 
## 1 2003-04-28 Mon      Dr Sanjay Gupta       
## 2 2004-01-13 Tue      Catherine Weitz       
## 3 2004-06-15 Tue      Hassan Ibrahim        
## 4 2005-09-06 Tue      Dr. Marc Siegel       
## 5 2006-02-13 Mon      Astronaut Mike Mullane
```

<div class="rmdwarning">
<p>R functions tend to use the timezone of <strong>YOUR</strong> computer’s operating system by default, or UTC, or GMT. You need to be careful when working with dates and times to either specify the time zone or convince yourself the default behavior works for your application.</p>
</div>

## Strings

### String detect & match

### Regular Expressions

### String split & separate

### String extract & remove

## Tidy Data



