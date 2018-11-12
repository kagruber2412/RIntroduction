---
title: "Do's and Don'ts"
sidebar: toc
category: Programming
order: 1
---

## Failing to vectorize

Improving computational efficiency by using vectorization.

```{r}
c(log(1), log(2))

# is equal to
log(c(1,2))
```

The `log()` function is vectorized: it does the same operation on a vector of elements as it would do on each single element. 

Why should we care?

```{r}
x <- 1:100
system.time(for(i in 1:length(x)){
  x[i] <- log(x[i])
})
```

```{r}
x <- 1:100
system.time(x <- log(x))
```

**Excessive looping makes your code slow!**

If vectorization is impossible: 

* Put as much outside of loops as possible!
  + Avoid creating (the same) sequence objects in each iteration - create the sequence first and reuse it to save computing time.
  
* Reduce number of iterations!
  + Check conditions (if statements) before (!) running the loop. Keep the iteration number low by running the loop only on the `TRUE` conditions.
  
* Use `data.table` objects to reduce memory overload (if you have a substantial amount of data and speed is an issue). 
* Run those operations that are not vectorized in an **apply** statement. 

**Example:** Summing over rows of a matrix.
Set-up a for-loop that calculates the sum for each row of a matrix named `X`. Use the `matrix()` function and define the number as rows as `nrow = 100000` and the number of columns as 10  containing the numbers 1:1000000 and having dimensions 100000 rows and 10 columns. 

```{r}
X <- matrix(1:1000000, nrow = 100000, ncol = 10)
```

Solution 1
```{r}
for.time <- system.time({
for.sum <- vector()
  for (i in 1:nrow(X)) {
      for.sum[i] <- sum(X[i,])
  }
})

# check the result
head(for.sum)

# obtain the computing time
for.time
```

Solution 2 (the faster solution)
```{r}
for.time <- system.time({
for.sum <- vector('integer', 100000) 
  for (i in 1:nrow(X)) {
      for.sum[i] <- sum(X[i,])
  }
})

# check the result
head(for.sum)

# obtain the computing time
for.time
```

Solution 3
```{r}
apply.time <- system.time(
  app.sum <- apply(X, MARGIN = 2, sum)
)

# check the result
head(app.sum)

# obtain the computing time
apply.time
```

Solution 4
```{r}
lapply.time <- system.time(
  lapp.sum <- unlist(lapply(1:dim(X)[2], function(l) sum(X[,l])))
)

# check the result
head(lapp.sum)

# obtain the computing time
lapply.time
```

Solution 5
```{r}
sum.time <- system.time(
  rowSums.sum <- rowSums(X)
)

# check the result
head(rowSums.sum)

# obtain the computing time
sum.time
```

What are the reasons?
* No need to create a container object.
* `lapply()` and `rowSums()` are based on (more) internal C code.


## Over-vectorizing

Improving human efficiency by shortening the time spent on writting code using the **apply** family of functions (to express a simple loop more compactly).

| function | Input | Output |
|----------|------------------------|------------------------|
| `apply()` | matrix or array | vector, array or list |
| `lapply()` | list or vector | list |
| `sapply()` | list or vector | vector, matrix or list |
| `vapply()` | list or vector | vector, matrix or list |
| `tapply()` | data.frame, categories | array or list |

* The `apply()` function is used for applying functions to the rows or columns of matrices or dataframes. 
Often you want to apply a function across one of the margins of a matrix – margin `1` being the rows and margin `2` the columns. 

For example:
```{r}
X <- matrix(1:24,nrow=4)
```
Row totals
```{r}
apply(X,1,sum)
```
Column totals
```{r}
apply(X,2,sum)
```

You can supply your own function definition within apply like this:
```{r}
apply(X,1,function(x) x^2+x)
```

* The `apply()` function has a for loop in its definition. The `lapply()` function buries the loop, it takes a function, applies it to each element in a list, and returns the results in the form of a list (since data frames are also lists, `lapply()` is useful to apply an operation on each column/row of a data frame).

Row totals
```{r}
lapply(1:dim(X)[1], function(l){sum(X[l,])})
```
Column totals
```{r}
lapply(1:dim(X)[2], function(l){sum(X[,l])})
```

**NOTE**: use `unlist()` to convert the output from a list to a vector.

```{r}
unlist(lapply(1:dim(X)[2], function(l){sum(X[,l])}))
```

*  `sapply()` *simplifies* the output of `lapply()`. But the simplification that you get may not be the simplification you expect and makes `sapply()` not suitable to be used inside of functions. The `vapply()` function is sometimes a safer alternative.

* `tapply()` applies a function to each bit of a partition of a data frame (alternatives are `by()` and `aggregate()`).

```{r}
tapply(mtcars$mpg, mtcars$cyl, mean) 
```

```{r}
tapply(mtcars$mpg, list(mtcars$cyl, mtcars$am), mean) 
```

# (Usefull) ressources

* [The R-inferno by Patrick Burns](https://www.burns-stat.com/documents/books/the-r-inferno/)
