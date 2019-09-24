Data\_import
================
Clement Mugenzi
9/17/2019

# Loading the dataset

``` r
## Reads in a dataset
## Absolute path and relative path - prefer relative path
wrang_df <- read_csv(file = "data_import_examples/FAS_litters.csv",
                     col_types = cols(
    Group = col_character(),
    `Litter Number` = col_character(),
    `GD0 weight` = col_double(),
    `GD18 weight` = col_double(),
    `GD of Birth` = col_integer(),
    `Pups born alive` = col_integer(),
    `Pups dead @ birth` = col_integer(),
    `Pups survive` = col_integer()
  )
)
wrang_df <- janitor::clean_names(wrang_df) # cleaning names and 
# also use janitor::.. only if the package has not been loaded.

wrang_df = janitor::clean_names(wrang_df)
```

``` r
# loading in the pups data
pups_df <- read_csv("data_import_examples/FAS_pups.csv")
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
pups_df <- janitor::clean_names(pups_df) # dont view dataset in rmarkdown
```

run this class(pull(pups\_df, )) to see what kind of variable you have.

## Read in excel file…

``` r
mlb11_data = read_excel(path = "data_import_examples/mlb11.xlsx",
                        range = "A1:D7") 

# range just reads a subset of the dataset
view(mlb11_data)
```

## Read in SAS

``` r
pulse_data = read_sas("data_import_examples/public_pulse_data.sas7bdat")
view(pulse_data)
# So why read_csv and not read.csv? Because read_csv imports datasets as a tibble, not a dataframe whereas read.csv imports data as dataframes.
```

# Data Manipulation

``` r
select(wrang_df, group, litter_number)
```

    ## # A tibble: 49 x 2
    ##    group litter_number  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # … with 39 more rows

``` r
select(wrang_df, group, litter_number, gd0_weight)
```

    ## # A tibble: 49 x 3
    ##    group litter_number   gd0_weight
    ##    <chr> <chr>                <dbl>
    ##  1 Con7  #85                   19.7
    ##  2 Con7  #1/2/95/2             27  
    ##  3 Con7  #5/5/3/83/3-3         26  
    ##  4 Con7  #5/4/2/95/2           28.5
    ##  5 Con7  #4/2/95/3-3           NA  
    ##  6 Con7  #2/2/95/3-2           NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA  
    ##  8 Con8  #3/83/3-3             NA  
    ##  9 Con8  #2/95/3               NA  
    ## 10 Con8  #3/5/2/2/95           28.5
    ## # … with 39 more rows

``` r
select(wrang_df, group, litter_number, gd0_weight, starts_with("pups"))
```

    ## # A tibble: 49 x 6
    ##    group litter_number gd0_weight pups_born_alive pups_dead_birth
    ##    <chr> <chr>              <dbl>           <int>           <int>
    ##  1 Con7  #85                 19.7               3               4
    ##  2 Con7  #1/2/95/2           27                 8               0
    ##  3 Con7  #5/5/3/83/3-3       26                 6               0
    ##  4 Con7  #5/4/2/95/2         28.5               5               1
    ##  5 Con7  #4/2/95/3-3         NA                 6               0
    ##  6 Con7  #2/2/95/3-2         NA                 6               0
    ##  7 Con7  #1/5/3/83/3-…       NA                 9               0
    ##  8 Con8  #3/83/3-3           NA                 9               1
    ##  9 Con8  #2/95/3             NA                 8               0
    ## 10 Con8  #3/5/2/2/95         28.5               8               0
    ## # … with 39 more rows, and 1 more variable: pups_survive <int>

``` r
select(wrang_df, litter_number, group, gd0_weight)
```

    ## # A tibble: 49 x 3
    ##    litter_number   group gd0_weight
    ##    <chr>           <chr>      <dbl>
    ##  1 #85             Con7        19.7
    ##  2 #1/2/95/2       Con7        27  
    ##  3 #5/5/3/83/3-3   Con7        26  
    ##  4 #5/4/2/95/2     Con7        28.5
    ##  5 #4/2/95/3-3     Con7        NA  
    ##  6 #2/2/95/3-2     Con7        NA  
    ##  7 #1/5/3/83/3-3/2 Con7        NA  
    ##  8 #3/83/3-3       Con8        NA  
    ##  9 #2/95/3         Con8        NA  
    ## 10 #3/5/2/2/95     Con8        28.5
    ## # … with 39 more rows

