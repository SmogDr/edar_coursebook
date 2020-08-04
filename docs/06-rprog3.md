# Strings, Dates, and Tidying {#rprog3}





## Objectives

This Chapter is designed around the following learning objectives. Upon completing this Chapter, you should be able to:  

- Define the meaning of "strings" and "date time" objects in R
- Manipulate character strings using the `stringr` and `tidyr` packages of functions
- Parse strings using regular expressions (regex)
- Describe how R stores the POSIXct date and time object internally
- Convert a column to a date format using functions from the `lubridate` package
- Extract information from a date object (e.g., month, year, day of week) using `lubridate` functions
- Search, organize, and visualize data that are linked to date objects
- Define the primary characteristics of *"Tidy Data"*
- Apply functions from the `dplyr` and `tidyr` packages to make data frames "tidy"

## Strings {#strings}
A ***string*** is a character variable like "John", or "blue", or "John has entered his blue phase". *Note: strings are defined in R using quotes `" "`* Strings often show up in data analysis in one of two ways:  

  1. As ***metadata***. Metadata means: "data that describe other data".  A *readme.txt* file is metadata; notes and code comments are metadata - all of these types of data usually come in the form of strings and are included *with the data your are analyzing* but not *in the data set* itself.  
  
  2. As ***vectorized data***.  In R programming, *"vectorized"* means: stored as a     column of data.  Examples of vectorized string variables you might find include things like: "participant names", or "survey responses to question 1", or "mode of failure".  The examples below show how strings are created in R. 
    

```r
# examples of vectorized string data
names_respond <- c("Ahmed", 
                   "Josh", 
                   "Mateo", 
                   "William", 
                   "Ali", 
                   "Wei", 
                   "Steve-O")
q1_responses <- c("because you told me to do it",
                  "it seemed like the right thing to do at the time",
                  "because I had been over-served",
                  "I don't know, I just did it",
                  "I got caught up in the heat of the moment",
                  "I was given an opportunity, so I took my shot",
                  "I plead the 5th")
failure_mode <- c("fracture",
                  "yielding", 
                  "deflection", 
                  "fatigue", 
                  "creep")
```
    
To analyze strings, you often begin by parsing each string. To parse means to examine the individual components. For example, when you read this sentence you parse out the words and then assign meaning to those words based on your memory, your understanding of grammar, and the contextual reality under which those words show up (i.e., whether you are reading an instruction manual, a text message, a novel, or a warrant for your arrest). Strings can be challenging to analyze because computers are built on logical operations and mathematics; strings are neither of those. Computers have fantastic memory, are OK at grammar, and are comically poor at contextualization. Taken together, this means that strings can be challenging (but not impossible) to analyze using computers. 

<div class="rmdnote">
<p>Are you active on social media platforms like Instagram or Twitter? You can bet that a computer program has downloaded and parsed all of your posts, each one as a string. You can learn a lot about a person (and their buying habits) from what they post online!</p>
</div>

In this chapter, we will introduce a few simple string functions from `{base}` R and the `stringr` package. We will also introduce the concept of **regular expressions** as a means to perform more advanced string manipulation.

