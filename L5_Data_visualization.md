# The data science process

Data visualisation is an important tool for generating and communicating insights.

<br>

**Exploratory graphs**: 

* Describing data, detecting patterns and trends.

* Produced instantly using default settings.

**Illustrative graphs**: 

* Illustrating a conclusion, making a convincing argument.

* Require changing graph titles, axis, labels, colors, symbols, adding legends, ... or adding information.

R includes at least three graphical systems: 

1. The **standard graphics** package, 
2. The **lattice** package for Trellis graphs and 
3. The **ggplot2** package based on the idea of the grammar-of-graphics.

<br>

In R graphs are build-up in two stages by successively calling graph functions:

1. Creating an (**exploratory**) default graph.
2. **Customizing** and **annotating** the default graph. 

<br>

# Exploratory graphs

**Example**: _Bike sharing Chicago_.

www.divvybikes.com covers information about 9.5 Million bike trips in Chicago. The data set is available in an aggregated version as `Chicago.agg.csv`.

```{r}
Chicago.agg <- read.csv("Chicago.agg.csv")
```

```{r}
str(Chicago.agg)
```

(Default) histogramm:

```{r}
hist(Chicago.agg$tripduration)
```

(Default) boxplot:

```{r}
boxplot(tripduration ~ events, data = Chicago.agg)
```

(Default) scatterplot:

```{r}
plot(tripduration ~ temperature, data = Chicago.agg)
```

## How to select among different graph types?

