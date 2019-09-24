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

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)

pivot_wider(
  #pivot_longer: transfer long format to wide format
  analysis_result,
  names_from = time,
  values_from = mean
)
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

### bind rows

``` r
fellowship_data = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship")

two_towers = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")

lotr_data = 
  bind_rows(fellowship_data, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words"
  ) %>% 
  select(movie, race, sex, words)
```

### join datasets

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) #recoding the numerical variable to categorical variable

litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))
```

try to join these datasets!!

``` r
fas_data = 
  left_join(pup_data, litter_data, by = "litter_number") #by: the joining variable;the left_join statement is clear, if you do not specify which variable you want to join by, it will auto-join with the variable the system picked;

  full_join(pup_data, litter_data, by = "litter_number") %>% filter(is.na(sex))
```

    ## # A tibble: 2 x 13
    ##   litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##   <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ## 1 #112          <NA>       NA      NA       NA      NA low7        23.9
    ## 2 #7/82-3-2     <NA>       NA      NA       NA      NA mod8        26.9
    ## # … with 5 more variables: gd18_weight <dbl>, gd_of_birth <int>,
    ## #   pups_born_alive <int>, pups_dead_birth <int>, wt_gain <dbl>
