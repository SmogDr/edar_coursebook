--- 
title: "Engineering Data Analysis in R"
author: "John Volckens"
date: "2024-11-08"
site: bookdown::bookdown_site
knit: "bookdown::render_book"
documentclass: book
bibliography: packages.bib
biblio-style: apalike
link-citations: yes
description: "This is an undergraduate technical elective course for mechanical engineers who wish to lean about data analysis using the R programming language."
---


```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
## 
## Attaching package: 'kableExtra'
## 
## 
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

### Grading for MECH 476

Course grades will be determined by the following three components:

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
   <td style="text-align:center;"> 15 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Assignments </td>
   <td style="text-align:center;"> 25 </td>
  </tr>
</tbody>
</table>

### Homework submission

There will be a homework assignment per coursebook chapter. Each homework will
be due at the start of the class period when lecture coverage of the *next*
chapter commences. For example, Chapter 2 Homework will be due at the start 
of the first lecture regarding Chapter 3.

From RStudio, you will  upload ("commit"/"push") your homework to a private
folder on our class GitHub Organization. Your commits will be time-stamped; any
commit after the start of class on a given day will be considered late, and the
assignment will be graded (or not graded) accordingly. Guidance on installing
and connecting R/RStudio and Git/GitHub is provided in the next section.

## Course set-up

This might be painful, but bear with me. There will be a lot of software
development [jargon](https://docs.github.com/en/github/getting-started-with-github/github-glossary) (e.g., "commit", "push", "pull"), but the general idea is:
We want to set up and learn how to use a collaborative tracking system. Git is
a version control system (i.e., Word's "track changes" feature on steroids).
GitHub is the most common Git-based collaborative cloud (i.e., Dropbox on
steroids). Everything in this class will be done within an RStudio Project on
your local computer that is mirrored on a private GitHub repository. If you do
not use GitHub (properly), you will receive an "incomplete" on the given
assignment, so it is imperative that you take your time with these steps and
read carefully. If you have any questions at each stage, ask! 

The following guidance regarding R/RStudio and Git/GitHub draws heavily on
[Jenny Bryan](https://jennybryan.org/){target="_blank"}'s book, [Happy Git with R](https://happygitwithr.com/){target="_blank"}, and her related
[paper](https://www.tandfonline.com/doi/full/10.1080/00031305.2017.1399928){target="_blank"} on
version control. I sprinkled in some suggestions from others. If you would like
to gain more background on basic Git and GitHub, take a look at [these slides](https://uncoast-unconf.github.io/uu-2019-day-zero/01-git-github-cornucopia/git-github-cornucopia.html#1){target="_blank"} developed by [Dr. Amelia McNamara](https://vis.social/@amelia){target="_blank"}.  

### Install R and RStudio

1. Download and install the pre-compiled binary of the most recent version (4.4+) of [R](https://cran.r-project.org){target="_blank"} appropriate for your machine's 
operating system
2. Download and install the most recent, preview version of [RStudio](https://rstudio.com/products/rstudio/download/#download){target="_blank"};
then, navigate in RStudio to Tools > Global Options to *NOT* "Restore .RData into workspace 
at setup" and *NEVER* "save workspace to .RData on exit" (it is okay that you 
might not know what this means yet)
3. If you've installed R and RStudio in the past, please download the latest 
versions and update the R packages with the following code:


``` r
# if you've previously installed R and RStudio, also update your R packages 
update.packages(ask = FALSE, checkBuilt = TRUE)
```

4. If you want to generate PDF output from R Markdown documents, you will also 
need to install LaTex. I suggest taking the following approach in the RStudio 
Console, if you have never installed LaTex. More installation guidance can be found 
[here](https://bookdown.org/yihui/rmarkdown/installation.html){target="_blank"}. 


``` r
# install R package
install.packages("tinytex")
# install LaTex "ingredients"
tinytex::install_tinytex()
```

### Install Git and create a GitHub account

1. Install [Git](https://git-scm.com/downloads){target="_blank"}. See [here](https://happygitwithr.com/install-git.html){target="_blank"} for OS-specific installation instructions. For Mac users, you need to install 
*parts of* 
[Xcode for Mac OS](https://thecoatlessprofessor.com/programming/cpp/r-compiler-tools-for-rcpp-on-macos/){target="_blank"} and 
[other things](https://mac.R-project.org/tools/){target="_blank"}). For R 4.0+, 
this process is easier, as R now uses 
[Apple Xcode 10.1 and GNU Fortran 8.2](https://github.com/fxcoudert/gfortran-for-macOS/releases){target="_blank"}. 

2. Create a [GitHub](https://github.com){target="_blank"} account. Pick a good [user name](https://happygitwithr.com/github-acct.html){target="_blank"}! 
3. [Introduce](https://happygitwithr.com/hello-git.html){target="_blank"} yourself to Git in RStudio with the following code in the Console. Provide your given name, not your user name, and the email address you used in creating your GitHub account. These commands return nothing, but you can check that it worked with `git config --global --list` in the [shell](https://happygitwithr.com/shell.html){target="_blank"}. 


``` r
# install `usethis` R package if needed (do this exactly once):
## install.packages("usethis")