### String detect & match
One of the simplest string operations is to search whether a string contains a pattern of interest. The `stringr` package (part of the [Tidyverse](https://stringr.tidyverse.org/)) was developed to make it easier for you to work with strings. Most of the functions in `stringr` begin with `str_` and end with a specific function name. A full list of functions is provided [here](https://stringr.tidyverse.org/reference/index.html). Some examples:  

  **`str_detect`** returns a vector of logical values (TRUE/FALSE) indicating whether the pattern was detected within each string searched. The function takes two arguments, the `string` to be searched and the `pattern` to search for.  If we search for the pattern "Josh" in our list of participants (`names_respond`), we get:
  

```r
str_detect(names_respond, "Josh")
```

```
## [1] FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
```
  
  **`str_match`** takes the same arguments but returns a vector of the matched values (by string index).
  

```r
str_match(names_respond, "Josh")
```

```
##      [,1]  
## [1,] NA    
## [2,] "Josh"
## [3,] NA    
## [4,] NA    
## [5,] NA    
## [6,] NA    
## [7,] NA
```

  **`str_subset`** returns only the entries from the vector where a match occurred.  If we subset our short list of names to the pattern of letters "li", we get:
  

```r
str_subset(names_respond, "li")
```

```
## [1] "William" "Ali"
```
  
In `{base}` R, these searches are performed with the `grep` family of functions. The term *"grep"* is an acronym for **<u>G</u>lobal <u>R</u>egular <u>E</u>xpression <u>P</u>attern** (more on *regular expressions* below).  Many of "old-school" coders use this family of functions (meaning: you will encounter them in the wild), so it's worth discussing them briefly.

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> stringr_funcs </th>
   <th style="text-align:left;"> base_funcs </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> str_detect(x, pattern) </td>
   <td style="text-align:left;"> grepl(pattern, x) </td>
  </tr>
  <tr>
   <td style="text-align:left;"> str_match(x, pattern) </td>
   <td style="text-align:left;"> regexec(pattern, x) + regmatches() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> str_subset(x, pattern) </td>
   <td style="text-align:left;"> grep(pattern, x, value = TRUE) </td>
  </tr>
</tbody>
</table>
### Regular Expressions
Before going much farther, we should spend some time discussing ***regular expressions*** or *regex* for short. When we pass a `pattern` argument to a function like `str_detect()`, the function treats that argument like a "regex".

<div class="rmdnote">
<p>A regular expression is a sequence of characters that define a search pattern to be implemented on a string.</p>
</div>

Regex sequences allow for pattern searching with logical and conditional relations, for example, the following "text search patterns" can be coded as regex:  

  * "any letter followed by the numbers 3, 4, or 5" ...  [:alpha:][345]
* "strings beginning with the letters 'ID' and followed by four numbers" ... ^ID[:digit:]{4}

In the R programming language, regular expressions follow the POSIX 1003.2 standard (regex can have different syntax based on the underlying standard).

**Regex** sequences have seemingly no end of sophistication and complexity; you could spend dozens of hours learning to use them and hundreds more learning to master them.  We will only introduce basic concepts here.  More in-depth study of regex syntax and usage can be found on [H. Wickham's R course](http://r4ds.had.co.nz/strings.html), on the `stringr` [cheatsheet](https://github.com/rstudio/cheatsheets/raw/master/strings.pdf) developed by RStudio, and 

<table>
 <thead>
  <tr>
   <th style="text-align:center;"> Regex syntax </th>
   <th style="text-align:center;"> String match </th>
   <th style="text-align:center;"> Example in R </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> \\d </td>
   <td style="text-align:center;"> Any digit </td>
   <td style="text-align:center;"> &quot;\\\\d&quot; </td>
  </tr>
  <tr>
   <td style="text-align:center;"> [abc] </td>
   <td style="text-align:center;"> matches a, b, or c </td>
   <td style="text-align:center;"> &quot;[abc]&quot; </td>
  </tr>
  <tr>
   <td style="text-align:center;"> [a-z] </td>
   <td style="text-align:center;"> matches every character between a and z </td>
   <td style="text-align:center;"> &quot;[a-z]&quot; </td>
  </tr>
  <tr>
   <td style="text-align:center;"> [^abc] </td>
   <td style="text-align:center;"> matches anything except a, b, or c </td>
   <td style="text-align:center;"> &quot;[^abc]&quot; </td>
  </tr>
</tbody>
</table>


### String extract & remove

### String split & separate



## Dates and Date-times

To begin, we discuss how `{base}` R code  handles dates and times, since there is a ton of code out there that utilizes these older functions. We will then quickly transition to the `lubridate` family of functions (part of the Tidyverse) because of their versatility and ease-of-use.  

### Dates and Times in base R

Dates and times in base R all proceed from an *"epoch"* or *time origin*.  In R, the *epoch* or "dawn of time" occurred at midnight on January 1^st^, 1970.  For the sake of the R programming world, the concept of time started at that precise moment and has moved forward ever since.  To note: R can handle date-times before 1/1/1970; it just treats them as negative values!  

To see a date-time object, you can tell R to give you the current "System Time" by calling the `Sys.time()` function.


```r
Sys.time()
```

```
## [1] "2020-08-03 17:43:24 MDT"
```
As you can see, we got back the date, time, and current timezone used by my computer.  If you want to see how this time is stored in R internally, you can use `unclass()`, which returns an object value with its class attributes removed.  When we wrap `unclass()` around `Sys.time()`, we will see the number of seconds that have occurred between the epoch of 1/1/1970 and right now:


```r
unclass(Sys.time())
```

```
## [1] 1596498205
```

That's a lot of seconds.  How many years is that?  
Just divide that number by [60s/min $\cdot$ 60min/hr $\cdot$ 24hr/d $\cdot$ 365d/yr] ~ 50.624626 years.  
This calculation ignores leap years but you get the point...

### Date-time formats
Note that the `Sys.time()` function provided the date in a ***"year-month-day"*** format and the time in an ***"hour-minute-second"*** format: 2020-08-03 17:43:24

Not everyone uses this exact ordering when they record dates and times, which is one of the reasons working with dates and times can be tricky.  You probably have little difficulty recognizing the following date-time objects as equivalent but not-so-much for some computer programs:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>(\#tab:date-times-1)Date-time objects come in different forms</caption>
<tbody>
  <tr>
   <td style="text-align:left;"> 12/1/99 8:46 PM </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1-Dec-1999 20: UTC </td>
  </tr>
  <tr>
   <td style="text-align:left;"> December 1st, 1999, 20:46:00 </td>
  </tr>
</tbody>
</table>

<div class="rmdnote">
<p>You will often see time referenced with a <strong>“UTC”</strong>, which stands for <em>“Universal Time, Coordinated”</em>. UTC is preferred by programmers because it doesn’t have a timezone and it doesn’t follow <em>Daylight Savings Time</em> conventions (daylight savings is the bane of many coders).</p>
<p>In practice, UTC is the same time as GMT (Greenwich Mean Time, pronounced “gren-itch”) but with an important distinction. GMT is one of the many <a href="https://wikipedia.org/wiki/List_of_tz_database_time_zones">time-zones</a> laid out across Earth’s longitude, whereas, <strong>UTC has no time zone</strong>.</p>
</div>

### Date-time classes in R

R has several classes of date-time objects, none of which are easy to remember:  

1. **`POSIXct`** - stored as the time, in seconds, between the `epoch` of 1970-01-01 00:00:00 UTC and the date-time object in question.  
    * the 'ct' stands for *"continuous time"* to represent "continuous seconds from origin";
    * A `POSIXct` object is a single numeric vector (and so provides for efficient computing).
2. **`POSIXlt`** - stored as a list of date-time objects.  
    * the 'lt' stands for *"list time"*.
    * A `POSIXlt` list contains the following elements:
      * *sec* as 0–61 seconds
      * *min* as 0–59 minutes
      * *hour* as 0–23 hours
      * *mday* as 1–31 day of the month
      * *mon* as 0–11 months after the first of the year
      * *year* as Years since 1900
      * *wday* as 0–6 day of the week, starting on Sunday
      * *yday* as 0–365 days of the year.
      * *isdst* as a flag for Daylight savings time.  Positive if in force, zero if not, negative if unknown.
3. **`POSIXt`** - this is a virtual class.  `POSIXt` (without the "l") is an internal way for R to convert between `POSIXct` and `POSIXlt` date-time objects.  
    * Think of the `POSIXt` as a way for R to perform operations/conversions between a `POSIXct` and `POSIXlt` object without throwing an error your way.

As a reminder, here are some of the most common **vector classes** in R:

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Class </th>
   <th style="text-align:left;"> Example </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> `character` </td>
   <td style="text-align:left;"> "Chemistry", "Physics", "Mathematics" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `numeric` </td>
   <td style="text-align:left;"> 10, 20, 30, 40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `factor` </td>
   <td style="text-align:left;"> Male [underlying number: 1], Female [2] </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `Date` </td>
   <td style="text-align:left;"> "2010-01-01" [underlying number: 14,610] </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `logical` </td>
   <td style="text-align:left;"> TRUE, FALSE </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `date-time` </td>
   <td style="text-align:left;"> "2020-06-23 11:05:20 MDT" </td>
  </tr>
</tbody>
</table>

To discover the class of a vector (including a column in a dataframe -- remember
each column can be thought of as a vector), you can use `class()`:


```r
class(Sys.time())
```

```
## [1] "POSIXct" "POSIXt"
```

Both the `POSIXct` and `POSIXlt` class of objects return the same value to the user; the difference is really in how these classes store date-time objects internally.  To examine them, you can coerce `Sys.time()` into each of the two classes using `as.POSIXct` and `as.POSIXlt` functions and then examine their attributes.


```r
time_now_ct <- as.POSIXct(Sys.time())
unclass(time_now_ct)
```

```
## [1] 1596498205
```


```r
time_now_lt <- as.POSIXlt(Sys.time())
str(unclass(time_now_lt)) # the `str()` function makes the output more compact
```

```
## List of 11
##  $ sec   : num 25
##  $ min   : int 43
##  $ hour  : int 17
##  $ mday  : int 3
##  $ mon   : int 7
##  $ year  : int 120
##  $ wday  : int 1
##  $ yday  : int 215
##  $ isdst : int 1
##  $ zone  : chr "MDT"
##  $ gmtoff: int -21600
##  - attr(*, "tzone")= chr [1:3] "" "MST" "MDT"
```
It's easy to see why the `POSIXct` object class is more computationally efficient; but it's also nice to see all the date-time information packed into the `POSIXlt`.  This is why R keeps a key to unlock both using `POSIXt`.  As my father used to say: clear as mud?

### Reading and classifying date-times
Oftentimes, when data is read into R, there are column elements that contain date and time information.  These dates and times are often interpreted by R as *character* vectors, which means they have lost their relational attributes (i.e., you cannot subtract "Monday 08:00" from "Wednesday 12:00" and get "2 days 4 hours"). If we want to analyze dates and times in a relational way, we need to instruct R to recognize these as date-time objects (i.e., as either the `POSIXct` or `POSIXlt` class). Thus, to convert a character vector into date or date-time object requires a change of that vector's ***class***.

Date-time elements can be tricky to work with for a few reasons:  

1. Different programs store and handle dates and times in different ways  
2. The existence of time zones means that date-time values can change with location
3. Date-time strings can be separated with spaces, colons, commas, slashes, dashes, or a mix of all those together (see Table \@ref(tab:date-times-1))

The `{base}` R function to convert between `character` classes and `date-time` classes is the function `strptime()`, which is short for *"**str**ing **p**arse into date-**time**"*. I mention this function not because I encourage you to use it but because I want you to be able to recognize it.  The function has over 39 conversion specifications that it can take as arguments.  That is to say, this function not simple to master.  If you are a glutton for punishment, I invite you to read the R Documentation `?strptime`.

In **summary** here are a few `{base}` R functions on date-time object that are worth knowing:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>(\#tab:base-r-times)Basic Date-time functions</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> {base} R Function </th>
   <th style="text-align:left;"> Value Returned </th>
   <th style="text-align:left;"> Example </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Sys.Date() </td>
   <td style="text-align:left;"> Current system date </td>
   <td style="text-align:left;"> "2020-06-23" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sys.time() </td>
   <td style="text-align:left;"> Current system date-Time </td>
   <td style="text-align:left;"> "2020-06-23 11:05:20 MDT" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sys.timezone() </td>
   <td style="text-align:left;"> Current system timezone </td>
   <td style="text-align:left;"> "America/Denver" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> as.POSIXct() </td>
   <td style="text-align:left;"> date-time object of class POSIXct </td>
   <td style="text-align:left;"> "2020-06-23 11:05:20 MDT" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> as.POSIXlt() </td>
   <td style="text-align:left;"> date-time object of class POSIXlt </td>
   <td style="text-align:left;"> "2020-06-23 11:05:20 MDT" </td>
  </tr>
  <tr>
   <td style="text-align:left;"> strptime() </td>
   <td style="text-align:left;"> date-time object of class POSIXlt </td>
   <td style="text-align:left;"> "2020-06-23 11:05:20 MDT" </td>
  </tr>
</tbody>
</table>

## `Lubridate` {#lubridate}
The `lubridate` package was developed specifically to make life easier when working with date-time objects. 

### `Lubridate` Parsing Functions
One of best aspects of `lubridate` is its ability to parse date-time objects with simplicity and ease; the `lubridate` parsing functions are designed as "named-to-order". Let me explain:  

> <span style="color: purple;"> **Parse**: ***to break apart and analyze the individual components*** (of something, like a character string) </span>

* If a character vector is written in "**y**ear-**m**onth-**d**ay" format (i.e., `"2020-Dec-18"`), then the `lubridate` function to convert that vector is `ymd()`.

* If a character vector is written in "**d**ay-**m**onth-**y**ear" format (i.e., `"18-Dec-2020"`), then the `lubridate` function to convert that vector is `dmy()`.  Try it out:


```r
#create a character vector
date_old <- "2020-Dec-18"

#prove to yourself it's a character class
class(date_old)
```

```
## [1] "character"
```

```r
#convert it to a `Date` class with `ymd()`
date_new <- ymd(date_old)

#prove to yourself it worked
class(date_new)
```

```
## [1] "Date"
```

That little conversion exercise may not have blown you away, but watch what happens when I feed the following set of wacky character vectors into that same `lubridate` parsing function, `ymd()`:


```r
messy_dates <- c("2020------Dec the 12",
                 "20.-.12.-.12",
                 "2020aaa12aaa12",
                 "20,12,12",
                 "2020x12-12",
                 "2020   ....    12        ......     12",
                 "'20.December-12")

ymd(messy_dates)
```

```
## [1] "2020-12-12" "2020-12-12" "2020-12-12" "2020-12-12" "2020-12-12"
## [6] "2020-12-12" "2020-12-12"
```

That's right.  The `ymd()` parsing function figured them all out correctly with almost no effort on your part.  But wait, there's more!  The `lubridate` package contains parsing functions for almost any order you can dream up.  

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Parsing Function </th>
   <th style="text-align:left;"> Format to Convert </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> `ymd()` </td>
   <td style="text-align:left;"> year-month-day </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `mdy()` </td>
   <td style="text-align:left;"> month-day-year </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `dmy()` </td>
   <td style="text-align:left;"> day-month-year </td>
  </tr>
</tbody>
</table>

And if you need to parse a time component, simply add a combination of `_hms` to the function call to parse time in "hours-minutes-seconds" format.  Some additional examples of how you would parse time that followed from a `ymd` format:

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Parsing Function </th>
   <th style="text-align:left;"> Format to Convert </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> `ymd_h()` </td>
   <td style="text-align:left;"> year-month-day_hours </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `mdy_hm()` </td>
   <td style="text-align:left;"> year-month-day_hours-minutes </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `dmy_hms()` </td>
   <td style="text-align:left;"> year-month-day_hours-minutes-seconds </td>
  </tr>
</tbody>
</table>
The beauty of the `lubridate` parsers is that they do the hard work of cleaning up the character vector, regardless of separators or delimiters within each string, and return either a `Date` or `Date-time` object class.

### Date-time manipulation with `Lubridate`

To convert the `date` column in the `daily_show` data into a Date
class, then, you can run:


```r
library(package = "lubridate")

class(x = daily_show$date) # Check the class of the 'date' column before mutating it
```

```
## [1] "character"
```

```r
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
# report the min and max dates
range(daily_show$date)

# calculate the duration from first to last date
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

- `wday` return the day of the week pertaining to a Date object
- `mday` return the day of the month pertaining to a Date object
- `yday` return the day of the year pertaining to a Date object
- `month` return the month pertaining to a Date object
- `quarter` return the quarter of hte year pertaining to a Date object
- `year` return the year pertaining to a date object

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


## Tidy Data



