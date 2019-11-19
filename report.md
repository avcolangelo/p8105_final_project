Report
================
11/19/2019

Start of code (place holder until we have
    page)

``` r
library(tidyverse)
```

    ## -- Attaching packages -------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.2.1     v purrr   0.3.2
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts ----------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(viridis)
```

    ## Loading required package: viridisLite

``` r
library(readxl)
library(plotly)
```

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

``` r
df = 
  read_csv("./data/squirrel_data.csv") %>% 
  janitor::clean_names() %>% 
  drop_na() %>% 
  plot_ly(
  x = ~x, y = ~y, type = "scatter", mode = "markers")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   X = col_double(),
    ##   Y = col_double(),
    ##   Date = col_double(),
    ##   `Hectare Squirrel Number` = col_double(),
    ##   Running = col_logical(),
    ##   Chasing = col_logical(),
    ##   Climbing = col_logical(),
    ##   Eating = col_logical(),
    ##   Foraging = col_logical(),
    ##   Kuks = col_logical(),
    ##   Quaas = col_logical(),
    ##   Moans = col_logical(),
    ##   `Tail flags` = col_logical(),
    ##   `Tail twitches` = col_logical(),
    ##   Approaches = col_logical(),
    ##   Indifferent = col_logical(),
    ##   `Runs from` = col_logical(),
    ##   `Zip Codes` = col_number(),
    ##   `Community Districts` = col_double(),
    ##   `Borough Boundaries` = col_double()
    ##   # ... with 2 more columns
    ## )

    ## See spec(...) for full column specifications.

## Group members (names and unis)

Alexis Colangelo, avc2129; Dania Jafar, dj2536; Si Li, sl4657; Kristal
Quispe, kq2127; Barik Rajpal, bsr2136

The written report produced by your team is central to this project.
This will detail how you completed your project, and should cover data
collection and cleaning, exploratory analyses, alternative strategies,
descriptions of approaches, and a discussion of results. We anticipate
that your project will change somewhat over time; these changes and the
reasons for them should be documented\! You will write one report
document per group, and be sure to include all group member names in the
document.

Your report should include the following topics. Depending on your
project type the amount of discussion you devote to each of them will
vary:

  - Motivation: Provide an overview of the project goals and motivation.

  - Related work: Anything that inspired you, such as a paper, a web
    site, or something we discussed in class.

  - Initial questions: What questions are you trying to answer? How did
    these questions evolve over the course of the project? What new
    questions did you consider in the course of your analysis?:

– What is the probability of a certain squirrel in a certain area;
visualize this probability/heat map – regression question: number
(count) of squirrels seen eating as predicted by foraging – Where are
squirrels are most concentrated? – What are the most common squirrel
behaviors?

  - Data: Source, scraping method, cleaning, etc.

  - Exploratory analysis: Visualizations, summaries, and exploratory
    statistical analyses. Justify the steps you took, and show any major
    changes to your ideas.

  - Additional analysis: If you undertake formal statistical analyses,
    describe these in detail Discussion: What were your findings? Are
    they what you expect? What insights into the data can you make? As
    this will be your only chance to describe your project in detail,
    make sure that your report is a standalone document that fully
    describes your process and results. We also expect you to write
    high-quality code that is understandable to an outside reader.
    Coding collaboratively and actively reviewing code within the team
    will help with this
