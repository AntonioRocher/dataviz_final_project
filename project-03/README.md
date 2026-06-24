# Data Visualization and Reproducible Research

> Antonio Rocher-Hernandez

# Data Visualization Project 03

This project explores data visualization techniques using weather and concrete datasets. It focuses on distribution analysis, density-based plots, spatial visualization, and predictive modeling.

---

## Project Overview

The project is divided into two parts:

### Part 1: Weather Distribution Analysis (Tampa Airport 2022)
This section explores how temperature and precipitation vary across months using histograms, density plots, ridge plots, and boxplots.

### Part 2: Concrete Strength Analysis
This section investigates how material composition and age affect concrete compressive strength, along with predictive modeling and visualization improvements.

---

# Visualizations

## Part 1: Temperature and Precipitation Distributions

These visualizations show how daily maximum temperatures vary throughout the year in Tampa.

- Histograms and density plots reveal seasonal shifts in temperature distribution.
- Faceted density plots highlight how temperature distributions change month by month.
- Ridge plots provide a clearer comparison of temperature distributions across months.
- A boxplot visualizes precipitation patterns and variability across months.

---

## Part 2: Concrete Strength Analysis

### 1. Distribution of Material Components

This section explores distributions of cement and water content in concrete mixtures. The analysis shows that values are concentrated within expected engineering ranges, indicating structured experimental design.

---

### 2. Concrete Strength vs Age

This visualization shows how concrete compressive strength increases as concrete ages. Younger concrete has lower strength, while older samples tend to be stronger, though variability exists due to mixture composition.

---

### 3. Multi-variable Relationship (Interactive)

This is an interactive visualization showing relationships between cement content, water content, age, and concrete strength.

Users can hover over points to view exact values for each observation.

This makes it easier to understand relationships that are difficult to interpret in a static scatterplot.

---

# Chart Redesign

### Original Chart Issue

One of the original visualizations had some design limitations:
- No reference point for interpretation
- Limited context for distribution shape
- Minimal visual guidance for interpretation
- Basic styling that made comparison harder

---

### Improved Chart

The redesigned version improves clarity and interpretability by:

- Adding a mean reference line for context
- Improving color contrast for readability
- Enhancing axis labels and titles
- Reducing unnecessary visual clutter
- Using a more structured layout

---

## Key Findings

- Temperature distributions in Tampa shift significantly by season.
- Concrete strength increases with age but depends heavily on mixture composition.
- Higher cement and lower water content generally lead to stronger concrete.
- Data patterns suggest controlled experimental design rather than random sampling.

---

## Accessibility

All visualizations follow accessibility best practices:

- Colorblind-safe palettes are used for continuous variables  
- Clear labels and legends are included  
- Alternative text descriptions are provided for every figure  
- Information is not conveyed through color alone  
- Visual hierarchy is improved using spacing, contrast, and annotations  

---

## Conclusion

This project demonstrates how visualization techniques can reveal patterns in both environmental and engineering datasets. It also shows the importance of improving chart design through redesign, as well as enhancing interpretability through interactive visualization tools.