``` r
select(wrang_df, litter_number, group, everything())
```

    ## # A tibble: 49 x 8
    ##    litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>         <chr>      <dbl>       <dbl>       <int>           <int>
    ##  1 #85           Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2     Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2   Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3   Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2   Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-… Con7        NA          NA            20               9
    ##  8 #3/83/3-3     Con8        NA          NA            20               9
    ##  9 #2/95/3       Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95   Con8        28.5        NA            20               8
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
select(wrang_df, litter_number, -group)
```

    ## # A tibble: 49 x 1
    ##    litter_number  
    ##    <chr>          
    ##  1 #85            
    ##  2 #1/2/95/2      
    ##  3 #5/5/3/83/3-3  
    ##  4 #5/4/2/95/2    
    ##  5 #4/2/95/3-3    
    ##  6 #2/2/95/3-2    
    ##  7 #1/5/3/83/3-3/2
    ##  8 #3/83/3-3      
    ##  9 #2/95/3        
    ## 10 #3/5/2/2/95    
    ## # … with 39 more rows

``` r
select(wrang_df, litter_number, gd0_weight:pups_born_alive) # define the new dataframe by calling it another name.
```

    ## # A tibble: 49 x 5
    ##    litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>                <dbl>       <dbl>       <int>           <int>
    ##  1 #85                   19.7        34.7          20               3
    ##  2 #1/2/95/2             27          42            19               8
    ##  3 #5/5/3/83/3-3         26          41.4          19               6
    ##  4 #5/4/2/95/2           28.5        44.1          19               5
    ##  5 #4/2/95/3-3           NA          NA            20               6
    ##  6 #2/2/95/3-2           NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 #3/83/3-3             NA          NA            20               9
    ##  9 #2/95/3               NA          NA            20               8
    ## 10 #3/5/2/2/95           28.5        NA            20               8
    ## # … with 39 more rows

``` r
select(wrang_df, GROUP = group, litter_number) # rename a variable through select function.
```

    ## # A tibble: 49 x 2
    ##    GROUP litter_number  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # … with 39 more rows

``` r
rename(wrang_df, GROUP = group, litter.number = litter_number)
```

    ## # A tibble: 49 x 8
    ##    GROUP litter.number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

## Filtering

``` r
filter(wrang_df, group == "Mod8") # == differentiate between assigning a new parameter and equalizing 
```

    ## # A tibble: 7 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Mod8  #97                 24.5        42.8          20               8
    ## 2 Mod8  #5/93               NA          41.1          20              11
    ## 3 Mod8  #5/93/2             NA          NA            19               8
    ## 4 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 5 Mod8  #7/110/3-2          27.5        46            19               8
    ## 6 Mod8  #2/95/2             28.5        44.5          20               9
    ## 7 Mod8  #82/4               33.4        52.7          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(wrang_df, gd_of_birth == 20)
```

    ## # A tibble: 32 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  3 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  4 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  5 Con8  #3/83/3-3           NA          NA            20               9
    ##  6 Con8  #2/95/3             NA          NA            20               8
    ##  7 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  8 Con8  #1/6/2/2/95-2       NA          NA            20               7
    ##  9 Con8  #3/5/3/83/3-…       NA          NA            20               8
    ## 10 Con8  #3/6/2/2/95-3       NA          NA            20               7
    ## # … with 22 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
filter(wrang_df, gd_of_birth < 20)
```

    ## # A tibble: 17 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #5/4/3/83/3         28          NA            19               9
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #59                 17          33.4          19               8
    ##  7 Mod7  #103                21.4        42.1          19               9
    ##  8 Mod7  #1/82/3-2           NA          NA            19               6
    ##  9 Mod7  #3/83/3-2           NA          NA            19               8
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## 11 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 12 Mod7  #94/2               24.4        42.9          19               7
    ## 13 Mod7  #62                 19.5        35.9          19               7
    ## 14 Low7  #112                23.9        40.5          19               6
    ## 15 Mod8  #5/93/2             NA          NA            19               8
    ## 16 Mod8  #7/110/3-2          27.5        46            19               8
    ## 17 Low8  #79                 25.4        43.8          19               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(wrang_df, pups_born_alive < 6, group == "Con7")
```

    ## # A tibble: 2 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(wrang_df, group == "Con7" | group == "Mod8") # Using OR
```

    ## # A tibble: 14 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Mod8  #97                 24.5        42.8          20               8
    ##  9 Mod8  #5/93               NA          41.1          20              11
    ## 10 Mod8  #5/93/2             NA          NA            19               8
    ## 11 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 12 Mod8  #7/110/3-2          27.5        46            19               8
    ## 13 Mod8  #2/95/2             28.5        44.5          20               9
    ## 14 Mod8  #82/4               33.4        52.7          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(wrang_df, group %in% c("Con7", "Mod8")) # this and line 90 do the same thing
```

    ## # A tibble: 14 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Mod8  #97                 24.5        42.8          20               8
    ##  9 Mod8  #5/93               NA          41.1          20              11
    ## 10 Mod8  #5/93/2             NA          NA            19               8
    ## 11 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 12 Mod8  #7/110/3-2          27.5        46            19               8
    ## 13 Mod8  #2/95/2             28.5        44.5          20               9
    ## 14 Mod8  #82/4               33.4        52.7          20               8
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(wrang_df, pups_born_alive >= 4, pups_born_alive <= 6)
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  4 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #1/82/3-2           NA          NA            19               6
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Low7  #112                23.9        40.5          19               6
    ## 11 Low8  #4/84               21.8        35.2          20               4
    ## 12 Low8  #99                 23.5        39            20               6
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
# Don't do this:
# filter(wrang_df, is.na(gd0_weight))

