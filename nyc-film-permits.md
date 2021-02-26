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
    select(-c(event_agency, parking_held, ))
```

``` r
head(film_permits) %>% knitr::kable()
```

| event\_id | event\_type                   | start\_date\_time   | end\_date\_time     | entered\_on         | borough   | community\_board\_s | police\_precinct\_s | category          | sub\_category\_name | country                  | zip\_code\_s | permit\_duration\_mins |
|----------:|:------------------------------|:--------------------|:--------------------|:--------------------|:----------|:--------------------|:--------------------|:------------------|:--------------------|:-------------------------|:-------------|:-----------------------|
|    446040 | Shooting Permit               | 2018-10-19 14:00:00 | 2018-10-20 04:00:00 | 2018-10-16 11:57:27 | Manhattan | 2                   | 1                   | Television        | Cable-episodic      | United States of America | 10012        | 840 mins               |
|    446168 | Shooting Permit               | 2018-10-19 14:00:00 | 2018-10-20 02:00:00 | 2018-10-16 19:03:56 | Manhattan | 12, 8               | 34, 50              | Film              | Feature             | United States of America | 10034, 10463 | 720 mins               |
|    186438 | Shooting Permit               | 2014-10-30 07:00:00 | 2014-10-31 02:00:00 | 2014-10-27 12:14:15 | Queens    | 2, 5                | 104, 108            | Television        | Episodic series     | United States of America | 11378        | 1140 mins              |
|    445255 | Shooting Permit               | 2018-10-20 07:00:00 | 2018-10-20 18:00:00 | 2018-10-09 21:34:58 | Brooklyn  | 2                   | 84                  | Still Photography | Not Applicable      | United States of America | 11201        | 660 mins               |
|    128794 | Theater Load in and Load Outs | 2013-11-16 00:01:00 | 2013-11-17 06:00:00 | 2013-11-07 15:48:28 | Manhattan | 4, 5                | 14                  | Theater           | Theater             | United States of America | 10001, 10121 | 1799 mins              |
|     43547 | Shooting Permit               | 2012-01-10 07:00:00 | 2012-01-10 19:00:00 | 2012-01-04 12:25:37 | Brooklyn  | 1, 2                | 108, 94             | Television        | Episodic series     | United States of America | 11101, 11222 | 720 mins               |

------------------------------------------------------------------------

``` r
film_permits %>% 
    select(event_type, start_date_time, end_date_time, permit_duration_mins) %>% 
    group_by(event_type) %>% 
    summarize(mean_mins = sum(permit_duration_mins)/n()) %>% 
    mutate(mean_hours = mean_mins / dhours(1)) %>% 
    knitr::kable(
        align = 'lcc',
        col.names = c("Event Type", "Average Permit Time (m)", "Average Permit Time (h)"))
```

| Event Type                    | Average Permit Time (m) | Average Permit Time (h) |
|:------------------------------|:-----------------------:|:-----------------------:|
| DCAS Prep/Shoot/Wrap Permit   |      866.1466 mins      |        14.43578         |
| Rigging Permit                |     1272.6995 mins      |        21.21166         |
| Shooting Permit               |     1007.0479 mins      |        16.78413         |
| Theater Load in and Load Outs |     3011.9991 mins      |        50.19998         |
