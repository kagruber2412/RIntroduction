---
title: "Loops"
sidebar: toc
category: Programming
order: 1
---

## Definition

* For-loops iterate across a sequence of values (in R for-loops iterate over a vector).
* Runs code repeatedly for each value in the sequence.
* Automating a multi-step process.

```{r}
for(var in sequence){
  statement
}
```

The variable `var` authorsuccessively takes on each value in the sequence. For each such value, the code is run with `var` having that value from the sequence.

### Example: Raise x to the power of x

```{r}
x <- 1:5   # create a sequence 1 to 5, assign the result to x

x[1]^x[1]
x[2]^x[2]
x[3]^x[3]
x[4]^x[4]
x[5]^x[5]
```


```{r}
for(i in 1:5){
  power <- x[i] ^ x[i]
  print(power)
}
```

* The function `for()` takes on the variable `i` as input, where `i` gets assigned a value of the sequenece from 1 to 5 (also called **counter**). Here, the sequence corresponds to the position index of the elements in `x`.

* For each iteration in the for-loop, a value is taken from the sequence and assigned to the variable, and the following code (in the braces, `{}`) is evaluated.

## Storage

* Change `print()` to assignment statement.
* Prepare a **container vector**  (having the correct length for storing the result).

```{r}
result <- vector(mode="integer", length=length(x))  # container vector of mode "integer" and length corresponding to the data vector x (the vector over which elements the loop is defined)
for(i in 1:5){
  power <- x[i]^x[i]
  result[i] <- power  # change print to assignment
}

result
```

```{r}
result <- data.frame(matrix(nrow = 5, ncol = 2))
colnames(result) <- c("i", "power")
for (i in 1:5) {
  power <- x[i] ^ x[i]
  result[i, 1] <- i
  result[i, 2] <- power
}

result
```

## Control structures

* If-else statements allows to test a condition and act on it depending on whether it’s `TRUE` or `FALSE`.
* Allows to test a condition and act on it depending on whether it’s true or false.

```{r}
if(condition) {
  do something
  } else {
    do something else
      }
```

### Example: Squarring instead of raiseing x to the power of x

```{r}
result <- vector(mode="integer", length=length(x))  # container vector of mode "integer" and length corresponding to the data vector x (the vector over which elements the loop is defined)
for(i in 1:5){
  if(i <= 2){         # if i is smaller/equal to 2  
  power <- x[i]^x[i]
  } else {
  power <- x[i]^2     # if i is larger than 2, take the squarre
  }
  result[i] <- power  # change print to assignment
}

result
```

## (Useful) Ressources

- [R for Beginners](https://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf)


## Exercises

**`one.sample`**

1. Read in the `facebook_data` file. Assign the file to an object called `data`.

2. Rename the columns in the data frame to `ID`, `Achievement`, `Facebookhours`, `No.Friends`.

3. Write a `for-loop` to apply the function `one.sample` to `Achievement`, `Facebookhours` and `No.Friends`.

```{r}
result <- vector("double", length=4). # Version 1
result <- unlist(list(NULL)).         # Version 2
result <- vector()                    # Version 3
for(i in 2:dim(data)[2]){
  result[i] <- t.test(data[,i])$statistic
}

result
```

**`mean`**

1. Write a `for-loop` to calculate the `mean()` for `Achievement`, `Facebookhours`, `No.Friends` (include the `ID` column but substitute it's mean with `0`).

```{r}
result <- vector()
for(i in 1:dim(data)[2]){
  if(sum(data[,i] / c(1:25))==25){
    result[i] <- NA
  } else {
  result[i] <- mean(data[,i])
  }
}
result
```

**`Data transformation`**

1. Write a for-loop classifying Achievement as 0 if the value is below the mean and as 1 if the value is above the mean.

```{r}
result <- vector()
for(i in 1:length(data$Achievement)){
  if(data$Achievement[i] < mean(data$Achievement)) {
    result[i] <- 0
  } else {
    result[i] <- 1
  }
}
result
```

```{r}
# Alternative
result <- ifelse(data$Achievement < mean(data$Achievement), 0, 1)
result
```