library(usethis)
usethis::use_git_config(user.name = "John Doe", 
                        user.email = "john.doe@colostate.edu")
```

Note: Avoid committing credentials or other sensitive information to GitHub by "[vaccinating](https://github.com/uncoast-unconf/uu-2019-day-zero/blob/master/00-preparation/03-usethis/README.md){target="_blank"}" with `usethis::git_vaccinate()`.

4. Optional but recommended for new users: Consider downloading a [GUI Git Client](https://happygitwithr.com/git-client.html){target="_blank"} to make 
version control easier and to build intuition. 
[GitHub Desktop](https://desktop.github.com/){target="_blank"} is likely 
sufficient for this course, but the choice is yours.
5. Optional: Sign up for free student perks via 
[GitHub Education](https://education.github.com/){target="_blank"}.

### Connect Git, GitHub, and RStudio

1. Read and follow the 
[instructions](https://happygitwithr.com/connect-intro.html){target="_blank"}
in Chapters 9-13 *exactly.* I hope you won't need to look at Chapter 14! Move 
slowly and carefully, and pay attention to the specific needs for your operating 
system.

### Create new project, GitHub first

0. Keep in mind: You should save the local R Project from this step in a top-level directory. We suggest creating an `R` folder at the top of your `Documents` folder (or OS-specific equivalent) to contain all of your R Projects. For the duration of this course, you should have a directory pathname like `/user/Documents/R/[YourLastName]-MECH476`. We will discuss directory structures and pathnames more in the next chapter.
1. Read and follow these 
[instructions](https://happygitwithr.com/new-github-first.html){target="_blank"}
*exactly* with the following additions: 
- Give your GitHub username to the Instructor, so you can be added to our 
[GitHub Organization](https://github.com/MECH476-FA23){target="_blank"}
- Work with the Instructor to create a *private* repository within the organization, labeled *[YourLastName]-MECH476*
- Confirm connection between your R Project and the GitHub repository, make subfolders (`data`, `code`, `figs`) within your `[YourLastName]-MECH476` folder on your local drive, and push them up to GitHub; this supports a 
[project-oriented workflow](https://uncoast-unconf.github.io/uu-2019-day-zero/02-project-workflows/workflow.html#13){target="_blank"}

### Use RStudio/GitHub system for homework submission

For homework submission, you will download the R Markdown templates provided
to you and save and edit them within your own R Project, which is connected a
private repository on the class GitHub Organization. In order to track your
changes and communicate with the Instructor, you will
regularly *commit* changes to your R Project files with meaningful *commit messages*. We will practice commits, pushes, and pulls to the *master branch*
(main copy of R Project) during class. 

### Confirm successful set-up

At this point, you should be able to commit, push to, and pull from the master
branch of your private GitHub repository within the RStudio IDE. In later
chapters, we will provide more information on these interfaces, and you will
have plenty of opportunities to practice this workflow. For now, "minimally 
functional" is good enough!

Once you have successfully installed and connected R/RStudio and Git/GitHub, 
**open an issue** on YOUR private repository within the 
[GitHub Organization](https://github.com/MECH476-FA23){target="_blank"}. 
Mention/assign `SmogDr` to let me know everything is working 
properly, or to request more help.

Then, in the 
[public repository](https://github.com/MECH476-FA23){target="_blank"} for
class-related questions and discussion, **open another issue**. You can ask a
question, share any course-related concerns, or post a brief comment about what
you hope to gain from this course. Remember to mention/assign
`SmogDr`, so that I am alerted to your post. 


### Asking for help (properly)

All questions regarding technology and code should be directed to the Teaching
Assistant via GitHub Issues on 
[this repository](https://github.com/MECH476-FA23/questions){target="_blank"}. 
If the question requires you to include full code, please consider using the 
R package to generate reproducible examples: `reprex`. Please watch this
[tutorial](https://reprex.tidyverse.org/articles/articles/learn-reprex.html){target="_blank"}
on how to use `reprex`. Essentially, you are copy-pasting self-contained code in
the GitHub Issue, so I can recreate your work and help you more effectively.
You won't do things perfectly as you start, but, hopefully by the end of the 
semester, you will have an efficient, reproducible workflow and effective
solution-seeking toolbox. 

## Coursebook

This coursebook will serve as the only required textbook for this course. I
regularly edit and add to this book, so content may change somewhat over the
semester. We typically cover about a chapter of the book every 1-2 weeks of the
course. You need to follow along and read this book thoroughly.

This coursebook includes: 

- Links to the slides presented in class for each topic
- In-course exercises, typically including links to the data used in the
exercise
- Homework assignments
- Appendix of [reference distributions](#dist)
- A list of vocabulary and concepts that should be mastered for each quiz

If you find any typos or bugs, or if you have any suggestions for how the book
can be improved, feel free to post it on the book's [GitHub Issues](https://github.com/SmogDr/edar_coursebook/issues){target="_blank"}.

This book was developed using Yihui Xie's 
[bookdown](https://bookdown.org){target="_blank"} framework. The book is built 
using code that combines R code, data, and text to
create a book for which R code and examples can be re-executed every time the
book is re-built, which helps identify bugs and broken code examples quickly.
The online book is hosted using GitHub's free 
[GitHub Pages](https://pages.github.com){target="_blank"}. All material for 
this book is available and can be explored at the book's 
[GitHub repository](https://github.com/SmogDr/edar_coursebook){target="_blank"}.

### Other helpful books (not required)

The best book to supplement the coursebook and lectures for this course is 
[R for Data Science](http://r4ds.had.co.nz){target="_blank"} by Garrett 
Grolemund and Hadley Wickham. The entire book is freely available online through 
the same format of the coursebook. You can also purchase a paper version of the 
book published by O'Reilly for around $40. This book is an excellent and 
up-to-date reference by some of the best R programmers in the world.

There are a number of other useful books available on general R programming,
including:

- [R for Dummies](https://colostate-primo.hosted.exlibrisgroup.com/primo-explore/fulldisplay?docid=01COLSU_ALMA51267598310003361&context=L&vid=01COLSU&lang=en_US&search_scope=Everything&adaptor=Local%20Search%20Engine&tab=default_tab&query=any,contains,r%20for%20dummies&sortby=rank&offset=0){target="_blank"}
- [R Cookbook](https://rc2e.com){target="_blank"}
- [R Graphics Cookbook](https://r-graphics.org){target="_blank"}
- [Roger Peng's Leanpub books](https://leanpub.com/u/rdpeng){target="_blank"}
- Various books on [bookdown.org](https://bookdown.org/){target="_blank"}

The R programming language is used extensively within certain fields, including
statistics and bioinformatics. If you are using R for a specific type of
analysis, you will be able to find many books with advice on using R for both
general and specific statistical analysis, including many available in print or
online through the CSU library.

## Acknowledgements

Most of the introductory material for this book was adapted from Dr. Brooke 
Anderson's course on 
[R Programming for Research](https://geanders.github.io/RProgrammingForResearch/){target="_blank"}
, to whom I owe thanks not only for the materials but for the many helpful 
discussions. I would also like to acknowledge 
[John Tukey](http://rsbm.royalsocietypublishing.org/content/49/537.full.pdf+html){target="_blank"},
one of the pioneers of exploratory data analysis, and the creators of the 
[NIST Engineering Statistics Handbook](https://doi.org/10.18434/M32189){target="_blank"}, 
from which I have drawn many techniques. Kathleen Wendt `@Wendtke`, the original TA for this course, also contributed 
a tremendous amount of effort to the original course setup and execution in 2020.  Thank you!
