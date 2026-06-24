# Data Visualization 

> Antonio Rocher-Hernandez 

## Mini-Project 2

This project explores weather patterns in the Atlanta area using temperature, humidity, cloud cover, precipitation, and predictive modeling data. The goal is to identify relationships between weather variables, visualize regional precipitation patterns, and evaluate the accuracy of a weather prediction model.

---

## Project Overview

The analysis focuses on three main questions:

* How are temperature, humidity, and cloud cover related?
* How do precipitation anomalies vary across Metro Atlanta throughout the year?
* How accurately can weather variables predict daily high temperatures?

---

## Visualizations

### Plot 1: Temperature vs. Humidity (Interactive)

This interactive scatterplot explores the relationship between humidity and daily maximum temperatures. Cloud cover is represented using a viridis palette.

The interactive features allow users to hover over individual points to view the date, temperature, and humidity values. This provides a level of detail that cannot be easily displayed in a static chart.

---

### Plot 2: Regional Distribution of Precipitation Anomalies

This map visualizes precipitation intensity anomalies across Metro Atlanta counties during different quarters of the year. The layout makes it easy to compare seasonal weather patterns and identify periods of increased storm activity.

---

### Plot 3: Predictive Model Evaluation

This visualization compares predicted daily high temperatures against observed temperatures. Residual errors are highlighted using a diverging color scale, allowing viewers to identify where the model performs well and where prediction errors are larger.

---

## Key Findings

* Higher temperatures generally occur on lower-humidity days.
* Precipitation anomalies vary across seasons, with stronger rainfall events occurring during spring and late summer.
* The predictive model performs well overall but shows larger errors during cooler weather periods.

---

## Accessibility

All visualizations were designed with accessibility in mind:

* Colorblind-safe palettes such as Viridis and Plasma were used where appropriate.
* Clear labels, titles, and legends improve readability.
* Alternative text descriptions are included in the source document using the `fig.alt` option.
* Information is not communicated through color alone whenever possible.

---

## Conclusion

This project demonstrates how weather data can be explored through both static and interactive visualizations. The combination of exploratory analysis, spatial mapping, and predictive model evaluation provides insight into Atlanta's weather patterns and the effectiveness of weather-based prediction models.
