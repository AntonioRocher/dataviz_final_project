---
title: "Data Visualization - Mini-Project 1"
author: "Antonio Rocher-Hernandez `arocherhernandez132@floridapoly.edu`"
output:
  html_document:
    keep_md: true
    df_print: paged
---

# Libraries


``` r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.2.1     ✔ readr     2.2.0
## ✔ forcats   1.0.1     ✔ stringr   1.6.0
## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
## ✔ purrr     1.2.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(lubridate)

traffic <- read_csv("../data/trafficMN.csv")
```

```
## Rows: 48204 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): holiday, weather_main, weather_description
## dbl  (5): temp, rain_1h, snow_1h, clouds_all, traffic_volume
## dttm (1): date_time
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

# Exploring the Data

## Data Structure


``` r
glimpse(traffic)
```

```
## Rows: 48,204
## Columns: 9
## $ holiday             <chr> "None", "None", "None", "None", "None", "None", "N…
## $ temp                <dbl> 288.28, 289.36, 289.58, 290.13, 291.14, 291.72, 29…
## $ rain_1h             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
## $ snow_1h             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
## $ clouds_all          <dbl> 40, 75, 90, 90, 75, 1, 1, 1, 20, 20, 20, 1, 1, 1, …
## $ weather_main        <chr> "Clouds", "Clouds", "Clouds", "Clouds", "Clouds", …
## $ weather_description <chr> "scattered clouds", "broken clouds", "overcast clo…
## $ date_time           <dttm> 2012-10-02 09:00:00, 2012-10-02 10:00:00, 2012-10…
## $ traffic_volume      <dbl> 5545, 4516, 4767, 5026, 4918, 5181, 5584, 6015, 57…
```

## Summary


``` r
traffic_summary <- traffic %>%
  summarize(
    Observations = n(),
    Avg_Traffic = round(mean(traffic_volume, na.rm = TRUE), 2),
    Min_Traffic = min(traffic_volume, na.rm = TRUE),
    Max_Traffic = max(traffic_volume, na.rm = TRUE),
    Avg_Temperature = round(mean(temp, na.rm = TRUE), 2)
  )

traffic_summary
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Observations"],"name":[1],"type":["int"],"align":["right"]},{"label":["Avg_Traffic"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Min_Traffic"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Max_Traffic"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Avg_Temperature"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"48204","2":"3259.82","3":"0","4":"7280","5":"281.21"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Missing Values


``` r
colSums(is.na(traffic))
```

```
##             holiday                temp             rain_1h             snow_1h 
##                   0                   0                   0                   0 
##          clouds_all        weather_main weather_description           date_time 
##                   0                   0                   0                   0 
##      traffic_volume 
##                   0
```
### Discussion

The dataset has data on traffic volume measurements, weather, and dates. There are no missing values.

# Data Preparation


``` r
traffic <- traffic %>%
  mutate(
    date_time = ymd_hms(date_time),
    hour = hour(date_time),
    weekday = wday(date_time, label = TRUE)
  )
```

```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `date_time = ymd_hms(date_time)`.
## Caused by warning:
## !  2037 failed to parse.
```

I made some additional variable to simplify the analysis

# Graph 1: Traffic Volume by Hour


``` r
hourly_traffic <- traffic %>%
  group_by(hour) %>%
  summarize(avg_traffic = mean(traffic_volume))

ggplot(hourly_traffic,
       aes(hour, avg_traffic)) +
  geom_line() +
  geom_point() +
  labs(
    title = "Average Traffic Volume by Hour",
    x = "Hour of Day",
    y = "Average Traffic Volume"
  ) +
  theme_minimal()
```

<img src="[https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_facet.pn](https://github.com/AntonioRocher/dataviz_final_project/blob/215d2364fcdfce284a44b3ca7a20335eabdadd15/Figures/Avg_Traffic_Volume_by_Hour.png)" width="80%" style="display: block; margin: auto;" />

```
## Warning: Removed 1 row containing missing values or values outside the scale range
## (`geom_line()`).
```

```
## Warning: Removed 1 row containing missing values or values outside the scale range
## (`geom_point()`).
```

![](Project1_analysis_data_viz_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
### Interpretation

It appears that traffic volume increases during the morning and reaches the maximum during the evening. The volume decreases as midnight approaches.

# Graph 2: Traffic Volume by Weekday


``` r
weekday_traffic <- traffic %>%
  group_by(weekday) %>%
  summarize(avg_traffic = mean(traffic_volume))

ggplot(weekday_traffic,
       aes(weekday, avg_traffic)) +
  geom_col() +
  labs(
    title = "Average Traffic Volume by Weekday",
    x = "Weekday",
    y = "Average Traffic Volume"
  ) +
  theme_minimal()
```

![](Project1_analysis_data_viz_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

### Interpretation

According to the graph, weekdays generally show higher traffic volume than weekends, which can be supported by having to travel to work during weekdays.

# Graph 3: Traffic Volume by Weather Condition


``` r
weather_traffic <- traffic %>%
  group_by(weather_main) %>%
  summarize(avg_traffic = mean(traffic_volume))

ggplot(weather_traffic,
       aes(reorder(weather_main, avg_traffic),
           avg_traffic)) +
  geom_col() +
  coord_flip() +
  labs(
    title = "Average Traffic Volume by Weather",
    x = "Weather Condition",
    y = "Average Traffic Volume"
  ) +
  theme_minimal()
```

![](Project1_analysis_data_viz_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

### Interpretation

This graph shows how different weather conditions influence traffic volume. The traffic volumes seems to decrease the worse the weather is.

# Discussion

## What were the original charts you planned to create?

I planned to create charts showing traffic volume by hour, weekday, and weather conditions. I feel like these variables seemed most likely to reveal useful traffic patterns.

## What story could you tell with your plots?

The plots show that traffic volume follows predictable commuting patterns. Traffic is highest during the weekday on work hours and lower during the weekends. The weather conditions also seem to influence traffic levels.

## How did you apply the principles of data visualization and design?

I selected chart types that are appropriate for the data being shown. Line charts were used for trends based on time, while bar graphs were used for comparisons between categories. I added clear labels, titles, and a minimal theme to improve readability.

# Conclusion

This analysis shows how traffic volume changes based on time and environmental conditions. The graphs showed clear commuting patterns and highlighted factors that may affect traffic.
