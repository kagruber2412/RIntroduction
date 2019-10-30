# Data types (revised)

In a dataset, we can distinguish two types of variables: 


| **continuous** | **categorical** |
|-----------------------------------------------|------------------------------------------------|
| unlimited number of double \& integer values | limited number of integer \& character values |

**NOTE!** Character values are not supported in statistical models. The only way to consider them is to convert them to a vector of integers. 

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
