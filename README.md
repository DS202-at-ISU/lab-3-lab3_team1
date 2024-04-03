
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(tidyr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.5
    ## ✔ ggplot2   3.4.4     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
# Assuming your dataset is named 'av'
# Replace 'path_to_data' with the actual path to your data file if it's not loaded in R yet

# Load data
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
  # Replace with your actual data file


# # Reshape Death columns
# deaths <- av %>%
#   pivot_longer(cols = starts_with("Death"),
#                names_to = c(".value", "Time"),
#                names_pattern = "([A-Za-z]+)([0-9]+)") %>%
#   mutate(Death = ifelse(Death != "", "yes", "no"))  # Mark as "yes" if not empty, otherwise "no"
# 
# # Reshape Return columns
# returns <- av %>%
#   pivot_longer(cols = starts_with("Return"),
#                names_to = c(".value", "Time"),
#                names_pattern = "([A-Za-z]+)([0-9]+)") %>%
#   mutate(Return = ifelse(Return != "", "yes", "no"))  # Mark as "yes" if not empty, otherwise "no"
# 
# # Calculate average deaths and returns suffered by an Avenger
# average_deaths <- deaths %>%
#   group_by(Name.Alias) %>%
#   summarise(avg_deaths = mean(Death == "yes", na.rm = TRUE))
# 
# average_returns <- returns %>%
#   group_by(Name.Alias) %>%
#   summarise(avg_returns = mean(Return == "yes", na.rm = TRUE))
# 
# # Output the results
# print("Average deaths:")
# print(average_deaths, n = nrow(average_deaths))
# print("Average returns:")
# print(average_returns, n = nrow(average_returns))
```

``` r
library(tidyr)
library(dplyr)
library(tidyverse)


deaths <- av %>% select(
  URL,
  Name.Alias,
  starts_with("Death")
) %>% pivot_longer(
  cols = Death1:Death5,
  names_to = "Time",
  values_to = "Died"
)
deaths
```

    ## # A tibble: 865 × 4
    ##    URL                                                Name.Alias     Time  Died 
    ##    <chr>                                              <chr>          <chr> <chr>
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonath… Deat… "YES"
    ##  2 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonath… Deat… ""   
    ##  3 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonath… Deat… ""   
    ##  4 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonath… Deat… ""   
    ##  5 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonath… Deat… ""   
    ##  6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dy… Deat… "YES"
    ##  7 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dy… Deat… ""   
    ##  8 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dy… Deat… ""   
    ##  9 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dy… Deat… ""   
    ## 10 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van Dy… Deat… ""   
    ## # ℹ 855 more rows

``` r
deaths <- deaths %>% mutate(
  Time = parse_number(Time)
)
maxdeaths <- deaths %>%
  filter(Died != "") %>%
  group_by(URL, Died) %>%
  summarise(
    Time_max = max(Time, na.rm = TRUE),
    .groups = "drop"
  )

maxdeaths %>% count(Time_max, Died)
```

    ## # A tibble: 6 × 3
    ##   Time_max Died      n
    ##      <dbl> <chr> <int>
    ## 1        1 NO      104
    ## 2        1 YES      53
    ## 3        2 NO        1
    ## 4        2 YES      14
    ## 5        3 YES       1
    ## 6        5 YES       1

``` r
deathsavg <-  maxdeaths %>% filter(Died == "YES")
deathsavg %>% count(Time_max, Died)
```

    ## # A tibble: 4 × 3
    ##   Time_max Died      n
    ##      <dbl> <chr> <int>
    ## 1        1 YES      53
    ## 2        2 YES      14
    ## 3        3 YES       1
    ## 4        5 YES       1

``` r
returns <- av %>% select(
  URL,
  Name.Alias,
  starts_with("Return")
) %>% pivot_longer(
  cols = Return1:Return5,
  names_to = "Time",
  values_to = "Return"
)
returns
```

    ## # A tibble: 865 × 4
    ##    URL                                                Name.Alias    Time  Return
    ##    <chr>                                              <chr>         <chr> <chr> 
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonat… Retu… "NO"  
    ##  2 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonat… Retu… ""    
    ##  3 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonat… Retu… ""    
    ##  4 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonat… Retu… ""    
    ##  5 http://marvel.wikia.com/Henry_Pym_(Earth-616)      "Henry Jonat… Retu… ""    
    ##  6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van D… Retu… "YES" 
    ##  7 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van D… Retu… ""    
    ##  8 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van D… Retu… ""    
    ##  9 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van D… Retu… ""    
    ## 10 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616) "Janet van D… Retu… ""    
    ## # ℹ 855 more rows

``` r
returns <- returns %>% mutate(
  Time = parse_number(Time)
)
maxreturns <- returns %>%
  filter(Return != "") %>%
  group_by(URL, Return) %>%
  summarise(
    Time_max = max(Time, na.rm = TRUE),
    .groups = "drop"
  )

maxreturns %>% count(Time_max, Return)
```

    ## # A tibble: 6 × 3
    ##   Time_max Return     n
    ##      <dbl> <chr>  <int>
    ## 1        1 NO        23
    ## 2        1 YES       38
    ## 3        2 NO         8
    ## 4        2 YES        7
    ## 5        3 NO         1
    ## 6        5 YES        1

``` r
deathsavg %>% count(Time_max, Died) -> tt
tt
```

    ## # A tibble: 4 × 3
    ##   Time_max Died      n
    ##      <dbl> <chr> <int>
    ## 1        1 YES      53
    ## 2        2 YES      14
    ## 3        3 YES       1
    ## 4        5 YES       1

``` r
# A tibble: 4 × 3

tt$Time_max
```

    ## [1] 1 2 3 5

``` r
tt$n
```

    ## [1] 53 14  1  1

``` r
tt$Time_max * tt$n
```

    ## [1] 53 28  3  5

``` r
sum(tt$Time_max * tt$n)
```

    ## [1] 89

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.