drop_na(wrang_df, gd0_weight, gd18_weight) # Getting rid of missing values
```

    ## # A tibble: 31 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Mod7  #94/2               24.4        42.9          19               7
    ## # … with 21 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

## Mutate

``` r
mutate(wrang_df, wt_gain = gd18_weight - gd0_weight,
       group = str_to_lower(group))
```

    ## # A tibble: 49 x 9
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 con8  #3/83/3-3           NA          NA            20               9
    ##  9 con8  #2/95/3             NA          NA            20               8
    ## 10 con8  #3/5/2/2/95         28.5        NA            20               8
    ## # … with 39 more rows, and 3 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>

## Arrange (same as sort)

``` r
arrange(wrang_df, pups_born_alive)
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  8 Mod7  #106                21.7        37.8          20               5
    ##  9 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 10 Con7  #4/2/95/3-3         NA          NA            20               6
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
arrange(wrang_df, desc(pups_born_alive))
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Low7  #102                22.6        43.3          20              11
    ##  2 Mod8  #5/93               NA          41.1          20              11
    ##  3 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  4 Con8  #3/83/3-3           NA          NA            20               9
    ##  5 Con8  #5/4/3/83/3         28          NA            19               9
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #4/2/95/2           23.5        NA            19               9
    ##  8 Mod7  #8/110/3-2          NA          NA            20               9
    ##  9 Low7  #107                22.6        42.4          20               9
    ## 10 Low7  #98                 23.8        43.8          20               9
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
arrange(wrang_df, pups_born_alive, gd0_weight) # goes in a hierarchy order.
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Mod7  #106                21.7        37.8          20               5
    ##  5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  8 Con8  #2/2/95/2           NA          NA            19               5
    ##  9 Low8  #99                 23.5        39            20               6
    ## 10 Low7  #112                23.9        40.5          19               6
    ## # … with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

## Pipe

let us create a collection of commands…

``` r
wrang_df = 
  read_csv("data_manipulation_examples/FAS_litters.csv") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) %>%
  drop_na(gd0_weight)
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
wrang_df
```

    ## # A tibble: 34 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 con8  #5/4/3/83/3         28          NA            19               9
    ##  7 mod7  #59                 17          33.4          19               8
    ##  8 mod7  #103                21.4        42.1          19               9
    ##  9 mod7  #3/82/3-2           28          45.9          20               5
    ## 10 mod7  #4/2/95/2           23.5        NA            19               9
    ## # … with 24 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   wt_gain <dbl>

``` r
wrang_df = 
  read_csv("data_manipulation_examples/FAS_litters.csv") %>%
  janitor::clean_names(dat = .) %>%
  select(.data = ., -pups_survive)
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
wrang_df
```

    ## # A tibble: 49 x 7
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## # … with 39 more rows, and 1 more variable: pups_dead_birth <dbl>

``` r
# Do not worry about this right now.
```

# Tidying Data

## Wide to long

``` r
library(tidyr)
pulse_data = 
  haven::read_sas("data_Tidy_examples /public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>%
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) %>% 
  mutate(
    visit = recode(visit, "bl" = "00m")
  )
```

## Separate in litters

``` r
wrang_df = 
  read_csv("data_Tidy_examples /FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  separate(col = group, into = c("dose", "day_of_tx"), 3)
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
view(wrang_df)
```

## Using pivot\_wider

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)
# longer to wider
pivot_wider(
  analysis_result,
  names_from = time,
  values_from = mean,
)
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

## Binding Rows

``` r
fellowship_ring = 
  readxl::read_excel("data_Tidy_examples /LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship")

two_towers = 
  readxl::read_excel("data_Tidy_examples /LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = 
  readxl::read_excel("data_Tidy_examples /LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")

lotr_data = 
  bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words"
  ) %>% 
  select(movie, race, sex, words)
```

## Joining datasets

``` r
pup_data = 
  read_csv("data_Tidy_examples /FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = 
  read_csv("data_Tidy_examples /FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))

fas_data = 
  left_join(pup_data, litter_data, by = "litter_number")
fas_data
```

    ## # A tibble: 313 x 13
    ##    litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##    <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ##  1 #85           male        4      13        7      11 con7        19.7
    ##  2 #85           male        4      13        7      12 con7        19.7
    ##  3 #1/2/95/2     male        5      13        7       9 con7        27  
    ##  4 #1/2/95/2     male        5      13        8      10 con7        27  
    ##  5 #5/5/3/83/3-3 male        5      13        8      10 con7        26  
    ##  6 #5/5/3/83/3-3 male        5      14        6       9 con7        26  
    ##  7 #5/4/2/95/2   male       NA      14        5       9 con7        28.5
    ##  8 #4/2/95/3-3   male        4      13        6       8 con7        NA  
    ##  9 #4/2/95/3-3   male        4      13        7       9 con7        NA  
    ## 10 #2/2/95/3-2   male        4      NA        8      10 con7        NA  
    ## # … with 303 more rows, and 5 more variables: gd18_weight <dbl>,
    ## #   gd_of_birth <int>, pups_born_alive <int>, pups_dead_birth <int>,
    ## #   wt_gain <dbl>

``` r
# Try to join these

fas_data = 
  left_join(pup_data, litter_data, by = "litter_number")
```
