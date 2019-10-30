# Data types (revised)

In a dataset, we can distinguish two types of variables: 


| **continuous** | **categorical** |
|-----------------------------------------------|------------------------------------------------|
| unlimited number of double \& integer values | limited number of integer \& character values |

**NOTE!** Character values are not supported in statistical models. The only way to consider them is to convert them to a vector of integers. 

<br>

# Factors in R

(see also R for Data Science, Chapter 12, or https://r4ds.had.co.nz/factors.html Chapter 2.15)

**Factors** store character values as integers. The character values are treated as labels associated with a set of unique integer values. This ensures that the statistical function used will treat such data correctly.

In R the `factor()`-function is used to create and modify factors: 

~~~
factor(x = character(), levels, labels = levels, ordered = is.ordered(x))
~~~

* `x`: a vector of distinct data values that will be returned as a vector of factor values.

* `levels`: (optional) a vector of the unique character values that `x` might take on. The default is the unique set of values taken by `as.character(x)`, sorted into increasing order of `x`. 

* `labels`: `(optional)` a character vector of labels for the levels (in the same order as levels). Duplicated values in labels can be used to map different values of `x` to the same factor level.

* `ordered`: logical flag to determine if the levels should be regarded as ordered (in the provided order).

<br> 

**Example:** _Ben\&Jerry ice-cream_ (continued).

| **continuous** | **categorical** |
|-----------------------------------------------|------------------------------------------------|
| price paid, coupon value, total spendings, quantity | promotion type, package size, flavor, formula description |

```{r}
str(BenAndJerry)
```

**NOTE!** In the dataset `size1_descr`, `flavor_descr` and `formula_descr` are character values. We have to convert them into **factors** to properly work with them!


```{r}
fat.content <- factor(BenAndJerry$formula_descr)
```
```{r}
head(fat.content)
```

In R’s memory, `LIGHT HALF THE FAT` and `REGULAR` are represented by the numbers `1` and `2`. The levels `LIGHT HALF THE FAT` and `REGULAR` are used when the factor is displayed. This is also more descriptive than 1 and 2.

You can also check the levels of a factor with `levels()`, and the number of levels with `nlevels()`:

```{r}
levels(fat.content)
nlevels(fat.content)
```

**NOTE!** A factors levels will always be character values. As a consequency, factors look like character vectors but they are actually integers.

<br>

The function `summary()` handles factors differently to characters (and numbers):

```{r}
summary(BenAndJerry$formula_descr)
```

```{r}
summary(fat.content)
```

The occurrence counts for each value is often more useful information.

<br>

## Unordered vs. ordered factors

The default order of the factor levels is **alphabetical**. Thus, R will assign 1 to the level `LIGHT HALF THE FAT` and 2 to the level `REGULAR` (because "L" comes before "R", even though the first element in this vector is `REGULAR`).

Sometimes, the order of the factor does not matter, other times you might want to specify the order because it is meaningful or it is required by a particular type of analysis (like changing the reference to consider a certain category as baseline).

```{r}
levels(fat.content)  # raw data levels
```

```{r}
min(fat.content)     # doesn't work
```

~~~
Error in Summary.factor(c(2L, 2L, 2L, 2L, 2L, 2L, 2L, 2L, 2L, 2L, 2L,  : 
  ‘min’ not meaningful for factors
~~~

**NOTE!** For unordered factors only the logical operators  `==` and `!=` can be used for comparing levels. Meaning: a factor can only be compared to another factor with an identical set of levels (not necessarily in the same ordering) or to another character vector. Specifying the order of the levels allows us to compare levels:

```{r}
fat.content <- factor(fat.content, levels = c("LIGHT HALF THE FAT", "REGULAR"), 
                      ordered = TRUE) # order levels
min(fat.content)     # works!
```

Ordered factors are compared in the same way as unordered factors, but the general dispatch mechanism precludes comparing ordered and unordered factors. Thus, on ordered factors the `min()`, `max()`, and `range()` functions can be applied (and for unordered factors R will generate an error).

Another way of changing the order of factor levels is offered by the `relevel()`-function. The `relevel()`-functions sets a specific value as a reference (= the first value in the level list): 

```{r}
div.fat <- factor(sample(c("regular", "skimmed", "light"), 
                          size = 10, replace = TRUE))
```

```{r}
div.fat <- relevel(div.fat, ref = "skimmed") # Make skimmed first
div.fat
```

```{r}
div.fat <- relevel(div.fat, ref = "regular") # Make regular first
div.fat
```

**NOTE!** `relevel()` won't work for ordered factors!

<br>

## Renaming factor levels

Rename _all_ levels by using **replacement operations**: 

```{r}
div.fat <- factor(sample(c("regular", "skimmed", "light"), 
                         size = 10, replace = TRUE), 
                  levels=c("regular","light","skimmed"), ordered=TRUE)
```
```{r}
levels(div.fat) <- c("10% fat", "6% fat", "0% fat")
div.fat
```

**NOTE!** This method will modify `div.fat` directly! Thus, you don’t have to save the result back into `div.fat`.

You can also rename _single_ levels by indexing:

```{r}
levels(div.fat)[levels(div.fat) == "light"] <- "6% fat"
levels(div.fat)
```

**NOTE!** Specific elements in a vector can be accessed by `[]`. The principle is similar to using the `subset()`-function but using square brackets allows you to assign values to a specific element which is not possible with `subset()`. 

In addition, using square brackets allows you to access specific elements in a vector by position:

```{r, eval=FALSE}
levels(div.fat)[1] <- "6% fat"  # access first level by position
levels(div.fat)[2] <- "10% fat" # access second level by position
```

In general, you can do the same in a more convenient way with the `revalue()` or `mapvalues()`-functions from the `plyr` package.

```{r}
# install.packages("plyr")
library(plyr)
```

```{r}
div.fat <- factor(sample(c("regular", "skimmed", "light"), size = 10, replace = TRUE), 
                  levels=c("regular","light","skimmed"), ordered=TRUE)
```

Rename _several_ levels with **`revalue()`** and **`mapvalues()`**:

```{r}
revalue(div.fat, c("regular" = "10% fat", "light" = "6% fat"))
mapvalues(div.fat, from = c("regular", "light"), to = c("10% fat", "6% fat"))
```

