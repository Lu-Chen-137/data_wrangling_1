Data Import
================
Lu Chen
09/17/2017

load in a dataset
-----------------

``` r
litters_data = read_csv(file = "./data/FAS_litters.csv") #read_csv would guess the type of your coloumn
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

``` r
litters_data = janitor::clean_names(litters_data) #use the janitor package to clean up the single quotation mark and Upper case in the variable names
```

load in pup data
================

``` r
pups_data = read_csv(file = "./data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

``` r
pups_data = janitor::clean_names(pups_data)
```

play with column parsing
========================

``` r
litters_data = read_csv(
  file = "./data/FAS_litters.csv",
    col_types = cols(
    Group = col_character(),
    `Litter Number` = col_character(), #having ` since there is space in the column name
    `GD0 weight` = col_double(),
    `GD18 weight` = col_double(),
    `GD of Birth` = col_integer(),
    `Pups born alive` = col_integer(),
    `Pups dead @ birth` = col_integer(),
    `Pups survive` = col_integer()
 )
)
```

read in an excel file
=====================

``` r
mlb11_data = 
  read_excel(
    path = "./data/mlb11.xlsx")
    #range = "A1:D7"

#write_csv(mlb11_data_subset, path = "./data/mlb1_subset")
```

read in SAS
===========

``` r
pulse_data = haven::read_sas("./data/public_pulse_data.sas7bdat")
```
