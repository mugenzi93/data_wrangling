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