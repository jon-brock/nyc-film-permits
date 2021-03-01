NYC Filming Permits
================

Data available
[here](https://data.cityofnewyork.us/City-Government/Film-Permits/tg4x-b46p).
The data pulled for this analysis occurred on February 24, 2021.

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
        permit_duration_mins = end_date_time - start_date_time) %>% 
    select(-c(event_agency, parking_held))
```

``` r
head(film_permits)
```

    ## # A tibble: 6 x 13
    ##   event_id event_type                    start_date_time     end_date_time      
    ##      <dbl> <chr>                         <dttm>              <dttm>             
    ## 1   446040 Shooting Permit               2018-10-19 14:00:00 2018-10-20 04:00:00
    ## 2   446168 Shooting Permit               2018-10-19 14:00:00 2018-10-20 02:00:00
    ## 3   186438 Shooting Permit               2014-10-30 07:00:00 2014-10-31 02:00:00
    ## 4   445255 Shooting Permit               2018-10-20 07:00:00 2018-10-20 18:00:00
    ## 5   128794 Theater Load in and Load Outs 2013-11-16 00:01:00 2013-11-17 06:00:00
    ## 6    43547 Shooting Permit               2012-01-10 07:00:00 2012-01-10 19:00:00
    ## # … with 9 more variables: entered_on <dttm>, borough <chr>,
    ## #   community_board_s <chr>, police_precinct_s <chr>, category <chr>,
    ## #   sub_category_name <chr>, country <chr>, zip_code_s <chr>,
    ## #   permit_duration_mins <drtn>

------------------------------------------------------------------------

``` r
film_permits %>% 
    select(event_type, start_date_time, end_date_time, permit_duration_mins) %>% 
    group_by(event_type) %>% 
    summarize(mean_mins = (round(sum(permit_duration_mins)/n()))) %>% 
    mutate(mean_hours = (round(mean_mins / dhours(1)))) %>% 
    knitr::kable(
        align = 'lcc',
        col.names = c("Event Type", "Average Permit Time (m)", "Average Permit Time (h)"))
```

| Event Type                    | Average Permit Time (m) | Average Permit Time (h) |
|:------------------------------|:-----------------------:|:-----------------------:|
| DCAS Prep/Shoot/Wrap Permit   |        866 mins         |           14            |
| Rigging Permit                |        1273 mins        |           21            |
| Shooting Permit               |        1007 mins        |           17            |
| Theater Load in and Load Outs |        3012 mins        |           50            |
