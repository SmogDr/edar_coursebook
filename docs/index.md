--- 
title: "Engineering Data Analysis in R"
author: "John Volckens and Kathleen E. Wendt"
date: "2020-07-21"
site: bookdown::bookdown_site
knit: "bookdown::render_book"
documentclass: book
bibliography: 
biblio-style: apalike
link-citations: yes
description: "This is an undergraduate technical elective course for mechanical engineers who wish to lean about data analysis using the R programming language."
---

```
## ── Attaching packages ──────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✓ ggplot2 3.3.0     ✓ purrr   0.3.3
## ✓ tibble  2.1.3     ✓ dplyr   0.8.3
## ✓ tidyr   1.0.2     ✓ stringr 1.4.0
## ✓ readr   1.3.1     ✓ forcats 0.4.0
```

```
## ── Conflicts ─────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```
## 
## Attaching package: 'kableExtra'
```

```
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

# Introduction {#intro}
 <font size="5"> Preface </font>

I developed this coursebook to help engineers begin to *think* about data. Most
of my job at the university is to conduct research, yet most of the students
who show up to my lab don't know where to begin when presented with data. The
irony is that, while engineering students are continuously drilled on how to
solve problems, they are rarely taught how to seek them out. My undergraduate
advisor, Dr. David Hemenway, once told me: "The data are always trying to tell
you something."  This book is an introduction to *data listening* because
listening comes before understanding.

In broad terms, scientific and engineering research is about discovery: finding
out something new. In practice, however, research is really about failure. Over
the short term, researchers fail in the act of discovery much more often than
they succeed. Such failure is to be expected because cutting-edge research
ain't easy. Once you come to terms with accepting failure as a regular
occurrence, you put yourself in a position to learn from it. To learn from
failure requires that you observe and diagnose "what went wrong" and to do
that, you need to listen to what the data are telling you. Let's begin.

## How to use this book

The coursebook is intended to be a sort of *self-help* guide for students who
want to learn R programming and the art of engineering data science. The book
is designed to get you started in the art, not master it. I'm not qualified to
teach mastery in the art of R or engineering data science, so look elsewhere
for that level of tutelage.

If you are new to these topics, you probably want to start at the beginning and
proceed through each chapter sequentially. Some sections or material might seem
boring or too easy. In that case, just skip to the end of the section and see
if you can complete the exercises and answer the questions.

Nearly all of the graphics and data presented in this book were created or
manipulated in R. In many places, however, I have hidden the code in order to
streamline the message. If you ever wonder "How did he do that?" you can
download any of the R Markdowns on [the GitHub repository](https://github.com/SmogDr/edar_coursebook/), where this coursebook
is hosted. 

## Prerequisites

This course is intended for upper-level undergraduates who have completed MECH
231 (Experimental Design).

## Grading

### Grading for MECH 481A6

Course grades will be determined by the following five components:

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:center;"> Assessment component </th>
   <th style="text-align:center;"> Percent of grade </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> Exams </td>
   <td style="text-align:center;"> 60 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Quizzes </td>
   <td style="text-align:center;"> 25 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Homework </td>
   <td style="text-align:center;"> 15 </td>
  </tr>
</tbody>
</table>

## Course set-up

Please download and install the latest version of R and RStudio. 

- R: https://cran.r-project.org 
- RStudio: https://rstudio.com/products/rstudio/download/#download

Students will also need to download and install git software and create a
GitHub account.

- Install git: https://git-scm.com/downloads 
- Create a GitHub account: https://github.com 

[Happy Git with R](https://happygitwithr.com/) by Dr. Jenny Bryan is a helpful
resource for installation and set-up. Also, a team associated with the Colorado
State University Statistics Department is actively developing a series of
one-credit modules for undergraduate students new to R. Their
[videos](https://csu-r.github.io/Module1/) on software installation and RStudio
orientation may be particularly helpful at this stage.

If you want to generate PDF output from R Markdown documents, you will also
need to install LaTex. I suggest taking the following approach, if you have
never installed LaTex on your personal computer. More installation guidance can
be found [here](https://bookdown.org/yihui/rmarkdown/installation.html). 


```r
# install R package
install.packages("tinytex")
# install LaTex "ingredients"
tinytex::install_tinytex()
```

## Coursebook

This coursebook will serve as the only required textbook for this course. I
regularly edit and add to this book, so content may change somewhat over the
semester. We typically cover about a chapter of the book every 1-2 weeks of the
course.

This coursebook includes: 

- Links to the slides presented in class for each topic
- In-course exercises, typically including links to the data used in the
exercise
- Homework assignments
- Appendix of [reference distributions](#dist)
- A list of vocabulary and concepts that should be mastered for each quiz

If you find any typos or bugs, or if you have any suggestions for how the book
can be improved, feel free to post it on the book's [GitHub Issues
page](https://github.com/SmogDr/edar_coursebook/issues).

This book was developed using Yihui Xie's [bookdown](https://bookdown.org)
framework. The book is built using code that combines R code, data, and text to
create a book for which R code and examples can be re-executed every time the
book is re-built, which helps identify bugs and broken code examples quickly.
The online book is hosted using GitHub's free [GitHub Pages](https://pages.github.com). All material for this book is
available and can be explored at the book's [GitHub repository](https://github.com/SmogDr/edar_coursebook).

### Other helpful books (not required)

The best book to supplement the coursebook and lectures for this course is [R for Data Science](http://r4ds.had.co.nz) by Garrett Grolemund and Hadley
Wickham. The entire book is freely available online through the same format of
the coursebook. You can also purchase a paper version of the book published by
O'Reilly for around $40. This book is an excellent and up-to-date reference by
some of the best R programmers in the world.

There are a number of other useful books available on general R programming,
including:

- [R for Dummies](https://colostate-primo.hosted.exlibrisgroup.com/primo-explore/fulldisplay?docid=01COLSU_ALMA51267598310003361&context=L&vid=01COLSU&lang=en_US&search_scope=Everything&adaptor=Local%20Search%20Engine&tab=default_tab&query=any,contains,r%20for%20dummies&sortby=rank&offset=0)
- [R Cookbook](https://colostate-primo.hosted.exlibrisgroup.com/primo-explore/fulldisplay?docid=01COLSU_ALMA21203304500003361&context=L&vid=01COLSU&lang=en_US&search_scope=Everything&adaptor=Local%20Search%20Engine&tab=default_tab&query=any,contains,r%20cookbook&sortby=rank&offset=0)
- [R Graphics Cookbook](http://www.amazon.com/R-Graphics-Cookbook-Winston-Chang/dp/1449316956/ref=sr_1_1?ie=UTF8&qid=1440997472&sr=8-1&keywords=r+graphics+cookbook)
- [Roger Peng's Leanpub books](https://leanpub.com/u/rdpeng)
- Various books on [bookdown.org](www.bookdown.org)

The R programming language is used extensively within certain fields, including
statistics and bioinformatics. If you are using R for a specific type of
analysis, you will be able to find many books with advice on using R for both
general and specific statistical analysis, including many available in print or
online through the CSU library.

## Acknowledgements

Most of the introductory material for this book were adapted from Professor [Brooke Anderson's R Programming Coursebook](https://geanders.github.io/RProgrammingForResearch/), to whom I owe
thanks not only for the materials but for the many helpful discussions. I would
also like to acknowledge [John Tukey](http://rsbm.royalsocietypublishing.org/content/49/537.full.pdf+html),
one of the pioneers of exploratory data analysis, and the creators of the [NIST Engineering Statistics Handbook](https://doi.org/10.18434/M32189), from which I
have drawn many techniques. 
