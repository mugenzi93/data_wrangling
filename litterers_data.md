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
