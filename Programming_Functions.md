---
title: "Functions"
sidebar: toc
category: Programming
order: 1
---

## Definition

```{r}
function_name <- function(argument) {
  statement
  }
```
**`function_name`**

- The actual name of the function.
- Stored in the R environment as an object with this name.

**`function`**

- Reserved word (followed by parentheses) used to declare a function in R. 

**`argument`**

- Placeholders (optional values). 
- A function may contain no arguments, default values or a value is passed to the argument.

**`{ statement }`**

- Statements within the curly brace form the function body. 
- Everything between the braces is part of the body of the function. Defines what the function does (and often also determines it's name).

**Example**:

```{r}
foo <- function(x) { x^2 }
foo(2)
```

The function `foo` squares a value and prints the result `4`.

## Return values

- Unlike some other languages explicit `return()` functions are used to return a value immediately from a function (used in processing steps). For example, the following construct does NOT do what is intended:

```{r}
return (2/5) * 3:9  # returns 0.4 and ignores the rest.
```

- The value returned from a function can be any valid object.

- If there are no explicit returns from a function, the value of the last evaluated expression is returned automatically in R.

**Example**:

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

- The `return()` function can return only a single object. If we want to return multiple values in R, we can use a (named) `list` and return it.

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

**Example**:

```{r}
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

**Example**:

```{r}
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

- [R for Beginners](https://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf)


## Generic functions

**Example**: OLS estimation

Necessary incredients:

```{r}
# load the data
load("Q4_2011.RData")
```

```{r}
# The outcome vector y
y <- Q4_2011$cnt

# Built the X matrix (design/ data) matrix, includes an intercept) 
X <- as.matrix(cbind(1,Q4_2011[,c("workingday","weathersit","atemp","hum","windspeed")])) 
# Note: force X to be a matrix as cbind() generates a list!
head(X)

# Alternatively you can use:
X <- model.matrix(cnt ~ workingday + weathersit + atemp + hum + windspeed, data=Q4_2011)
# model.matrix() offers more flexibility: allows for the definition of contrasts (and the intercept is automatically included and labeled as "intercept")
head(X)
```

* Obtaining the beta coefficients $\hat{\beta} = (X'X)^{-1} X'y$

```{r}
beta_hat <- solve(t(X) %*% X) %*% t(X) %*% y
beta_hat
```

* Obtaining the residuals (defined as the deviations of the predicted values $\hat{y} = X \hat{\beta}$ from the observed values $y$):

```{r}
resid <- y - X %*% beta_hat
head(resid)
```

* Obtaining $\hat{\sigma}^2 = (e'e) / (n - p)$:

```{r}
sigma2_hat <- (t(resid) %*% resid) / (nrow(X) - ncol(X)) 
sigma2_hat
```

* Obtaining the variance-covariance (vcov) matrix $\hat{\sigma}^2 (X'X)^{-1}$

```{r}
vcov_hat <- c(sigma2_hat) * solve(t(X) %*% X) 
vcov_hat
```

* Obtaining the standard errors (the square root of the diagonal elements in vcov):

```{r}
sqrt(diag(vcov_hat))
```

### Putting it all together ("writting a function")

```{r}
ols <- function(x = x, y = y, data = data){
  X <- model.matrix(as.formula(paste("y ~", paste(x, collapse="+"))), data=data) # Generate X
  # We can use the function paste() within model.matrix()
  # Note: you could also parse x and y directly within a formula statement to model.matrix (what is done by the lm() function but a bit tricky)
  
  beta_hat <- solve(t(X) %*% X) %*% t(X) %*% y               # obtaining the beta coefficients
  fitted <- X %*% beta_hat                                   # obtaining the fitted values (model predictions)
  resid <- y - fitted                                        # obtaining the residuals
  sigma2_hat <- (t(resid) %*% resid) / (nrow(X) - ncol(X))   # obtaining the sigma^2_hat
  vcov_hat <- c(sigma2_hat) * solve(t(X) %*% X)              # obtaining the VCOV
  se <- sqrt(diag(vcov_hat))                                 # obtaining the standard errors
  
  # Collect everything in a list
  list(beta=beta_hat, resid=resid, se=se, vcov=vcov_hat, X=X, fitted=fitted)
}

# Assign the result to an object named `res`
res <- ols(x = c("workingday","weathersit","atemp","hum","windspeed"), y = y, data = Q4_2011)
```

Short for what is included:
```{r}
names(res)
```

What is type is the output?

```{r}
typeof(res)
```

Get a full overview of the listing
```{r}
str(res)
```

Write a function for deriving a coefficient table

```{r}
summary.ols <- function(x = x){
  # data.frame makes the output kable() fit!
  data.frame("Estimate" = x$beta, 
             "Std.Error" = x$se, 
             "t.value" = x$beta/x$se, 
             "p.value" = 2*pnorm(-abs(x$beta/x$se))) # Note: abs() avoids control structures (i.e. differentiating between negative and positive t.values)
}

round(summary.ols(res),4)
```

You can also add significance codes: 

```{r}
summary.ols <- function(x = x){
  sig <- 2*pnorm(-abs(x$beta/x$se))
  codes <- ifelse(sig <= 0.01, paste("**"), ifelse(sig <= 0.05, paste("*"),""))               
  data.frame("Estimate" = x$beta, 
             "Std.Error" = x$se, 
             "t.value" = x$beta/x$se, 
             "p.value" = sig, 
             "Sig." = codes)
}
summary.ols(res) # note: rounding is now no longer that easy (same problem holds for the ordinary summary function)
```

### Classes and methods

Compare to the built-in function `lm()`
```{r}
res.lm <- lm(cnt ~ workingday + weathersit + atemp + hum + windspeed, data = Q4_2011)
```

Short for what is included:
```{r}
names(res.lm)
```

What type is the output?

```{r}
typeof(res.lm)
```

Get a full overview of the listing (looks familiar)

```{r}
str(res.lm)
```

```{r}
summary(res.lm)
```

What is then the difference?
```{r}
class(res.lm)
```

```{r}
class(res)
```

We can also assign our own class. Why should we care?

```{r}
methods(summary)
```
