---
title: "Data Visualization for Exploratory Data Analysis"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 03


In this exercise you will explore methods to create different types of data visualizations (such as plotting text data, or exploring the distributions of continuous variables).


## PART 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) for 2022, attempt to recreate the charts shown below which were generated using data from 2016. You can read the 2022 dataset using the code below: 


``` r
library(tidyverse)
library(lubridate)
library(ggridges)
library(viridis)
weather_tpa <- read_csv("https://raw.githubusercontent.com/aalhamadani/datasets/master/tpa_weather_2022.csv")
# random sample 
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022    12    12       0             76       59     67.5
## 2  2022     3    10       0.00001       84       72     78  
## 3  2022     9     3       0.4           93       76     84.5
## 4  2022     6     8       0             92       81     86.5
```

See Slides from Week 4 of Visualizing Relationships and Models (slide 10) for a reminder on how to use this type of dataset with the `lubridate` package for dates and times (example included in the slides uses data from 2016).

Using the 2022 data: 

(a) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_facet.png" alt="" width="80%" style="display: block; margin: auto;" />



``` r
weather_tpa <- weather_tpa %>%
  mutate(
    date = make_date(year, month, day),
    month_name = factor(
      month(date, label = TRUE, abbr = FALSE),
      levels = month.name
    )
  )

ggplot(weather_tpa, aes(x = max_temp)) +
  geom_histogram(
    binwidth = 3,
    color = "white",
    fill = "steelblue"
  ) +
  facet_wrap(~month_name, ncol = 4) +
  labs(
    x = "Maximum Temperature (°F)",
    y = "Count"
  ) +
  theme_bw()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-3-1.png)<!-- -->



Hint: the option `binwidth = 3` was used with the `geom_histogram()` function.

(b) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_density.png" alt="" width="80%" style="display: block; margin: auto;" />



``` r
ggplot(weather_tpa, aes(x = max_temp)) +
  geom_density(
    kernel = "gaussian",
    bw = 0.5,
    fill = "skyblue",
    alpha = 0.6
  ) +
  labs(
    title = "Density of Daily Maximum Temperatures",
    x = "Maximum Temperature (°F)",
    y = "Density"
  ) +
  theme_minimal()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


Hint: check the `kernel` parameter of the `geom_density()` function, and use `bw = 0.5`.

(c) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_density_facet.png" alt="" width="80%" style="display: block; margin: auto;" />



``` r
ggplot(weather_tpa, aes(x = max_temp)) +
  geom_density(fill = "orange", alpha = 0.6) +
  facet_wrap(~month_name) +
  theme_minimal()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


Hint: default options for `geom_density()` were used. 

(d) Generate a plot like the chart below:


<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_ridges_plasma.png" alt="" width="80%" style="display: block; margin: auto;" />



``` r
library(ggridges)
library(viridis)

ggplot(weather_tpa,
       aes(x = max_temp,
           y = month_name,
           fill = stat(x))) +
  geom_density_ridges_gradient(
    quantile_lines = TRUE,
    quantiles = c(0.25, 0.5, 0.75)
  ) +
  scale_fill_viridis_c(option = "plasma") +
  theme_ridges()
```

```
## Warning: `stat(x)` was deprecated in ggplot2 3.4.0.
## ℹ Please use `after_stat(x)` instead.
## This warning is displayed once per session.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## Picking joint bandwidth of 1.93
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


Hint: use the`{ggridges}` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters. The plot above uses the `plasma` option (color scale) for the _viridis_ palette.


