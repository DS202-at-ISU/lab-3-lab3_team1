
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
```

    ## Warning: package 'tidyr' was built under R version 4.3.3

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Assuming your dataset is named 'av'
# Replace 'path_to_data' with the actual path to your data file if it's not loaded in R yet

# Load data
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
  # Replace with your actual data file


# Reshape Death columns
deaths <- av %>%
  pivot_longer(cols = starts_with("Death"),
               names_to = c(".value", "Time"),
               names_pattern = "([A-Za-z]+)([0-9]+)") %>%
  mutate(Death = ifelse(Death != "", "yes", "no"))  # Mark as "yes" if not empty, otherwise "no"

# Reshape Return columns
returns <- av %>%
  pivot_longer(cols = starts_with("Return"),
               names_to = c(".value", "Time"),
               names_pattern = "([A-Za-z]+)([0-9]+)") %>%
  mutate(Return = ifelse(Return != "", "yes", "no"))  # Mark as "yes" if not empty, otherwise "no"

# Calculate average deaths and returns suffered by an Avenger
average_deaths <- deaths %>%
  group_by(Name.Alias) %>%
  summarise(avg_deaths = mean(Death == "yes", na.rm = TRUE))

average_returns <- returns %>%
  group_by(Name.Alias) %>%
  summarise(avg_returns = mean(Return == "yes", na.rm = TRUE))

# Output the results
print("Average deaths:")
```

    ## [1] "Average deaths:"

