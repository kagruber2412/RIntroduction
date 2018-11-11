# Do's and Don'ts

## Failing to vectorize:

```{r}
c(log(1), log(2))

# is equal to
log(c(1,2))
```

```{r}
x <- 1:100
for(i in 1:length(x)){
  x[i] <- log(x[i])
}
x
```

```{r}
x <- 1:100
log(x)
```

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

If vectorization is impossible: Put as much outside of loops as possible!
* Creating the same sequence object in each iteration - create the sequence first and than resuse it to save computing time.
* Condition checks (if statements) should be done before running the loop, and the loop should only be run for `TRUE` conditions to keep the iteration number low.
* Use `data.table` objects to reduce memory overload.
* Run those operations that are not vectorized in an **apply** statement. 

**Example:** Summing over rows of a matrix.
Set-up a for-loop that calculates the sum for each row of a matrix named `X`. Use the `matrix()` function and define the number as rows as `nrow = 100000` and the number of columns as 10  containing the numbers 1:1000000 and having dimensions 100000 rows and 10 columns. 

```{r}
X <- matrix(1:1000000, nrow = 100000, ncol = 10)
```

```{r}
# Solution 1
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

```{r}
# Solution 2
for.time <- system.time({
for.sum <- vector('integer', 100000) # the faster option
  for (i in 1:nrow(X)) {
      for.sum[i] <- sum(X[i,])
  }
})

# check the result
head(for.sum)

# obtain the computing time
for.time
```

```{r}
# Solution 3
apply.time <- system.time(
  app.sum <- apply(X, MARGIN = 2, sum)
)

# check the result
head(app.sum)

# obtain the computing time
apply.time
```

```{r}
# Solution 4
lapply.time <- system.time(
  lapp.sum <- unlist(lapply(1:dim(X)[2], function(l) sum(X[,l])))
)

# check the result
head(lapp.sum)

# obtain the computing time
lapply.time
```

```{r}
# Solution 5
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
* `lapply()` and `rowSums()` are based on more internal C++ code.
