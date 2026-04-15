# 🌸 R Shiny Interactive Species Classifier

An interactive R Shiny application that classifies iris species using a **rule-based spatial classification engine** built on convex hull geometry and polygon containment logic. Users input flower measurements and the app determines species membership by checking whether the point falls within the convex hull of each species' known measurement space.

---

## What It Actually Does

This is more than an EDA app — it implements a **custom geometric classifier** without using a pre-built ML model:

- Computes **convex hulls** for each species across all measurement combinations
- Uses **spatial point-in-polygon detection** (`gContains` via the `rgeos` package) to determine species membership
- Handles **overlapping regions** with explicit set logic (intersection, union, difference of species polygons)
- Returns classification decisions with **uncertainty flagging** when a point falls in an ambiguous boundary region

This mimics how a rule-based classification system works in practice — useful context for pricing, underwriting, and risk-tiering applications in insurance and healthcare analytics.

---

## App Features

### 1 · Interactive Species Classifier
- Select any two measurement axes (Sepal Length, Sepal Width, Petal Length, Petal Width)
- Input a custom measurement point
- App returns the predicted species and renders the classification plot with the user's point overlaid on species regions

### 2 · Multi-Panel Exploratory Dashboard
For each species (Iris-setosa, Iris-versicolor, Iris-virginica) across both Sepal and Petal dimensions:
- Scatter plot with LOESS smoothing and annotated mean lines
- Marginal histograms (X and Y axes)
- Boxplots per dimension
- All panels arranged in a custom `grid.arrange` layout

### 3 · Summary Statistics Tables
- Mean and standard deviation by species and measurement using `formattable` with color-scaled bars
- Species distribution percentages

---

## Classification Logic

The classifier uses set-based spatial reasoning across three species regions:

```
R = Iris-setosa convex hull
G = Iris-versicolor convex hull  
B = Iris-virginica convex hull
```

Point classification outcomes:
| Condition | Result |
|-----------|--------|
| Point in R | Iris-setosa |
| Point in G only | Iris-versicolor |
| Point in B only | Iris-virginica |
| Point in G ∩ B (overlap) | Ambiguous — versicolor or virginica |
| Point in G ∩ B, on actual data point | Definitive classification |
| Point outside all regions | Not iris species |

The `master.csv` file contains the extended dataset used for boundary computation.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| R / Shiny | Interactive web application |
| ggplot2 | Multi-panel visualization |
| rgeos / sp | Spatial geometry and point-in-polygon detection |
| plyr | Convex hull computation per group |
| gridExtra | Custom multi-panel layout |
| formattable | Color-scaled summary tables |
| plotly | Interactive chart rendering |

---

## How to Run

```r
# Install dependencies
install.packages(c("shiny", "ggplot2", "dplyr", "reshape2", "formattable",
                   "gridExtra", "ggExtra", "plotly", "rgeos", "sp", "plyr"))

# Clone and run
git clone https://github.com/ummuglsm0134/R-Shiny-App-Project
# Open RShinyApp.Rmd in RStudio and knit, or run the Shiny app directly
```

---

## Key Takeaways

- Rule-based spatial classifiers are interpretable and auditable — important properties in regulated domains like healthcare and insurance
- Convex hull boundary logic naturally surfaces uncertainty at class boundaries, rather than forcing a hard classification
- The same geometric approach applies to higher-dimensional risk segmentation problems (e.g., identifying which customers fall in overlapping risk tiers)

---

## Author

**Lia Arslan** — Data Scientist  
[github.com/ummuglsm0134](https://github.com/ummuglsm0134) · [linkedin.com/in/uarslan](https://linkedin.com/in/uarslan)
