---
output:
  pdf_document: default
  html_document: default
---
# Functional Programming {#rprog4}




## Ch. 8 Objectives

This chapter is designed around the following learning objectives for functional
programming in R. Upon completing this chapter, you should be able to:  

- Describe the basic tenets of functional programming
- Define functions and their basic attributes
- Create simple functions using named and optional arguments
- Define the meaning of *"vectorized operations"* in R
- Interpret and apply the `purrr::map` family of functions

## What is ***functional programming***?

*Functional programming* means just that: programming with functions. A basic 
philosophy of functional programming in R is to replace "for-loops" with functions.
While there isn't anything inherently wrong with for-loops, they can be difficult
to follow, especially when they are nested within one to another.
R is well-poised to support the elimination of "for-loops" because R is a 
*vectorized language*. To be vectorized means that you don't need a for-loop to 
execute this code:


```r
evens <- seq(from = 2, to = 20, by = 2)  
evens_squared <- evens^2
```

with code like this:


```r
evens_squared <- for( i in length(evens)){  
  evens_squared <- evens[i] * evens[i] 
}
```

The top code performs the `squared` operator across each entry in the 
vector, one after another.  The bottom set uses a for-loop to accomplish the
same task, albeit in a longer (and harder to follow) version.

The pipe operator `%>%` is an ally in this endeavor because it allows you to 
pass an object (like a vector or data frame) through series of functions in serial order. The pipe function is also easier to follow (with your mind) because 
it allows you to interpret the code as if you were reading instructions in a 
"how-to" manual: 
*"take this object, then do this, then do this, then do that"*. **Without** the `%>%`
operator, we are forced to nest functions together or write long and verbose
code that steps through treatments slowly (and somewhat painfully).  Looking at
the code below should make clear which set is easier to comprehend:


```r
#nested code
daily_show_2000 <- select(filter(rename(daily_show, 
                                          year = YEAR, 
                                          job = GoogleKnowlege_Occupation, 
                                          date = Show, 
                                          name = Raw_Guest_List), 
                                   year == 2000), 
                            job, date, name)
#piped code
daily_show_2000 <- daily_show %>%
  rename(year = YEAR, 
         job = GoogleKnowlege_Occupation, 
         date = Show, 
         name = Raw_Guest_List) %>%
  filter(year == 2000) %>%
  select(job, date, name)
```

In the nested example above, we "see" the `dplyr::select()` function first, 
yet the arguments to this function show up last (at the end of the nested code).
This separation between functions and their arguments only gets worse as the 
degree of nesting increases. The piped code, however, produces the same result
as the nested code but in a much more "readable" fashion. A similar analogy 
holds for the nesting of "for-loops" (which, when nested, are even 
harder to follow!).

## Writng Functions

> <span style="color: blue;"> "If you have to do something more than twice,
use a function to do it." - Programming Proverb </span>

Up to this point, we have relied entirely on functions sourced from base R or
from packages like `Tidyverse::`(and for good reason - they are very useful).  
Every programmer, however, at some point in their journey, discovers that the 
function they are desiring simply doesn't exist - at least not in the way that 
suits their vision. To that end, it's worth learning how to create your own 
functions.

The syntax for function generation is relatively straight forward:


```r
#example code; will not run

function_name <- function(argument1, argument2, ...) {
   # insert code to manipulate arguments here
   # maybe some code to check validity of input arguments
   # specify a return value, if desired
}
```

Each function has **arguments** as input, **body code** as the working parts, and
an **environment** where the function resides. These components can be
accessed by passing the function name as an argument to:  

