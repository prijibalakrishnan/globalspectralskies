Comparing spectral skies
================
Priji Balakrishnan
3/12/2020

We load the following libraries for our analysis.

``` r
library(tidyverse)
library(lubridate)
library(here)
```

We load the
data.

``` r
singapore_skies <- readRDS(here::here("resources/processed_data/singapore.rds"))
berlin_skies <- readRDS(here::here("resources/processed_data/berlin.rds"))
spectral_skies <- singapore_skies %>%
  bind_rows(berlin_skies) %>%
  arrange(loc, datetime, wv) %>%
  mutate(measurement = as_factor(str_c(loc, as.character(datetime), sep="-")))
```

    ## Warning in bind_rows_(x, .id): binding factor and character vector, coercing
    ## into character vector

    ## Warning in bind_rows_(x, .id): binding character and factor vector, coercing
    ## into character vector

All experiments:

``` r
levels(spectral_skies$measurement)
```

    ##  [1] "BER-2019-02-25 09:07:00" "BER-2019-02-25 12:00:00"
    ##  [3] "BER-2019-02-25 15:00:00" "SIN-2016-11-01 11:55:00"
    ##  [5] "SIN-2016-11-17 11:53:00" "SIN-2016-11-18 13:20:00"
    ##  [7] "SIN-2016-11-18 13:25:00" "SIN-2016-11-18 13:30:00"
    ##  [9] "SIN-2016-11-18 13:35:00" "SIN-2016-11-18 13:40:00"
    ## [11] "SIN-2016-11-18 13:50:00" "SIN-2016-11-18 13:55:00"
    ## [13] "SIN-2016-11-18 14:00:00" "SIN-2016-11-21 16:20:00"
    ## [15] "SIN-2016-11-21 16:25:00" "SIN-2016-11-21 16:30:00"
    ## [17] "SIN-2016-11-22 11:30:00" "SIN-2016-11-22 11:35:00"
    ## [19] "SIN-2016-11-22 11:40:00" "SIN-2016-11-22 11:45:00"
    ## [21] "SIN-2016-11-22 11:50:00" "SIN-2016-11-22 11:55:00"
    ## [23] "SIN-2016-11-22 12:00:00" "SIN-2016-12-07 08:00:00"
    ## [25] "SIN-2016-12-07 08:05:00" "SIN-2016-12-13 06:45:00"
    ## [27] "SIN-2016-12-13 06:50:00" "SIN-2016-12-13 06:55:00"
    ## [29] "SIN-2016-12-13 07:00:00" "SIN-2016-12-13 07:05:00"
    ## [31] "SIN-2016-12-13 07:10:00" "SIN-2016-12-16 19:00:00"
    ## [33] "SIN-2016-12-16 19:10:00" "SIN-2016-12-16 19:15:00"
    ## [35] "SIN-2017-04-06 18:07:00" "SIN-2018-02-13 11:22:00"
    ## [37] "SIN-2018-02-18 07:49:00" "SIN-2018-02-18 08:46:00"
    ## [39] "SIN-2018-02-18 18:42:00" "SIN-2018-05-01 07:56:00"
    ## [41] "SIN-2018-05-24 12:39:00" "SIN-2018-05-28 13:24:00"
    ## [43] "SIN-2018-07-21 14:01:00" "SIN-2018-07-25 18:46:00"
    ## [45] "SIN-2018-07-26 07:50:00" "SIN-2018-07-28 15:11:00"
    ## [47] "SIN-2018-08-11 18:45:00"

The following notebook expects data according to the following schema:

  - `MonthYear`: date format
  - `Time`: time format
  - `wv`
  - `spd`
  - `SkyType`
  - `CCT`
  - `loc`

## Comparison between different Singapore morning skies

We subset the data to get measurements in Singapore during the morning.

``` r
singapore_morning <- spectral_skies %>%
  filter(loc == "SIN", hour(datetime) <= 9)
```

``` r
singapore_morning %>%
  ggplot() +
  geom_line(aes(x = wv, y = spd, group = measurement, color = measurement))
```

![](comparing_spectral_skies_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

## Comparison between Singapore and Berlin
