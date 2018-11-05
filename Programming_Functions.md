---
title: "Programing 1: Functions"
author: "Kathrin Gruber"
date: ""
output: pdf_document
---

## Definition

```{r, eval=FALSE}
function_name <- function(argument) {
  statement
  }
```

**`function`**

- Reserved word (followed by parentheses) used to declare a function in R. 

**`argument`**

- Placeholders (optional values). 
- A function may contain no arguments, default values or a value is passed to the argument.

**`{ statement }`**

- Statements within the curly brace form the function body. 
- Everything between the braces is part of the body of the function. Defines what the function does.

**`function_name`**

- The actual name of the function.
- Stored in the R environment as an object with this name.

### Example

```{r}
foo <- function(x) {x^2}
foo(2)
```

The function `foo` returns the value `4`.

## Return values

- If there are no explicit returns from a function, the value of the last evaluated expression is returned automatically in R.

- Generally explicit `return()` functions are used to return a value immediately from a function (used in processing steps).

- The value returned from a function can be any valid object.

### Example

```{r}
z <- sample(c(1:100), 10) # generate 10 random samples from the sequences 1 to 100 
z
```

```{r}
foo <- function(x) {
  step1 <- x^2        # squarre the elements in x
  step2 <- sum(step1) # sum-up the squarred values
  return(step2)       # return step 2
}

foo(z)
```

```{r}
foo <- function(x) {
  step1 <- x^2        # squarre the elements in x
  step2 <- sum(step1) # sum-up the squarred values
  return(step1)       # return step 1
}

foo(z)
```

## Lists

The `return()` function can return only a single object. If we want to return multiple values in R, we can use a (named) `list` and return it.

```{r}
foo <- function(x) {
  step1 <- x^2        # squarre the elements in x
  step2 <- sum(step1) # sum-up the squarred values
  list(step1=step1, step2=step2)
}

foo(z)
```

- Lists are the R objects which contain elements of different types like scalars, vectors, matrices but also of different mode like numerics, logicals and characters.

- Like for `data.frames`, the `list` elements can be given names and they can be accessed using these names (otherwise they are accessed by the index of the element in the list via `[[ ]]`).

## Named arguments

- The function can also be called using named arguments.
- In a single function call named and unnamed arguments can be used togehter.

### Example

```{r, results='hide'}
foo <- function(x, power=power) {
  step1 <- x^power    # raise the elements in x to the power of y
  step2 <- sum(step1) # sum-up the squarred values
  list(step1=step1, step2=step2)
}

y <- 2
foo(z, y)
foo(z, power=y)
```

## Default values

- Provide an appropriate value to the formal argument in the function declaration.
- The use of a default value to an argument makes it optional when calling the function.

### Example

```{r, results='hide'}
foo <- function(x, power=2) {
  step1 <- x^power    # raise the elements in x to the power of y
  step2 <- sum(step1) # sum-up the squarred values
  list(step1=step1, step2=step2)
}

y <- 2
foo(z, y)
foo(z, power=y)
foo(z)
```

## (Useful) Ressources

- https://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf

## Exercises

**`Percentages`**

1. Read in the file `facebook_data`. Assign the file to an object called `data`.

```{r}
data <- read.csv2("facebook_data.csv")
data
```

2. Rename the columns in the data frame to `ID`, `Achievement`, `Facebookhours`, `No.Friends`.

```{r}
# overriding the colnames
colnames(data) <- c("ID", "Achievement", "Facebookhours", "No.Friends")
# check 
colnames(data)
```

3. Write a function `percentages` that calculates the percentage of `Facebookhours` spent per day, rounded to two decimal places and including a percentage sign after the rounded number (Hint: use the functions `round()` and `paste()`).

```{r}
percentages <- function(x) {   # takes on only one argument x (the data)
  res <- (x/24)*100            # divide the argument x (the data) by 24, multiply by 100 to obtain percentages
  return(paste(round(res, digits = 2), "%", sep="")) # return the (rounded) values incl. % (we do not want to have a space in between the value and %)
}

percentages(data$Facebookhours) # call the use defined function `percentages` on the Facebookhours data
```

**`RANGE`**

1. Write a function `RANGE` to calculate the spread of the values in the percentage of `Facebookhours` spent per day (Hint: use the functions `min()` and `max()`).
2. In addition, calculate the `median` of the values within the `RANGE` function. Return both results.

```{r}
RANGE <- function(x){                # takes on only one argument x (the data)
  res <- (x/24)*100                  # recycle the result from the previous function, transform data values into percentages
  spread <- max(res) - min(res)      # calculatae the spread of res (the tranformed data)
  Median <- median(res)              # calculate the median of res (the transformed data)
  list(Median=Median, Spread=spread) # Name the elements in the list
}

RANGE(data$Facebookhours)            # call the user defined funtion `RANGE` on the facebookhours data vector
```

**`one.sample`**

1. Write a function `one.sample` to calculate a `one-sample t-statistic` (see https://en.wikipedia.org/wiki/Student%27s_t-test) to test the null hypothesis that the population mean  is `x`, where `x` can be specified by the user.
2. Apply this function to `Achievement`, `Facebookhours` and `No.Friends`.

Note: the formula is $(\bar{y} - x) / (\hat{\sigma}_y / \sqrt{n_y})$

```{r}
one.sample <- function(data=data, x=0){  # takes on two arguments: the data and the population mean x (provided default value 0)
  denominator <- sqrt(var(data)/ length(data)) # note sqrt of variance = standard deviation
  nominator <- mean(data) - x
  t.value <- nominator/denominator
  return(t.value)
}

one.sample(data$Facebookhours)
```

More advanced (statistical) solution
```{r}
one.sample <- function(data=data, x=0){          # takes on two arguments: the data and the population mean x (provided default value 0)
  denominator <- sqrt(var(data)/ length(data))   # note sqrt of variance = standard deviation
  nominator <- mean(data) - x
  t.value <- nominator/denominator
  df <- length(data)-1                           # calculate the degrees of freedom (n-1)
  p.value <- 2*pt(t.value, df, lower.tail=FALSE) # calculate the p.value (t.value is t-distributed)
  list(t.value = t.value, df=df, p.value=p.value)
}

one.sample(data$Facebookhours)
```
