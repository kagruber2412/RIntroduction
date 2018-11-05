---
title: "Exercises: Writting Functions"
author: Kathrin Gruber
output: pdf_document
---

## 1. Sum of three largest values.

Work out the sum of the three largest values in the following vector:
```{r}
y<-c(8,3,5,7,6,6,8,9,2,3,9,4,10,4,11)
```
Consider the following steps: sort the vector into descending order using the function `sort()`. Add up the values of the first three elements of the sorted array using the function `sum()`.

```{r eval=FALSE, include=FALSE}
# Solution 
sum(rev(sort(y))[1:3])  # use the function `rev()` to reverse the sorting (sorting in descending order)
sum(sort(y, decreasing=TRUE)[1:3]) # alternatively set the decreasing argument in sort to TRUE
```

## 2. Closest value function.
Find the value in a vector that is closest to a specified value. Consider the random vector `xv<-rnorm(1000,100,10)` and find the value of `xv` that is closest to `108.0` by using the function `which()`. Next, write a function `closest()` that returns the closest value to a specified value.

```{r eval=FALSE, include=FALSE}
# Solution
closest<-function(xv,sv){
  xv[which(abs(xv-sv)==min(abs(xv-sv)))] # using the function `abs()` avoids differentiating between cases
}

sv <- 108.8            # define test value
xv<-rnorm(1000,100,10) # generate 1000 normally distributed random numbers (having mean 100 and sd 10)
head(xv)               # check what is in xv
closest(xv, sv)
```

## 3. Arithmetic mean function.
The mean is defined as the sum of the elements in a vector `y` divided by its number of elements `n`. Write a function `arithmetic.mean` that calculates the mean for an input vector `y`. Use the functions `length()` and `sum()`. Test your function on the input vectors `c(0,0,0,1)` (to check that your function result is correct) and `c(3,3,4,5,5)`.

```{r eval=FALSE, include=FALSE}
# Solution
arithmetic.mean <- function(x){
  sum(x)/length(x)
}

y <- c(0,0,0,1)
arithmetic.mean(y)
y <- c(3,3,4,5,5)
arithmetic.mean(y)
```

## 4. Median of a single sample function.
The median (or 50th percentile) is the middle value of the sorted values of a vector of numbers. Write a function `med()` that calculates the median of a single vector `y`.You can obtain a sorted vector by using `sort(y)[ceiling(length(y)/2)]`. Test your function on the input vectors `c(3,3,4,5,5)` and `c(3,3,3,4,5,5)`.

Note, you also need to take into account if the vector contains an even number of elements, then there is no middle value and you need to work out the arithmetic average of the two values of `y` on either side of the middle. To decide if the vector contains an add or an even number of elements you can use the  `modulo` function `%%`.

```{r eval=FALSE, include=FALSE}
# Solution
med<-function(x) {
  odd.even<-length(x)%%2
  if (odd.even == 0) {
    (sort(x)[length(x)/2]+sort(x)[1+ length(x)/2])/2}
  else {
    sort(x)[ceiling(length(x)/2)]}
}

# odd
y <- c(3,3,4,5,5)
med(y)        
# even
y <- c(3,3,3,4,5,5)
med(y)   
```

## 5. Geometric mean function.
For processes that change multiplicatively rather than additively, neither the arithmetic mean nor the median is an ideal measure of central tendency. Under these conditions, the appropriate measure is the geometric mean. The geometric mean of a vector `y` is defined as follows
$$\widetilde{y} = \sqrt[n]{\prod_{i=1}^{n}y_i}$$

Let’s take a simple example: the numbers of insects on 5 plants were 10, 1, 1000, 1, 10. Multiplying the numbers together gives 100 000. There are five numbers, so we want the fifth root of this. Roots are fractional powers, so the fifth root is a number raised to the power `1/5 = 0.2`. Remember: in R powers are calculated by using the `^` symbol.

Write a function `geometric.mean` that calculates the geometric mean for an input vector `y`. Test your function on the input vector `c(10,1,1000,1,10)`.

```{r eval=FALSE, include=FALSE}
# Solution
y <- c(10,1,1000,1,10)
geometric.mean <- function(x){
  prod(x)^(1/length(x))
}
geometric.mean(y)
```

