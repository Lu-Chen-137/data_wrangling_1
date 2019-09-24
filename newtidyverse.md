new\_tidyr\_version
================
Lu Chen
09/24/2019

wide to long
============

``` r
pulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names()
  pivot_longer(
  pulse_data,
  bdi_score_bl:bdi_score_12m,
  names_to = "visit", #creating a new column containing bdi data
  names_prefix = "bdi_score_",
  values_to = "bdi" #
) %>% 
  mutate(
    visit = recode(visit, "bl" = "00m") #change/set the value of bl in visit column into "00m"
  )
```

    ## # A tibble: 4,348 x 5
    ##       id   age sex   visit   bdi
    ##    <dbl> <dbl> <chr> <chr> <dbl>
    ##  1 10003  48.0 male  00m       7
    ##  2 10003  48.0 male  01m       1
    ##  3 10003  48.0 male  06m       2
    ##  4 10003  48.0 male  12m       0
    ##  5 10015  72.5 male  00m       6
    ##  6 10015  72.5 male  01m      NA
    ##  7 10015  72.5 male  06m      NA
    ##  8 10015  72.5 male  12m      NA
    ##  9 10022  58.5 male  00m      14
    ## 10 10022  58.5 male  01m       3
    ## # … with 4,338 more rows

seperate in litters
===================

``` r
litters_data = 
  read.csv("./data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  separate(col = group, into = c("dose", "day_of_tx"),3) #seperate the old column "group" into two new columns dose and day_of_tx;
```
