---
title: "Grammar of Graphics"
sidebar: toc
category: getting-started
order: 1
---

Graphs are forms of communication:

- **Analysis**: describing data, detecting patterns and trends (default settings often give reasonable results)
- **Communication**: illustrating a conclusion (should be directly readable and often require changing graph titles, axis, labels, colors, symbols, adding legends, ...)

One of the main advantages of R is its strong graphic capabilities!

# Standard graphs

* Standard graphs in the `base` system are created by calling successive R functions to "build up" a graph in two stages:

1. **Creating** of a (customized) plot
2. **Annotating** of a plot (adding lines, points, text, legends)

**Example**: Cereals

```{r}
cereals <- read.csv("cereal.csv")  # read-in the cereals data set
```

Histogramm
```{r}
hist(cereals$rating)    # Histogramm of ratings
```

Scatterplot
```{r}
plot(calories ~ rating, data = cereals)  # Scatterplot of calories against rating
```

Boxplot
```{r}
boxplot(rating ~ mfr, data = cereals)    # Boxplot of rating vs. manufacturer
```

other examples: `barplot()`, `piechart()`, `dotplot()`, ...

## Customizing standard graphs

Controlling (global) graphic parameters (colors, point symbols, line styles, labels and titles).

### Histogramm

 * customizing bar color and the title
 
```{r}
hist(cereals$rating, col = "gray")
```

* customizing the title

```{r}
hist(cereals$rating, main = "Histogramm of cerals ratings")
```

* customizing the x axis label

```{r}
hist(cereals$rating, xlab = "Rating")
```

* customizing the y axis scale

```{r}
hist(cereals$rating, freq = FALSE)
```

### Scatterplot

* customizing the plotting symbol color

```{r}
plot(calories ~ rating, col = "#848482", data = cereals)
```

* customizing the plotting symbol (plotting character)

```{r}
plot(calories ~ rating, pch = 19, data = cereals)
```

* changing the plotting symbol size

```{r}
plot(calories ~ rating, data = cereals, cex = 2)
```

* changing the x and y range (limits: from = ?, to = ?)

```{r}
plot(calories ~ rating, xlim = c(0,100), ylim = c(0,200), data = cereals)
```

### Boxplot

* changing the box color

```{r}
boxplot(rating ~ mfr, col = "slateblue", data = cereals)
```

```{r}
boxplot(rating ~ mfr, col = c("slategray","slateblue"), data = cereals)
```
**NOTE**: The box color vector is *recycled*!

* changing the tick labels (A: American Home Food Products; G: General Mills; K: Kelloggs; N: Nabisco; P: Post; Q: Quaker Oats; R: Ralston Purina)

```{r}
boxplot(rating ~ mfr, names = c("A.H.F.P.","Mills","Kellogs","Nabisco","Post","Quaker","Purina"),
        data = cereals)
```

## More customizing

The function `par()` can be used to specify the global graphics parameters that affect **all** plots in the active R session.

* Label text perpendicular to axis

```{r}
par(las = 3) 
boxplot(rating ~ mfr, names = c("A.H.F.P.","Mills","Kellogs","Nabisco","Post","Quaker", "Purina"),
       data = cereals)
```

* decrease the top margin (as no title is provided)

```{r}
par(mar = c(5,4,1,2))
boxplot(rating ~ mfr, 
        names = c("A.H.F.P.","Mills","Kellogs","Nabisco","Post","Quaker", "Purina"),
        data = cereals)
```

## Colors in R

* Built-in 657 named colors:

```{r}
colors()
```

![](./Ressources/colors.png)