## 6. Trimmed mean function. 
The trimmed mean is another statistical measure of central tendency. It involves the calculation of the mean after discarding given parts ofsample at the high and low end, and typically discarding an equal amount of both. 

Write a function `trim.mean` that calculates a trimmed mean of `x`, which ignores both the smallest and largest values. Consider the following steps: sort the vector `x` using the function `sort()`. Remove the first element using `x[-1]` and the last using `x[-length(x)]`. Test your function on the vector `c(8,3,5,7,6,6,8,9,2,3,9,4,10,4,11)`.

```{r eval=FALSE, include=FALSE}
# Solution
trim.mean <- function(x){
  mean(sort(x)[-c(1,length(x))])
}

y <- c(8,3,5,7,6,6,8,9,2,3,9,4,10,4,11)
trim.mean(y)
```

## 7. Euclidean norm function. 
Write a function `norm()` that will compute the Euclidean norm of a numeric vector. The Euclidean norm of a vector 
`y` is defined as $$||y|| = \sqrt{\sum_{i=1}^{n} y_i^2}$$.
Test your function on the vectors `c(0,0,0,1)` (to check that your function result is correct) and `c(2,5,2,4)`.

```{r eval=FALSE, include=FALSE}
# Solution
norm <- function(x){
  res <- sqrt(sum(x^2))
  return(res)
}

y <- c(0,0,0,1)
norm(y)

y <- c(2,5,2,4)
norm(y)
```

## 8. Sample variance function.
The population variance is defined as the average of the squared deviations 
$$\sigma^2 = \frac{1}{n} \sum_{i=1}^{n} (y_i - \bar{y})^2$$ 
where $\bar{y}$ denotes the sample mean. The sample variance gives an estimate of the population variance that is biased by a factor of $\frac{n-1}{n}$. For this reason, $s^2$ is referred to as the biased sample variance. Correcting for this bias yields the unbiased sample variance defined as 
$$s^2 = \frac{n-1}{n} \sigma^2 = \frac{1}{n-1} \sum_{i=1}^{n} (y_i - \bar{y})^2$$
where we divide the sum of squares by the degrees of freedom $n-1$ rather than by the sample size $n$ (to compensate for the fact we have estimated the sample mean from the data).

Write a function `sample.var` that calculates the bias corrected sample variance. Test your function on the vector `c(13,7,5,12,9,15,6,11,9,7,12)`.

```{r eval=FALSE, include=FALSE}
# Solution
variance<-function(x){
  res <- sum((x - mean(x))^2)/(length(x)-1)
  return(res)
}
y <- c(13,7,5,12,9,15,6,11,9,7,12)
variance(y)
```

## 9. Standard error function.
The variance is used in two main ways: for establishing measures of unreliability (e.g. confidence intervals) and for testing hypotheses (e.g. Student’s t test). 

Unreliability measures are called standard errors. The standard error of the mean is
$$ se = \sqrt{\frac{s^2}{n}}$$
where $s^2$ is the variance and $n$ is the sample size. There is no built-in R function to calculate the standard error of the mean. Write a function `se` that calculates the standard error of the mean. Test your function on the vector `c(13,7,5,12,9,15,6,11,9,7,12)`

```{r}
se<-function(x){
  se <- sqrt(var(x)/length(x))
  return(se)
}

y <- c(13,7,5,12,9,15,6,11,9,7,12)
se(y)
```

## 10. Standard errors and confidence limits function.
You can refer to functions from within other functions. 

A confidence interval (CI) can be defined as *t from tables* times the standard error: $$CI = t_{\alpha/2, df} \times se$$. 

Write a function `ci95` that computes the 95% confidence interval for the mean using the function `se()`. The R function `qt` gives the value of Student's $t$ with $1-\alpha/2 = 0.975$ and degrees of freedom `df = length(x)-1`. Test the function on 150 normally distributed random numbers with mean 25 and standard deviation 3: `rnorm(150,25,3)`.

```{r}
ci95<-function(x) {
t.value<- qt(0.975,length(x)-1)
standard.error<-se(x)
ci<-t.value*standard.error
cat("95% Confidence Interval = ", mean(x) -ci, "to ", mean(x) +ci,"\n") }

x<-rnorm(150,25,3)
ci95(x)
```

