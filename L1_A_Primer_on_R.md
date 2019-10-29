---
title: "A Primer on R (and RStudio)"
sidebar: toc
category: getting-started
order: 1
---

# What is R?

**1990**: R appeared as a free implementation of a dialect of the _S language_. R was used as programming language for the undergraduate statistics courses at the University of Auckland, New Zealand.

**1995**: R's source codes were made available under _GNU General Public License version 2.0_. 

**1997**: R files (sources, binaries and documentation) were made available for download at the _Comprehensive R Archive Network_ (CRAN). 

**2000**: R 1.0.0 (No. extensions: 3)

**2018**: R. 3.5.1 (No. extensions: 13,458)

**Today**: R 3.6.1 (No. extensions: 16,000 +) 

> Today, R is probably the most widely used open-source software environment for data analysis and statistical graphics in academia and business. 

> With over 16.000 extensions the statistical and computational toolkit is huge making the R programming language the lingua franca of data science.

# Why R?

* Very intuitive syntax.
* Easy to program.
* Sophisticated graphics capabilities.
* Offers functionality for a large number of statistical procedures.
* Easy to extend (also to other software environments and plattforms). 
* **It's free**.

# The R language design

- R is an interpreted language.
- Interaction with R takes place at the command prompt **`>`**. 
- R interprets and executes the command, returns the output. 
- Output can be returned on the console, on the graphic device or to a file.

**Example:** Type 2+2 at the R command prompt, and press enter. 

```{r}
2 + 2
```

**NOTE!** If a command is not completed at the end of a line, R will give a continuation prompt **`+`**, by default.

```{r}
2 + 
  2
```

Solution: Finish the command or hit **`Esc`**.

## Functions

- R has a pure functional core! Every computation happens by evaluating functions.
- A Function consists of: 

1. Function name **`function.name`**
2. Function call `()`
3. Function _`arguments`_ (can be variables, data, default values or other functions).

```{r}
cat("function.name(argument1 = var.x, argument3 = data, argument4 = TRUE,  ...)")
```

* The function call passes arguments by **position** or by **name**. If any argument is passed by name the order in which it appears is irrelevant.

* Arguments with **default values** are omitted. If such an argument is not specified, R takes the default value.

**Example:** The linear model function **`lm()`** for fitting a linear regression model.

~~~
lm(formula, data, subset, weights, na.action, method = 'qr',
   model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE,
   contrasts = NULL, offset, ...)
~~~

Regressing `Temp` on `Ozone` from R's `airquality` data set (daily air quality measurements in New York, May to September 1973).

**Option (1):** Pass required arguments  **by position**.

```{r}
lm(Ozone ~ Temp, airquality)
```

**Option (2):** Pass required arguments **by name**.

```{r}
lm(data = airquality, formula = Ozone ~ Temp)
```

**Example:** The boxplot function **`boxplot()`** for producing a box-and-whisker plot.

~~~
boxplot(x, ..., range = 1.5, width = NULL, varwidth = FALSE, notch = FALSE, 
       outline = TRUE, names, plot = TRUE, border = par('fg'), col = NULL, 
       log = '', pars = list(boxwex = 0.8, staplewex = 0.5, outwex = 0.5), 
       horizontal = FALSE, add = FALSE, at = NULL)")
~~~

Producing a horizontal boxplot in gray of the monthly numbers of sunspots from R's `sunspoth.month` data set. 

**Options (1) + (2):** Pass arguments **by name and position**.

```{r}
boxplot(sunspot.month, col="gray", horizontal = TRUE)
```

NOTE! It is easier to remember the name of an argument than its position. If you can not remember the name or the position you can get help by using **`?function.name`** or **`help(function.name)`**.

## Actions

1(a). **Plotting**. Producing graphical output (e.g., creating a plot in the graphic device).

1(b). **Printing**. Producing printed output (e.g., returning results on the console) 

2. **Assignments**. Assign the output to a name.

- R operates on named data structures. The assignment operator `<-` points to the name receiving the value (Note: the assignment operator consists of the two characters `<` "less than" and `-` "minus").
- R names are unlimited in length.
- R names allow all alphanumeric symbols plus `.` and `_` (only restriction: if a name starts with `.` the second character is not allowed to be a digit.)
- R names do not allow special symbols like `^`,`!`, `$`, `@`, `+`, `-`, `/`, `*`.

**Example**: Calculate the mean of the monthly numbers of sunspots and name the result `mean.sunspot`.

```{r}
mean.sunspot <- mean(sunspot.month)
```

Print out the result on the console.

`> Mean.sunspot`

```{r}
cat("Error: object 'Mean.sunspot' not found")
```

NOTE! R is **case sensitive**! Capitalisation does matter. Here, `M` and `m` are different symbols and refer to different objects.

3. **Subsetting**. Extracting elements from an object. 

**Example**: Investigating R's `airquality` data set.

```{r}
head(airquality)
```

Select all observation days with more than 20 mph of windspeed. 

```{r}
subset(airquality, subset=Wind>20)
```