* R uses **hexadecimal** (a base-16 number system) to represent colors (hex model also translates to RGB, HSV, HCL color models), see also:
   + [latexcolor.com](http://latexcolor.com)

* **Color palettes**:
   + [ColorBrewer.org](http://colorbrewer2.org)

Built-in color palettes
```{}
n <- 10
rainbow(n)
heat.colors(n)
terrain.colors(n)
topo.colors(n)
cm.colors(n)
```

ColorBrewer palettes
```{r}
library("RColorBrewer")
display.brewer.all()
```
1. **Sequential palettes** for ordered data that progress from low to high 
2. **Diverging palettes** put emphasis on mid-range critical values and extremes at both ends of the data range. 
3. **Qualitative palettes** for nominal or categorical data (they do not imply differences between groups).

Wes Anderson movie palettes
```{r}
library(wesanderson) 
wes_palettes
```

Do-it-yourself color palettes
```{r}
colorRampPalette(c("white", "gray30"))(10)  # (interpolating a 'sequential' palette)
```

## Annotating an existing standard graph

### Scatterplot

* Adding text to a (customized) scatterplot

```{r}
# Creating a color vector for the data points
mfr.col <- ifelse(cereals$mfr == "A", "#66c2a5", ifelse(cereals$mfr == "G", "#fc8d62",
           ifelse(cereals$mfr == "K", "#8da0cb", ifelse(cereals$mfr == "N", "#e78ac3", 
           ifelse(cereals$mfr == "P", "#a6d854", ifelse(cereals$mfr == "Q", "#ffd92f",
           ifelse(cereals$mfr == "R", "#e5c494", NA)))))))

# Avoid "ties" using jitter (adds a random amount of noise to the variable)
cereals$calories.jitter <- jitter(cereals$calories, amount = 10)

# Scatterplot of rating against the jittered calories
plot(calories.jitter ~ rating, col = mfr.col, 
     main = "Scatterplot of cereals", xlim = c(0,100), ylim = c(0,200), 
     cex = 2, pch = 19, data = cereals)
     
# Adding the manufacturer label to the points
text(cereals$rating, cereals$calories.jitter, labels=cereals$mfr)

# Alternative: Identify the cereals brand name
identify(cereals$rating, cereals$calories.jitter, labels = cereals$name, plot = TRUE)
```

* Adding a (fitted regression) line to the scatterplot

```{r}
linear.model <- lm(calories ~ rating, data = cereals)
abline(coef(linear.model))
```

* Adding text to the (fitted regression) line in the scatterplot

```{r}
text(40, 160, paste("y =",round(coef(linear.model)[1],2),"* calories + (",round(coef(linear.model)[2],2),"* rating)"))
```

### Boxplot

* Adding data points to a customized boxplot

```{r}
boxplot(rating ~ mfr, col = rainbow(7), 
       names = c("A.H.F.P.","Mills","Kellogs","Nabisco","Post","Quaker", "Purina"), las = 3,
       data = cereals)
points(rating ~ mfr, pch = 19, col = "slategray", data = cereals)
```

* Adding a legend to the boxplot

```{r}
legend("topleft", legend = c("A.H.F.P.","Mills","Kellogs","Nabisco","Post","Quaker", "Purina"), 
       bty = "n", fill = rainbow(7))
```

# The Grammer of Graphics

* Grammar to describe and construct statistical graphs; based on the idea of building up a graph by semantic components.

* `ggplot2` is an R library that allows to build graphical features up in a series of layers:
 1. **aesthetic** mapping of the data, defines how variables are connected to visual properties or outputs (e.g. color, size, shape).
 2. **geometric** objects representing the data.
 3. **coordinate systems**.
 4. **faceting** the data; splitting by some predefined criteria to display sup-graphs.
 5. **themes** to control non-data elements.
 6. **scales** map values in the data space to values in the aesthetic space (color, size, labels, ...) and are reported on the plot using axes and legends.

### Boxplot

```{r}
library(ggplot2)
```

* Supply the `cereal` dataset and aesthetic mapping with `aes()` to the function call `ggplot()`

```{r}
p <- ggplot(cereals, aes(x = mfr, y = rating)) + 
  geom_boxplot()    # Box plot of rating vs. manufacturer
p
```

```{r}
p <- ggplot(cereals, aes(x = mfr, y = rating)) + 
  geom_boxplot(outlier.colour = "red", outlier.shape = 8, outlier.size = 4) 
                     # Change outlier (color, shape and size)
p
```

```{r}
p + coord_flip()     # Add coordiante system flip (rotate the boxes)
```

```{r}
p + geom_jitter(shape = 16, position = position_jitter(0.1)) # Add jittered data points 
                      # (0.1 : degree of jitter in x direction)
```


```{r}
p <- ggplot(cereals, aes(x = mfr, y = rating, color = mfr)) +   
                      # Change box plot line colors by groups
  geom_boxplot() +
  labs(title = "Boxplot of cereals", x = "Manufacturer", y = "Rating")  
                      # Add scale labels and plot title
p
```

```{r}
p + scale_color_brewer(palette = "Dark2")  
                     # Add box line colors by groups using brewer color palettes
```

```{r}
p + scale_color_grey() + # Add box line colors by groups using grey scale
    theme_classic()  # Add classic plot theme
```

## (Useful) Ressources

* http://r4ds.had.co.nz/data-visualisation.html
* http://vita.had.co.nz/papers/layered-grammar.pdf
* [R Graphics Cookbook by Winston Chang](http://www.cookbook-r.com/Graphs/)
* [R Graphics by Paul Murrell](https://www.stat.auckland.ac.nz/~paul/RGraphics/rgraphics.html)