``` r
print(average_deaths, n = nrow(average_deaths))
```

    ## # A tibble: 163 × 2
    ##     Name.Alias                              avg_deaths
    ##     <chr>                                        <dbl>
    ##   1 ""                                            0.22
    ##   2 "\"Giulietta Nefaria\""                       0.2 
    ##   3 "Adam"                                        0.2 
    ##   4 "Adam Brashear"                               0.2 
    ##   5 "Alani Ryan"                                  0.2 
    ##   6 "Alex Summers"                                0.2 
    ##   7 "Alexis"                                      0.2 
    ##   8 "Alias: Jonas"                                0.2 
    ##   9 "Amadeus Cho"                                 0.2 
    ##  10 "America Chavez"                              0.2 
    ##  11 "Angelica Jones"                              0.2 
    ##  12 "Anna Marie"                                  0.2 
    ##  13 "Anthony Edward \"Tony\" Stark"               0.2 
    ##  14 "Anthony Edward Stark"                        0.2 
    ##  15 "Anthony Ludgate Druid"                       0.4 
    ##  16 "Anya Corazon"                                0.2 
    ##  17 "Ares"                                        0.4 
    ##  18 "Ashley Crawford"                             0.2 
    ##  19 "Ava Ayala"                                   0.2 
    ##  20 "Barbara Barton (nee Morse)"                  0.2 
    ##  21 "Benjamin Jacob Grimm"                        0.2 
    ##  22 "Bonita Juarez"                               0.2 
    ##  23 "Brandon Sharpe"                              0.2 
    ##  24 "Brandt"                                      0.2 
    ##  25 "Brian Braddock"                              0.2 
    ##  26 "Brunnhilde"                                  0.2 
    ##  27 "Carl Lucas"                                  0.2 
    ##  28 "Carol Susan Jane Danvers"                    0.2 
    ##  29 "Cassie Lang"                                 0.2 
    ##  30 "Charlie-27"                                  0.2 
    ##  31 "Christopher Powell"                          0.2 
    ##  32 "Circe"                                       0.2 
    ##  33 "Clinton Francis Barton"                      0.4 
    ##  34 "Craig Hollis"                                0.2 
    ##  35 "Crystal Amaquelin Maximoff"                  0.2 
    ##  36 "Daisy Johnson"                               0.2 
    ##  37 "Dane Whitman"                                0.2 
    ##  38 "Daniel Thomas Rand K'ai"                     0.2 
    ##  39 "David Alleyne"                               0.2 
    ##  40 "DeMarr Davis"                                0.2 
    ##  41 "Delroy Garrett Jr."                          0.2 
    ##  42 "Dennis Dunphy"                               0.4 
    ##  43 "Dennis Sykes"                                0.2 
    ##  44 "Doctor Stephen Vincent Strange"              0.2 
    ##  45 "Dorreen Green"                               0.2 
    ##  46 "Dorrek VIII/Theodore \"Teddy\" Altman"       0.2 
    ##  47 "Doug Taggert"                                0.2 
    ##  48 "Eden Fesi"                                   0.2 
    ##  49 "Elijah Bradley"                              0.2 
    ##  50 "Elvin Haliday"                               0.2 
    ##  51 "Emery Schaub"                                0.2 
    ##  52 "Eric Brooks"                                 0.2 
    ##  53 "Eric Kevin Masterson"                        0.4 
    ##  54 "Eric O'Grady"                                0.2 
    ##  55 "Eros"                                        0.2 
    ##  56 "Fiona"                                       0.2 
    ##  57 "Flash Thompson"                              0.2 
    ##  58 "Gene Lorrene"                                0.2 
    ##  59 "Greer Grant Nelson"                          0.2 
    ##  60 "Greg Willis"                                 0.2 
    ##  61 "Heather Douglas"                             0.4 
    ##  62 "Henry Jonathan \"Hank\" Pym"                 0.2 
    ##  63 "Henry P. McCoy"                              0.2 
    ##  64 "Heracles"                                    0.2 
    ##  65 "Humberto Lopez"                              0.2 
    ##  66 "Izzy Kane"                                   0.2 
    ##  67 "Jacques Duquesne"                            0.2 
    ##  68 "James \"Logan\" Howlett"                     0.2 
    ##  69 "James Buchanan Barnes"                       0.2 
    ##  70 "James R. Rhodes"                             0.2 
    ##  71 "James Santini"                               0.2 
    ##  72 "Janet van Dyne"                              0.2 
    ##  73 "Jeanne Foucault"                             0.2 
    ##  74 "Jennifer Takeda"                             0.2 
    ##  75 "Jennifer Walters"                            0.2 
    ##  76 "Jessica Jones"                               0.2 
    ##  77 "Jessica Miriam Drew"                         0.2 
    ##  78 "Jim Hammond (alias)"                         0.2 
    ##  79 "Jocasta"                                     1   
    ##  80 "John Aman"                                   0.2 
    ##  81 "John F. Walker"                              0.2 
    ##  82 "Johnny Gallo"                                0.2 
    ##  83 "Jonathan Hart"                               0.4 
    ##  84 "Julia Carpenter"                             0.2 
    ##  85 "Julie Power"                                 0.2 
    ##  86 "Kaluu"                                       0.2 
    ##  87 "Katherine \"Kate\" Bishop"                   0.2 
    ##  88 "Kelsey Leigh Shorr"                          0.2 
    ##  89 "Ken Mack"                                    0.2 
    ##  90 "Kevin Kale Connor"                           0.2 
    ##  91 "Kevin Masterson"                             0.2 
    ##  92 "Loki Laufeyson"                              0.2 
    ##  93 "Lyra"                                        0.2 
    ##  94 "Mar-Vell"                                    0.6 
    ##  95 "Marc Spector"                                0.2 
    ##  96 "Marcus Milton"                               0.2 
    ##  97 "Maria Hill"                                  0.2 
    ##  98 "Maria de Guadalupe Santiago"                 0.2 
    ##  99 "Marrina Smallwood"                           0.4 
    ## 100 "Martinex T'Naga"                             0.2 
    ## 101 "Matt Murdock"                                0.2 
    ## 102 "Matthew Liebowitz (birth name)"              0.4 
    ## 103 "Maya Lopez"                                  0.4 
    ## 104 "Michiko Musashi"                             0.2 
    ## 105 "Miguel Santos"                               0.2 
    ## 106 "Moira Brandon"                               0.2 
    ## 107 "Monica Chang"                                0.2 
    ## 108 "Monica Rambeau"                              0.2 
    ## 109 "Namor McKenzie"                              0.2 
    ## 110 "Natalia Alianovna Romanova"                  0.2 
    ## 111 "Nathaniel Richards"                          0.2 
    ## 112 "Nicholas Fury, Jr., Marcus Johnson"          0.2 
    ## 113 "Nicholette Gold"                             0.2 
    ## 114 "Noh-Varr"                                    0.2 
    ## 115 "Ororo Munroe"                                0.2 
    ## 116 "Otto Octavius"                               0.2 
    ## 117 "Patsy Walker"                                0.2 
    ## 118 "Peter Benjamin Parker"                       0.4 
    ## 119 "Phillip Coulson"                             0.2 
    ## 120 "Phillip Javert"                              0.2 
    ## 121 "Pietro Maximoff"                             0.2 
    ## 122 "Ravonna Lexus Renslayer"                     0.4 
    ## 123 "Reed Richards"                               0.2 
    ## 124 "Richard Milhouse Jones"                      0.2 
    ## 125 "Richard Rider"                               0.2 
    ## 126 "Rita DeMara"                                 0.2 
    ## 127 "Robbie Baldwin"                              0.2 
    ## 128 "Robert Bruce Banner"                         0.2 
    ## 129 "Robert L. Frank Sr."                         0.2 
    ## 130 "Robert Reynolds"                             0.2 
    ## 131 "Roberto da Costa"                            0.2 
    ## 132 "Sam Alexander"                               0.2 
    ## 133 "Samuel Guthrie"                              0.2 
    ## 134 "Samuel Thomas Wilson"                        0.2 
    ## 135 "Scott Edward Harris Lang"                    0.2 
    ## 136 "Shang-Chi"                                   0.2 
    ## 137 "Sharon Carter"                               0.2 
    ## 138 "Shiro Yoshida"                               0.2 
    ## 139 "Simon Williams"                              0.2 
    ## 140 "Stakar"                                      0.2 
    ## 141 "Steven Rogers"                               0.2 
    ## 142 "Susan Richards (nee Storm)"                  0.2 
    ## 143 "T'Challa"                                    0.2 
    ## 144 "Taki Matsuya"                                0.2 
    ## 145 "Thaddeus Ross"                               0.2 
    ## 146 "Thomas \"Tommy\" Shepherd"                   0.2 
    ## 147 "Thor Odinson"                                0.4 
    ## 148 "Tony Masters"                                0.2 
    ## 149 "Val Ventura"                                 0.2 
    ## 150 "Vance Astrovik"                              0.2 
    ## 151 "Veranke"                                     0.2 
    ## 152 "Victor Alvarez"                              0.2 
    ## 153 "Victor Mancha"                               0.2 
    ## 154 "Victor Shade (alias)"                        0.2 
    ## 155 "Wade Wilson"                                 0.2 
    ## 156 "Walter Newell"                               0.2 
    ## 157 "Wanda Maximoff"                              0.2 
    ## 158 "Wendell Elvis Vaughn"                        0.2 
    ## 159 "William \"Billy\" Kaplan"                    0.2 
    ## 160 "William Baker"                               0.4 
    ## 161 "X-51"                                        0.2 
    ## 162 "Yondu Udonta"                                0.2 
    ## 163 "Yvette"                                      0.2

