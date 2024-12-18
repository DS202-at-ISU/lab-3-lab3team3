
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

``` r
# Conditional installation
if (!requireNamespace("dplyr", quietly = TRUE)) {
  install.packages("dplyr")
}

if (!requireNamespace("tidyr", quietly = TRUE)) {
  install.packages("tidyr")
}

if (!requireNamespace("readr", quietly = TRUE)) {
  install.packages("readr")
}

# Load the libraries
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
library(tidyr)
library(readr)
```

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
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
#Jack
deaths <- av %>%
  pivot_longer(cols = starts_with("Death"), 
               names_to = "Time", 
               values_to = "Death") %>%
  mutate(Time = parse_number(Time)) %>%
  select(URL, Time, Death)

returns <- av %>%
  pivot_longer(cols = starts_with("Return"), 
               names_to = "Time", 
               values_to = "Return") %>%
  mutate(Time = parse_number(Time)) %>%
  select(URL, Time, Return)
 
print(deaths)
```

    ## # A tibble: 865 × 3
    ##    URL                                                 Time Death
    ##    <chr>                                              <dbl> <chr>
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)          1 "YES"
    ##  2 http://marvel.wikia.com/Henry_Pym_(Earth-616)          2 ""   
    ##  3 http://marvel.wikia.com/Henry_Pym_(Earth-616)          3 ""   
    ##  4 http://marvel.wikia.com/Henry_Pym_(Earth-616)          4 ""   
    ##  5 http://marvel.wikia.com/Henry_Pym_(Earth-616)          5 ""   
    ##  6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     1 "YES"
    ##  7 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     2 ""   
    ##  8 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     3 ""   
    ##  9 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     4 ""   
    ## 10 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     5 ""   
    ## # ℹ 855 more rows

``` r
print(returns)
```

    ## # A tibble: 865 × 3
    ##    URL                                                 Time Return
    ##    <chr>                                              <dbl> <chr> 
    ##  1 http://marvel.wikia.com/Henry_Pym_(Earth-616)          1 "NO"  
    ##  2 http://marvel.wikia.com/Henry_Pym_(Earth-616)          2 ""    
    ##  3 http://marvel.wikia.com/Henry_Pym_(Earth-616)          3 ""    
    ##  4 http://marvel.wikia.com/Henry_Pym_(Earth-616)          4 ""    
    ##  5 http://marvel.wikia.com/Henry_Pym_(Earth-616)          5 ""    
    ##  6 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     1 "YES" 
    ##  7 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     2 ""    
    ##  8 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     3 ""    
    ##  9 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     4 ""    
    ## 10 http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)     5 ""    
    ## # ℹ 855 more rows

# \<\<\<\<\<\<\< HEAD

``` r
avg_deaths <- deaths %>% # Average
  filter(Death == "YES") %>%           # Only consider actual deaths
  group_by(URL) %>%             # Group by each Avenger
  summarise(total_deaths = n()) %>%    # Count total deaths for each Avenger
  summarise(avg_deaths = mean(total_deaths))  # Calculate the average number of deaths

print(avg_deaths)
```

    ## # A tibble: 1 × 1
    ##   avg_deaths
    ##        <dbl>
    ## 1       1.29

> > > > > > > 71745a2967507adf70b844e37c08aa375b6a2718 \## Individually

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

# Individual Answers:

## Bhargav Yellepeddi:

### FiveThirtyEight Statement

> “Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team.”

### Count the number of unique Avengers who have died at least once

``` r
library(dplyr)
library(readr)

deaths_summary <- deaths %>%
  filter(tolower(Death) == "yes") %>%   # Only consider actual deaths
  distinct(URL) %>%              # Get unique Avengers who died at least once
  summarise(total_avengers_died = n())  # Count the number of unique Avengers

print(deaths_summary)
```

    ## # A tibble: 1 × 1
    ##   total_avengers_died
    ##                 <int>
    ## 1                  69

### Answer

Based on the dataset, I found that there are 69 unique Avengers who have
died at least once. Therefore, the statement that “69 Avengers had died
at least one time after joining the team” is confirmed by our analysis.

## Matthew Ritland:

### FiveThirtyEight Statement

> “I counted 89 total deaths”

\###Count number of total deaths

``` r
total_deaths <- deaths %>%
  filter(Death == "YES") %>%   # Filter for actual deaths
  summarise(total_deaths_count = n())  # Count total number of deaths

# Print total deaths
print(total_deaths)
```

    ## # A tibble: 1 × 1
    ##   total_deaths_count
    ##                <int>
    ## 1                 89

### Answer:

I filtered through the death dataset, and only kept the one’s that
included “YES” in the row, then summed the total of rows (which only
acted as counting each death)

## Jack Larson:

### FiveThirtyEight Statement

> 57 occasions the individual made a comeback

### counting the number of Avengers that returned

``` r
returns_after_death <- returns %>%
  filter(tolower(Return) == "yes") %>%
  summarise(n())

print(returns_after_death)
```

    ## # A tibble: 1 × 1
    ##   `n()`
    ##   <int>
    ## 1    57

### Include your answer

The author claimed that in total, there have been 57 total returns after
death. After my anaylsis, I found this to be true.

## Nathan Cole:

## David Chan:

### FiveThirtyEight Statement

> Given the Avengers’ 53 years in operation and overall mortality rate,
> fans of the comics can expect one current or former member to die
> every seven months or so, with a permanent death occurring once every
> 20 months.

### Total number of deaths and permanent deaths among Avengers

``` r
library(dplyr)

death_stats <- av %>%
  summarise(
    total_deaths = sum(Death1 == "YES") + sum(Death2 == "YES") + sum(Death3 == "YES") +
                   sum(Death4 == "YES") + sum(Death5 == "YES"),
    total_permanent_deaths = sum(Death1 == "YES" & Return1 == "NO") +
                             sum(Death2 == "YES" & Return2 == "NO") +
                             sum(Death3 == "YES" & Return3 == "NO") +
                             sum(Death4 == "YES" & Return4 == "NO") +
                             sum(Death5 == "YES" & Return5 == "NO"),
    years_in_operation = 53
  ) %>%
  mutate(
    avg_death_rate_months = (years_in_operation * 12) / total_deaths,
    avg_permanent_death_rate_months = (years_in_operation * 12) / total_permanent_deaths
  )

death_stats
```

    ##   total_deaths total_permanent_deaths years_in_operation avg_death_rate_months
    ## 1           89                     32                 53              7.146067
    ##   avg_permanent_death_rate_months
    ## 1                          19.875

### Include your answer:

The analysis confirms that Avengers’ deaths occur approximately every
seven months, with permanent deaths happening roughly every 20 months,
validating the FiveThirtyEight statement.