(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.



``` r
ggplot(weather_tpa,
       aes(x = month_name,
           y = precipitation)) +
  geom_boxplot(fill = "lightblue") +
  labs(
    title = "Monthly Precipitation Distribution",
    x = "Month",
    y = "Precipitation"
  ) +
  theme_minimal()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-10-1.png)<!-- -->



## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on. 

### Option (B): Data on Concrete Strength 

Concrete is the most important material in **civil engineering**. The concrete compressive strength is a highly nonlinear function of _age_ and _ingredients_. The dataset used here is from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), and it contains 1030 observations with 9 different attributes 9 (8 quantitative input variables, and 1 quantitative output variable). A data dictionary is included below: 


Variable                      |    Notes                
------------------------------|-------------------------------------------
Cement                        | kg in a $m^3$ mixture             
Blast Furnace Slag            | kg in a $m^3$ mixture  
Fly Ash                       | kg in a $m^3$ mixture             
Water                         | kg in a $m^3$ mixture              
Superplasticizer              | kg in a $m^3$ mixture
Coarse Aggregate              | kg in a $m^3$ mixture
Fine Aggregate                | kg in a $m^3$ mixture      
Age                           | in days                                             
Concrete compressive strength | MPa, megapascals


Below we read the `.csv` file using `readr::read_csv()` (the `readr` package is part of the `tidyverse`)


``` r
concrete <- read_csv("../data/concrete.csv", col_types = cols())
```


Let us create a new attribute for visualization purposes, `strength_range`: 


``` r
new_concrete <- concrete %>%
  mutate(strength_range = cut(Concrete_compressive_strength, 
                              breaks = quantile(Concrete_compressive_strength, 
                                                probs = seq(0, 1, 0.2))) )
```



1. Explore the distribution of 2 of the continuous variables available in the dataset. Do ranges make sense? Comment on your findings.



``` r
ggplot(concrete, aes(Cement)) +
  geom_histogram(bins = 30,
                 fill = "steelblue",
                 color = "white") +
  theme_minimal()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-13-1.png)<!-- -->



``` r
ggplot(concrete, aes(Water)) +
  geom_histogram(bins = 30,
                 fill = "darkgreen",
                 color = "white") +
  theme_minimal()
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

Comment:

Standard concrete mixes need anywhere from 150 kg/m³ to 450 kg/m³ of cement depending on the target strength. The lower bound is just under 100, which refers to very lean mixes, making this entire range reasonable for a concrete dataset. In concrete mix designs, water content goes from 140 to 210 kg/m³. The nature of the Cement plot and the spikes on the Water plot indicate that this data comes from structured mix designs instead of random environmental sampling.


2. Use a _temporal_ indicator such as the one available in the variable `Age` (measured in days). Generate a plot similar to the one shown below. Comment on your results.

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/concrete_strength.png" alt="" width="80%" style="display: block; margin: auto;" />



``` r
ggplot(new_concrete,
       aes(
         x = factor(Age),
         y = Concrete_compressive_strength,
         fill = strength_range
       )) +
  geom_boxplot() +
  labs(
    title = "Concrete Compressive Strength by Age",
    x = "Age (in days)",
    y = "Compressive Strength (in MPa)",
    fill = "Strength Range"
  )
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
Comment:

The plot shows that concrete strength increases as the concrete gets older. The lowest strength values are mostly seen during the first few days, while higher strength values become more common at later ages. This makes sense because concrete continues to gain strength over time.

There is also some variation in strength at the same age. This represents the importance of age, but other factors such as the ingredients used in the concrete mix can also affect the final strength. The graph shows a positive relationship between age and compressive strength, with older concrete tending to be stronger than younger concrete.

3. Create a scatterplot similar to the one shown below. Pay special attention to which variables are being mapped to specific aesthetics of the plot. Comment on your results. 

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/cement_plot.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(new_concrete,
       aes(x = Cement,
           y = Concrete_compressive_strength,
           color = Water,                     
           size = Age)) +
  geom_point(alpha = 0.6) +                
  scale_color_viridis_c(option = "viridis") + 
  scale_size_continuous(range = c(1, 8)) +   
  labs(
    title = "Exploring Strength versus (Cement, Age, and Water)",
    x = "Cement",
    y = "Strength",
    color = "Water",
    size = "Age",
    caption = "Age is measured in days"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 16, hjust = 0), 
    plot.caption = element_text(hjust = 1, size = 10) 
  )
```

![](Rocher-Hernandez_project_03_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

Comment:

This plot shows the relationship between concrete, ingredients, and time. More cement gives you a much stronger mix, but only if you give it time, which is why the most points with age 300 are at the top. The highest strength cement are all dark purple and blue because keeping the water low makes the concrete dense and tough. If you throw in too much water, it doesn't matter how much cement you add; the mix gets diluted, it cannot get any stronger. One can see that the data is stacked in vertical lines, meaning this was a lab experiment where scientists were systematically testing these exact recipes to find the perfect balance of ingredients and time for the concrete.