``` r
print("Average returns:")
```

    ## [1] "Average returns:"

``` r
print(average_returns, n = nrow(average_returns))
```

    ## # A tibble: 163 × 2
    ##     Name.Alias                              avg_returns
    ##     <chr>                                         <dbl>
    ##   1 ""                                             0.14
    ##   2 "\"Giulietta Nefaria\""                        0.2 
    ##   3 "Adam"                                         0.2 
    ##   4 "Adam Brashear"                                0   
    ##   5 "Alani Ryan"                                   0   
    ##   6 "Alex Summers"                                 0   
    ##   7 "Alexis"                                       0   
    ##   8 "Alias: Jonas"                                 0.2 
    ##   9 "Amadeus Cho"                                  0   
    ##  10 "America Chavez"                               0   
    ##  11 "Angelica Jones"                               0   
    ##  12 "Anna Marie"                                   0   
    ##  13 "Anthony Edward \"Tony\" Stark"                0.2 
    ##  14 "Anthony Edward Stark"                         0.2 
    ##  15 "Anthony Ludgate Druid"                        0.4 
    ##  16 "Anya Corazon"                                 0   
    ##  17 "Ares"                                         0.4 
    ##  18 "Ashley Crawford"                              0   
    ##  19 "Ava Ayala"                                    0   
    ##  20 "Barbara Barton (nee Morse)"                   0.2 
    ##  21 "Benjamin Jacob Grimm"                         0.2 
    ##  22 "Bonita Juarez"                                0   
    ##  23 "Brandon Sharpe"                               0   
    ##  24 "Brandt"                                       0.2 
    ##  25 "Brian Braddock"                               0   
    ##  26 "Brunnhilde"                                   0   
    ##  27 "Carl Lucas"                                   0   
    ##  28 "Carol Susan Jane Danvers"                     0   
    ##  29 "Cassie Lang"                                  0.2 
    ##  30 "Charlie-27"                                   0   
    ##  31 "Christopher Powell"                           0   
    ##  32 "Circe"                                        0   
    ##  33 "Clinton Francis Barton"                       0.4 
    ##  34 "Craig Hollis"                                 0   
    ##  35 "Crystal Amaquelin Maximoff"                   0   
    ##  36 "Daisy Johnson"                                0   
    ##  37 "Dane Whitman"                                 0   
    ##  38 "Daniel Thomas Rand K'ai"                      0   
    ##  39 "David Alleyne"                                0   
    ##  40 "DeMarr Davis"                                 0.2 
    ##  41 "Delroy Garrett Jr."                           0   
    ##  42 "Dennis Dunphy"                                0.4 
    ##  43 "Dennis Sykes"                                 0.2 
    ##  44 "Doctor Stephen Vincent Strange"               0   
    ##  45 "Dorreen Green"                                0   
    ##  46 "Dorrek VIII/Theodore \"Teddy\" Altman"        0   
    ##  47 "Doug Taggert"                                 0.2 
    ##  48 "Eden Fesi"                                    0   
    ##  49 "Elijah Bradley"                               0   
    ##  50 "Elvin Haliday"                                0   
    ##  51 "Emery Schaub"                                 0   
    ##  52 "Eric Brooks"                                  0   
    ##  53 "Eric Kevin Masterson"                         0.4 
    ##  54 "Eric O'Grady"                                 0.2 
    ##  55 "Eros"                                         0   
    ##  56 "Fiona"                                        0   
    ##  57 "Flash Thompson"                               0   
    ##  58 "Gene Lorrene"                                 0   
    ##  59 "Greer Grant Nelson"                           0   
    ##  60 "Greg Willis"                                  0   
    ##  61 "Heather Douglas"                              0.4 
    ##  62 "Henry Jonathan \"Hank\" Pym"                  0.2 
    ##  63 "Henry P. McCoy"                               0   
    ##  64 "Heracles"                                     0   
    ##  65 "Humberto Lopez"                               0.2 
    ##  66 "Izzy Kane"                                    0   
    ##  67 "Jacques Duquesne"                             0.2 
    ##  68 "James \"Logan\" Howlett"                      0.2 
    ##  69 "James Buchanan Barnes"                        0   
    ##  70 "James R. Rhodes"                              0   
    ##  71 "James Santini"                                0   
    ##  72 "Janet van Dyne"                               0.2 
    ##  73 "Jeanne Foucault"                              0   
    ##  74 "Jennifer Takeda"                              0   
    ##  75 "Jennifer Walters"                             0.2 
    ##  76 "Jessica Jones"                                0   
    ##  77 "Jessica Miriam Drew"                          0   
    ##  78 "Jim Hammond (alias)"                          0.2 
    ##  79 "Jocasta"                                      1   
    ##  80 "John Aman"                                    0   
    ##  81 "John F. Walker"                               0   
    ##  82 "Johnny Gallo"                                 0   
    ##  83 "Jonathan Hart"                                0.4 
    ##  84 "Julia Carpenter"                              0   
    ##  85 "Julie Power"                                  0   
    ##  86 "Kaluu"                                        0   
    ##  87 "Katherine \"Kate\" Bishop"                    0   
    ##  88 "Kelsey Leigh Shorr"                           0   
    ##  89 "Ken Mack"                                     0.2 
    ##  90 "Kevin Kale Connor"                            0.2 
    ##  91 "Kevin Masterson"                              0   
    ##  92 "Loki Laufeyson"                               0   
    ##  93 "Lyra"                                         0   
    ##  94 "Mar-Vell"                                     0.6 
    ##  95 "Marc Spector"                                 0   
    ##  96 "Marcus Milton"                                0.2 
    ##  97 "Maria Hill"                                   0   
    ##  98 "Maria de Guadalupe Santiago"                  0   
    ##  99 "Marrina Smallwood"                            0.4 
    ## 100 "Martinex T'Naga"                              0   
    ## 101 "Matt Murdock"                                 0   
    ## 102 "Matthew Liebowitz (birth name)"               0.4 
    ## 103 "Maya Lopez"                                   0.4 
    ## 104 "Michiko Musashi"                              0   
    ## 105 "Miguel Santos"                                0   
    ## 106 "Moira Brandon"                                0.2 
    ## 107 "Monica Chang"                                 0   
    ## 108 "Monica Rambeau"                               0   
    ## 109 "Namor McKenzie"                               0.2 
    ## 110 "Natalia Alianovna Romanova"                   0.2 
    ## 111 "Nathaniel Richards"                           0   
    ## 112 "Nicholas Fury, Jr., Marcus Johnson"           0   
    ## 113 "Nicholette Gold"                              0   
    ## 114 "Noh-Varr"                                     0   
    ## 115 "Ororo Munroe"                                 0   
    ## 116 "Otto Octavius"                                0.2 
    ## 117 "Patsy Walker"                                 0.2 
    ## 118 "Peter Benjamin Parker"                        0.4 
    ## 119 "Phillip Coulson"                              0   
    ## 120 "Phillip Javert"                               0   
    ## 121 "Pietro Maximoff"                              0.2 
    ## 122 "Ravonna Lexus Renslayer"                      0.4 
    ## 123 "Reed Richards"                                0   
    ## 124 "Richard Milhouse Jones"                       0   
    ## 125 "Richard Rider"                                0.2 
    ## 126 "Rita DeMara"                                  0.2 
    ## 127 "Robbie Baldwin"                               0   
    ## 128 "Robert Bruce Banner"                          0.2 
    ## 129 "Robert L. Frank Sr."                          0.2 
    ## 130 "Robert Reynolds"                              0.2 
    ## 131 "Roberto da Costa"                             0   
    ## 132 "Sam Alexander"                                0   
    ## 133 "Samuel Guthrie"                               0   
    ## 134 "Samuel Thomas Wilson"                         0   
    ## 135 "Scott Edward Harris Lang"                     0.2 
    ## 136 "Shang-Chi"                                    0   
    ## 137 "Sharon Carter"                                0.2 
    ## 138 "Shiro Yoshida"                                0.2 
    ## 139 "Simon Williams"                               0.2 
    ## 140 "Stakar"                                       0   
    ## 141 "Steven Rogers"                                0.2 
    ## 142 "Susan Richards (nee Storm)"                   0   
    ## 143 "T'Challa"                                     0   
    ## 144 "Taki Matsuya"                                 0   
    ## 145 "Thaddeus Ross"                                0.2 
    ## 146 "Thomas \"Tommy\" Shepherd"                    0   
    ## 147 "Thor Odinson"                                 0.4 
    ## 148 "Tony Masters"                                 0   
    ## 149 "Val Ventura"                                  0   
    ## 150 "Vance Astrovik"                               0   
    ## 151 "Veranke"                                      0.2 
    ## 152 "Victor Alvarez"                               0   
    ## 153 "Victor Mancha"                                0.2 
    ## 154 "Victor Shade (alias)"                         0.2 
    ## 155 "Wade Wilson"                                  0.2 
    ## 156 "Walter Newell"                                0   
    ## 157 "Wanda Maximoff"                               0.2 
    ## 158 "Wendell Elvis Vaughn"                         0.2 
    ## 159 "William \"Billy\" Kaplan"                     0   
    ## 160 "William Baker"                                0.2 
    ## 161 "X-51"                                         0.2 
    ## 162 "Yondu Udonta"                                 0   
    ## 163 "Yvette"                                       0

``` r
# deaths <- av %>% select(
#   URL, 
#   Name.Alias,
#   starts_with("Death")
# ) %>% pivot_longer(
#   cols = Death1:Death5,
#   names_to = "Time",
#   values_to = "Died"
# )
# deaths
# deaths <- deaths %>% mutate(
#   Time = parse_number(Time)
# )
# maxdeaths <- deaths %>% 
#   filter(Died != "") %>% 
#   group_by(URL, Died) %>% 
#   summarise(
#     Time_max = max(Time, na.rm = TRUE),
#     .groups = "drop"
#   )
# 
# maxdeaths %>% count(Time_max, Died)
```

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