- `formals()`, to see arguments
- `body()`, to see the function's code
  - or type just the function name, without `()`, into the console
  - *but note that many base R functions are coded in C++* and can be accessed 
  [on GitHub](https://github.com/wch/r-source){target="_blank"}. 
- `environment()`, to see where the function resides.
  - more on *R environments* below

### Example Function: my_mean
Let's create a simple function, named `my_mean`, to calculate the arithmetic mean 
for a numeric vector. This function takes only one argument, a vector `x`, for 
which we calculate the mean (`sum(x) / length(x)`), assign it to a
value `y`, and the return `y` as output. Note that all the *"action"* for `my_mean`
happens within the curly braces `{ }` that follow the function assignment. 


```r
my_mean <- function(x) {
     y <- sum(x) / length(x) 
     return(y)
   }

my_mean(1:5)
```

```
## [1] 3
```

While this function is simple and straightforward, it does have some limitations.
For example, what happens if we pass a character vector to `my_mean` or a numeric
vector that contains `NA` values?  In the former case, we will get an error
because our function uses the base R function `sum()`, which requires numeric, 
logical, or complex vectors as input (so our function inherits the properties
of that function).

If any `NA` values are present, however, we are less fortunate: only NA is 
returned.  This many not seem like a big deal but if your analysis depends on
calling `my_mean` in several locations (over and over), you might have a hard 
time debugging it...


```r
my_mean(c(1, 2, 3, 4, 5, NA))
```

```
## [1] NA
```

For this reason, functions often contain *optional arguments* that allow the user
to specify how to handle such errors.  The function `my_mean2` below contains
an optional argument, `na.rm = TRUE`, that defaults to remove `NA` values when 
present.  This function uses `if else` logic to handle the `na.rm = TRUE` 
argument, since this argument can only have one of two values.


```r
my_mean2 <- function(x, na.rm = TRUE) {
  if (na.rm == FALSE) {
    y <- sum(x) / length(x)
    return(y)
    }
  else {
    y <- sum(na.omit(x)) / length(na.omit(x))
    return(y)
    }
}
```
Now, when we pass a vector containing `NAs` to `my_mean2`, we get a numeric 
result.


```r
my_mean2(c(1:5,NA), na.rm = TRUE)
```

```
## [1] 3
```

A couple points worth noting about the functions above. First, take note that 
most *homemade* functions rely on other functions called within their body text, 
and so they inherit the properties of those functions.  
Second, you may have noticed that while 
the `body()` code in `my_mean2` assigns the mean of `x` to a new variable, `y`, 
this function-specific variable does not show up in the **"Global Environment"** 
pane after the function is executed. This is due to how *environments* are created in R: an *environment* essentially draws walls around objects.  

Indeed, objects that are defined within functions are part of the *evaluation*
*environment* within that function, even if the function returns the value of 
the object as output (*"what happens inside functions stays inside functions"*). 
A detailed discussion on R environments is beyond the scope of this book, 
look [here](https://www.r-bloggers.com/environments-in-r/){target="_blank"}
for an introduction to environments and 
[here](https://adv-r.hadley.nz/environments.html){target="_blank"}
for a more detailed tutorial.
The take-home point is that a homemade function can exist within the global 
environment and return values to the global environment, even if some of the objects 
created within that function don't show up in the global environment.

### Example Function: "import.w.name"
Oftentimes, you will have a list of files of the same type/structure
that you want to import and analyze in a single data frame. This exercise (and
the section that follows) will demonstrate how you can streamline that process
using functional programming. Let's create a function to *"import a file with* 
*its name appended".*

For this example, assume that your files represent data from a network of sensors, 
where the name/ID assigned to each sensor is **included in its file name** but 
*not in the file itself*. To give you an example of what we are working with,
let's use `list.files()` to look at the file names and paths (*note: you can find this* 
*.zip file on our Canvas site*). For this exercise,
we show a short list of 8 files (4 each from two sensors) but one could imagine
this list being hundreds of entries.*To Note: these are real data collected* 
*using sensors from*
*[this citizen-science network](http://www.purpleair.com/map){target="_blank"}*
*and published*
*by our research group*
*[here](https://doi.org/10.1016/j.atmosenv.2019.117067){target="_blank"} in 2019.*
The "PA" in each file name stands for 
[Purple Air](https://www2.purpleair.com/){target="_blank"}.


```r
list.files('./data/purpleair/', full.names=TRUE)
```

```
## [1] "./data/purpleair//PA019_20181022.csv"
## [2] "./data/purpleair//PA019_20181023.csv"
## [3] "./data/purpleair//PA019_20181024.csv"
## [4] "./data/purpleair//PA019_20181025.csv"
## [5] "./data/purpleair//PA020_20181022.csv"
## [6] "./data/purpleair//PA020_20181023.csv"
## [7] "./data/purpleair//PA020_20181024.csv"
## [8] "./data/purpleair//PA020_20181025.csv"
```

Thus, we wish to write a function that not only imports these .csv files into a data
frame but also extracts the part of the file name (i.e., PA019 and PA020) 
as one of the data columns (otherwise, when the data were combined we might not know what data was associated with a given sensor!).  In this function we will also 
include a step to clean up the newly created data frame with a call to 
`dplyr::select()` to retain only a few variables of interest.  Seeing that these
files are .csv, we can leverage `readr::read_csv`


```r
# create an object that tracks the file names and file paths
file_list <- list.files('./data/purpleair/', full.names=TRUE)

# function to import a .csv and include part of the filename as a data column
import.w.name <- function(pathname) {
  #create a tibble by importing the 'pathname' file
  df <- read_csv(pathname, col_names = TRUE)
  df <- df %>%
    # use stringr::str_extract & a regex to get sensor ID from file name
    # regex translation: "look for a /, then extract all letters and numbers that follow until _"
    mutate(sensor_ID = str_extract(pathname, 
                                  "(?<=//)[:alnum:]+(?=_)")) %>%
    # return only a few salient variables to the resultant data frame using dplyr::select
    select(UTCDateTime, 
           current_temp_f, 
           current_humidity, 
           pressure,
           pm2_5_atm,
           sensor_ID) %>%
    na.omit() # remove NA values, which happens when sensor goes offline
  return(df)
}
```

After sourcing this function, we can test it out on the first entry of our list
of files.  *We specify the first entry with a subset to* `file_list[1]`.


```r
PA_data_1 <- import.w.name(file_list[1])

head(PA_data_1)
```

```
## # A tibble: 6 × 6
##   UTCDateTime       current_temp_f current_humidity pressure pm2_5_atm sensor_ID
##   <chr>                      <dbl>            <dbl>    <dbl>     <dbl> <chr>    
## 1 2018/10/22T17:52…             90               12     852.      2.45 PA019    
## 2 2018/10/22T17:53…             86               13     852.      3.43 PA019    
## 3 2018/10/22T17:55…             87               13     852.      2.76 PA019    
## 4 2018/10/22T17:56…             87               13     852.      2    PA019    
## 5 2018/10/22T17:58…             87               13     852.      1.82 PA019    
## 6 2018/10/22T17:59…             87               13     852.      1.98 PA019
```

The `import.w.name()` function is useful, but not versatile; 
it was written to handle a special type of file with a particular naming 
convention. Those limitations aside, one could imagine how this function could 
be easily adapted to suit other file types and formats.  As you develop your
coding skills, a good strategy is to keep useful functions in a .R script
file so that you can call upon them when needed: `source(import.w.name.R)`

## The `purrr::` package
The `purrr::` package was designed specifically with functional programming in mind.
Similar to the discussion of *vectorized operations* above, `purrr::` was created
to help you apply functions to vectors in a way that is easy to implement and 
easy to "read".

### Function Mapping
The `map_` family of functions are the core of the `purrr` package. These 
functions are intended to *map* functions (i.e., to apply them) to individual elements in a vector (or data frames); the `map_` functions are similar to functions like `lapply()` and `vapply()` from base R (but more versatile). *"Mapping"* a function onto a vector is a common theme of functional programming. To illustrate how the `map_` functions work, its best to visualize the process first.

<div class="figure" style="text-align: center">
<img src="./images/map_anno1.png" alt="The map functions transform their input by applying a function to each element of a list or atomic vector and returning an object of the same length as the input." width="632" />
<p class="caption">(\#fig:map-anno1)The map functions transform their input by applying a function to each element of a list or atomic vector and returning an object of the same length as the input.</p>
</div>

As shown above in Figure \@ref(fig:map-anno1), the generic form of `map()`  always returns a `list` object as the output (thanks to Hadley Wickham for the [graphic](https://adv-r.hadley.nz/functionals.html#map){target="_blank"}). A  `list` is a generic vector that can contain multiple objects (other vectors, data frames, etc.). Indeed, a data frame (and a `tibble`) is a special kind of list: a data frame is a list of vectors (columns) of equal length. A data frame can contain different vector objects but those objects must be placed in separate columns and each column vector must be the same length (when all the columns are equal length that means that data frames are *rectangular* lists). A generic `list` does not need to be rectangular because generic list entries are more distinct. Further, a list can contain another list (like a set of small boxes that are nested within a larger box). Think of a `list` like a drawer organizer - it can contain lots of different objects, all of which might be related to another in some way or, at least, might be used together for some common purpose. To review:   

   - A ***vector*** is a one-dimensional object where all the entries are the same type.  
   - A ***matrix*** is a two-dimensional (rectangular) object where all entries are the same type, but ordered into *rows* and *column* indices.
   - A ***data frame*** is a two dimensional (rectangular) object that contains columns of different vectors. The type of vector (e.g., `character`, `numeric`, `logical`, etc.) can differ from one column to the next but all entries within each vector column must be the same.
   - A ***list*** can contain different vector objects of different types and different lengths. A list can also contain other lists.  A list does not need to be rectangular.
   
To illustrate the versatility of lists, let's create one that contains a numeric vector, a matrix, and a data frame.


```r
my.chr.vector <- c("Harry", "Ron", "Hermione", "Draco")

my.num.matrix <- matrix(data = 1:20, nrow=5)

my.df <- slice_sample(.data = mpg, n=7)

my.list <- list("entry_1" = my.chr.vector, 
                "entry_2" = my.num.matrix, 
                "entry_3" = my.df)

glimpse(my.list)
```

```
## List of 3
##  $ entry_1: chr [1:4] "Harry" "Ron" "Hermione" "Draco"
##  $ entry_2: int [1:5, 1:4] 1 2 3 4 5 6 7 8 9 10 ...
##  $ entry_3: tibble [7 × 11] (S3: tbl_df/tbl/data.frame)
##   ..$ manufacturer: chr [1:7] "volkswagen" "audi" "subaru" "volkswagen" ...
##   ..$ model       : chr [1:7] "jetta" "a4" "impreza awd" "gti" ...
##   ..$ displ       : num [1:7] 1.9 2.8 2.5 2 5.3 2 2
##   ..$ year        : int [1:7] 1999 1999 2008 2008 2008 2008 2008
##   ..$ cyl         : int [1:7] 4 6 4 4 8 4 4
##   ..$ trans       : chr [1:7] "manual(m5)" "auto(l5)" "manual(m5)" "auto(s6)" ...
##   ..$ drv         : chr [1:7] "f" "f" "4" "f" ...
##   ..$ cty         : int [1:7] 33 16 20 22 16 19 20
##   ..$ hwy         : int [1:7] 44 26 27 29 25 27 28
##   ..$ fl          : chr [1:7] "d" "p" "r" "p" ...
##   ..$ class       : chr [1:7] "compact" "compact" "compact" "compact" ...
```

Lists can be accessed in similar ways to vectors. For example, by using single-bracket indexing, `[ ]`, a list element is returned. 


```r
my.list[1]
```

```
## $entry_1
## [1] "Harry"    "Ron"      "Hermione" "Draco"
```

Note that *"single bracket"* indexing returns a "list element" of class: `list`.


```r
class(my.list[1])
```

```
## [1] "list"
```

If you want access to the vector contents of the list element, you can use the `$` symbol to access the contents of each list entry, or, you can use *"double bracket"* indexing, `[[ ]]`:


```r
my.list$entry_1
```

```
## [1] "Harry"    "Ron"      "Hermione" "Draco"
```

```r
class(my.list$entry_1)
```

```
## [1] "character"
```


```r
my.list[[2]]
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    6   11   16
## [2,]    2    7   12   17
## [3,]    3    8   13   18
## [4,]    4    9   14   19
## [5,]    5   10   15   20
```

```r
class(my.list[[2]])
```

```
## [1] "matrix" "array"
```

Working with lists is one of the mental hurdles to overcome when learning `map()`. However, there are variants in the `map_` family of functions that return simpler object types; we will work with these simplified functions to begin.  


|`Purrr::` Function |Object Type Returned                  |
|:------------------|:-------------------------------------|
|map()              |list                                  |
|map_lgl()          |logical                               |
|map_int()          |integer                               |
|map_dbl()          |numeric                               |
|map_chr()          |character                             |
|map_dfr()          |data frame (output by row-binding)    |
|map_dfc()          |data frame (output by column-binding) |

Let's use `map_dfr()` to apply our function, `import.w.name()`, onto the vector of file names and paths (`file_list`). The *"r"* in `map_dfr()` means that the resultant data frame output by `map_` will be created by binding rows together as the function moves down each index in the file list.  This is shown schematically in Figure \@ref(fig:map-dfr-anno) below.


```r
file_list <- list.files('./data/purpleair/', full.names=TRUE)

# map the import.w.name() function to all objects within `file_list` sequentially
# and combining the result into a single data frame with row binding
PA_data_merged <- map_dfr(file_list, import.w.name) 

glimpse(PA_data_merged)
```

```
## Rows: 6,374
## Columns: 6
## $ UTCDateTime      <chr> "2018/10/22T17:52:21z", "2018/10/22T17:53:41z", "2018…
## $ current_temp_f   <dbl> 90, 86, 87, 87, 87, 87, 86, 86, 86, 86, 86, 86, 86, 8…
## $ current_humidity <dbl> 12, 13, 13, 13, 13, 13, 12, 13, 13, 13, 13, 13, 13, 1…
## $ pressure         <dbl> 851.66, 851.68, 851.56, 851.59, 851.54, 851.57, 851.5…
## $ pm2_5_atm        <dbl> 2.45, 3.43, 2.76, 2.00, 1.82, 1.98, 1.95, 2.48, 2.27,…
## $ sensor_ID        <chr> "PA019", "PA019", "PA019", "PA019", "PA019", "PA019",…
```

<div class="figure" style="text-align: center">
<img src="./images/map_dfr_anno.png" alt="Example: using `map_dfr()` to import a file list using a custom function" width="632" />
<p class="caption">(\#fig:map-dfr-anno)Example: using `map_dfr()` to import a file list using a custom function</p>
</div>


## Homework 
This homework will give you practice at writing functions, mapping functions, and cleaning/plotting data. To begin, download the PurpleAir data files from Canvas. Note: the data are contained in a .zip file, which you can unzip on your computer or using R!

