NYC Filming Permits
================

## Load required package(s)

``` r
library(tidyverse)
library(lubridate)
```

## Load dataset

``` r
film_permits <- read_csv('film-permits.csv') %>% 
    janitor::clean_names() %>% 
    mutate(
        start_date_time = mdy_hms(start_date_time),
        end_date_time = mdy_hms(end_date_time),
        entered_on = mdy_hms(entered_on),
        permit_duration_mins = end_date_time - start_date_time)
```

``` r
head(film_permits)
```

    ## # A tibble: 6 x 15
    ##   event_id event_type start_date_time     end_date_time      
    ##      <dbl> <chr>      <dttm>              <dttm>             
    ## 1   446040 Shooting … 2018-10-19 14:00:00 2018-10-20 04:00:00
    ## 2   446168 Shooting … 2018-10-19 14:00:00 2018-10-20 02:00:00
    ## 3   186438 Shooting … 2014-10-30 07:00:00 2014-10-31 02:00:00
    ## 4   445255 Shooting … 2018-10-20 07:00:00 2018-10-20 18:00:00
    ## 5   128794 Theater L… 2013-11-16 00:01:00 2013-11-17 06:00:00
    ## 6    43547 Shooting … 2012-01-10 07:00:00 2012-01-10 19:00:00
    ## # … with 11 more variables: entered_on <dttm>, event_agency <chr>,
    ## #   parking_held <chr>, borough <chr>, community_board_s <chr>,
    ## #   police_precinct_s <chr>, category <chr>, sub_category_name <chr>,
    ## #   country <chr>, zip_code_s <chr>, permit_duration_mins <drtn>

``` r
film_permits %>% 
    select(start_date_time, end_date_time, permit_duration_mins) %>% 
    summarize(mean_mins = sum(permit_duration_mins)/n())
```

    ## # A tibble: 1 x 1
    ##   mean_mins    
    ##   <drtn>       
    ## 1 1199.839 mins

``` r
    separate(
        col = start_date_time,
        into = c("date", "time"),
        sep = " ")
```
