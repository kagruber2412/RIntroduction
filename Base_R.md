---
title: "Base R"
author: "Kathrin Gruber"
date: ""
output: pdf_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

# Data types

R has three basic data types (storage modes): 

1(a). **Numeric** (real numbers). Two most common numeric types are `double` (double precision floating point numbers) and `integer`.

1(b). **Complex** (real and imaginary numbers).

2. **Logical** (boolean). Reserved words for denoting logical constants are `TRUE` and `FALSE` (and `NA` for missing value). 

3. **Character strings** (text).

# Data structures

## Vectors

- A vector is a collection of elements of a single data type (homogenous or **atomic**).
- The `c()` function concatenates elements together.

```{r}
dbl <- c(0.5, 0.6, 0.25)          # double
int <- c(9L, 10L, 11L, 12L, 13L)  # integer
int <- c(9:13)                    # integer sequence
lgl <- c(TRUE, FALSE, FALSE)      # logical
cmp <- c(1+0i, 2+4i)              # complex
chr <- c("a", "b", "c")           # character
```

Combinations
```{r}
c(dbl,dbl)
```

<div class = "blue">
Changing the data type of a vector is often called <b>coercion</b>. Coercion ensures to maintain the same primitive data type for elements in the same vector.
```{r}
c(dbl, chr)
```
</div>

### Vector actions

Printing out the `dbl` vector on the console.
```{r}
dbl
```

Checking the data type of the `dbl` vector.
```{r}
typeof(dbl)
```

Checking the length the `dbl` vector.
```{r}
length(dbl)
```

Extracting the first element of the `dbl` vector.
```{r}
dbl[1]
```

Extracting the first and the second element of the `dbl` vector by another vector.
```{r}
dbl[c(1,2)]
```

**Example tasks**

- Negative Index `dbl[-c(1,2)]`
- Duplicate Indexes `dbl[c(2,2)]`
- Re-Order Indexes `dbl[c(3,1,2)]`
- Range Index `dbl[1:3]`
- Out-of-Range Index `dbl[4]`
- Logical index vector `dbl[c(TRUE, FALSE, FALSE)]`


### Vector arithmetic

Arithmetic operations of vectors are performed elementwise.

```{r}
a <- c(10, 20, 30, 40) 
b <- c(1, 2, 3, 4)
```

Scalar multiplication (each element multiplied by 5)
```{r}
a * 5
```

Vector multiplication (summation of the elements in a and b)
```{r}
a * b
```

<div class = "blue">
```{r}
u <- c(10, 20, 30) 
v <- c(1, 2, 3, 4, 5, 6, 7, 8, 9) 
u / v
```
If two vectors are of unequal length, the shorter vector will be <b>recycled</b> in order to match the longer one
</div>

## Matrices

- A matrix is a collection of data elements arranged in a two-dimensional rectangular layout (short: a two-dimensional generalization of vectors). 
- The components in a matrix must be of the same basic type. 

Combining vectors columnwise
```{r echo=TRUE}
m1 <- cbind(a, b)   # two columns
m1
```

Combining vectors rowwise
```{r}
m2 <- rbind(a, b)   # two rows
m2
```

**Example tasks**

- Three columns `m1 <- cbind(b, b, a)`
- Three rows `m2 <- rbind(a, a, b)`
- Matrix combinations `m <- rbind(m2, m2)`

<div class = "blue">
```{r, eval=FALSE}
m <- cbind(m1,m2)
```
Two matrices of unequal dimensions (number of rows or number of columns) cannot be combined.
</div>

Creating a matrix from elements of a vector

```{r echo=TRUE}
b <- c(1, 2, 3, 4)                 # vector with 4 elements
B <- matrix(b, ncol=2)             # matrix with 2 columns, filled-up columnwise
B
```

```{r echo=TRUE}
b <- c(1, 2, 3, 4)                  # vector with 4 elements
D <- matrix(b, ncol=2, byrow=TRUE)  # matrix with 2 columns, filled-up rowwise
D
```

<div class = "blue">
- How do we know this? `?matrix`
</div>

### Matrix actions

Checking the number of rows of the `B matrix.
```{r echo=TRUE}
nrow(B)
```
Checking the number of columns of the `B` matrix.
```{r echo=TRUE}
ncol(B)
```
Checking the dimension `[nrow, ncol]` of the `B` matrix.
```{r echo=TRUE}
dim(B)        
```
Extracting the first row of the `B` vector.
```{r echo=TRUE}
B[1,]        
```
Extracting the first column of the `B` vector.
```{r echo=TRUE}
B[,1]        
```
Extracting the first element of the `B` vector.
```{r echo=TRUE}
B[1,1]        
```

### Matrix arithmetic

Matrix addition (subtraction)
```{r echo=TRUE}
B + B
```
Scalar multiplication
```{r echo=TRUE}
B * 2
```
Elementwise multiplication
```{r echo=TRUE}
B * B
```
Matrix multiplication
```{r echo=TRUE}
B %*% B
```

**Example tasks**

- Transpose `t(B)`
- Inverse `solve(B)`
- Eigenvalues `eigen(B)`
- Determinant `det(B)`

## Data frames

- Matrix like structures, but columns can have different data types (**data matrices**)
- Columns are vectors (numeric, character, or logical) of the same length

```{r}
dbl <- c(0.5, 0.6, 0.25, 1.2, 0.333)      # double
int <- c(9L, 10L, 11L, 12L, 13L)          # integer
lgl <- c(TRUE, FALSE, FALSE, TRUE, TRUE)  # logical
chr <- c("a", "b", "c", "d", "e")         # character
df <- data.frame(dbl,int,lgl,chr)
df
```

Build-in data frame `airquality`
```{r}
head(airquality)   # Previewing (the first 6 rows)
```

**Example tasks**

- Number of rows `nrow(airquality)`
- Number of columns `ncol(airquality)`
- Dimensions `dim(airquality)`
- Get the first and third column `airquality[,c(1,3)]` alternative `airquality[,c("Ozone","Wind")]`