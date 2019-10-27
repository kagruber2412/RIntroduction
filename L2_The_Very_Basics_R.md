---
title: "The very basics"
sidebar: toc
category: getting-started
order: 1
---

# Data types

R has three basic data types: 

1(a). **Numeric** (``real numbers''). 

The two most common numeric types are `double` (double precision floating point numbers) and `integer` (without floating point).

1(b). **Complex** (``real and imaginary numbers'').

2. **Logical** (``boolean''). 

Reserved words for denoting logical constants are `TRUE` and `FALSE` (and `NA` for missing value). 

3. **Character**.

Data type for storing letters and symbols (strings, text).


# Data structures

1. **Scalar**.

2. **Vector**. Collection of elements of a single (``atomic'') data type.

3. **Matrix**. Collection of elements arranged in a two-dimensional rectangular layout (a two-dimensional generalization of a vector). Same as vector, all elements must be of a single data type.

4. **Data frame**. More general matrix like structure (``data matrix''). Different columns can have different data types.

5. **List**. Generic vector containing other objects. No restriction on data types or length of the single components.


# Vectors

Concatinating elements together with **`c()`**.
```{r}
c(0.5, 0.6, 0.25)            # double
c(9L, 10L, 11L, 12L, 13L)    # integer
c(9:13)                      # integer sequence
c(TRUE, FALSE, FALSE)        # logical
c(1+0i, 2+4i)                # complex
c("a", "b", "c")             # character
```

## Vector actions

Assign the vectors to names:
```{r}
dbl <- c(0.5, 0.6, 0.25)     
chr <- c("a", "b", "c")      
```

Print out the `dbl` and `chr` vectors on the console:
```{r}
dbl
chr
```

Check the number of elements in `dbl` and `chr`:
```{r}
length(dbl)
length(chr)
```

Check the data type `dbl` and `chr`:
```{r}
typeof(dbl)
typeof(chr)
```

Combine two vectors:
```{r}
c(dbl,dbl)
```

```{r}
c(dbl, chr)
```

NOTE! The automatic change of the data type of the resulting vector is called **coercion**. Coercion ensures the same data type for each element in the vector is maintained.


## Vector arithmetic

Define two new numeric vectors `a` and `b` each having 4 elements:
```{r}
a <- c(1, 2, 3, 4)
b <- c(10, 20, 30, 40) 
```

Multiply each element in `a` by 5 (scalar multiplication):
```{r}
a * 5
```

Multiply the elements in `a` by the elements in `b` (vector multiplication):
```{r}
a * b
```

Multiply the elements in `a` by the elements of some numeric vector `v` of length 5:
```{r}
v <- c(1.1, 1.2, 1.3, 1.4, 1.5) 
a * v
```

NOTE! Arithmetic operations of vectors are performed **elementwise**. If two vectors are of unequal length, the shorter vector will be **recycled** in order to match the longer one (here, the first element in `a` is used again).

# Matrices

**Option (1)**: Combining two vectors columnwise with `cbind()`:
```{r}
A <- cbind(a, b)   # two columns
A
```

**Option (2)**: Combining two vectors rowwise with `rbind()`:
```{r}
B <- rbind(a, b)   # two rows
B
```

**Option (3)**: Creating a matrix from elements of a vector with `matrix()`:
```{r}
A <- matrix(a, ncol=2, nrow=2)    # matrix with 2 columns and 2 rows
A
```

The arguments _`nrow`_ and _`ncol`_ indicate the number of rows and number of columns the resulting matrix consists of. 

NOTE! For 4 elements and _`ncol`_`= 2` the matrix can only have 2 rows. Thus, there is no need to specify both arguments.

```{r}
A <- matrix(a, ncol=2)             # matrix with 2 columns and 2 rows
A
```

NOTE! By default the matrix is filled up column after column (R treats a matrix object internally as a column vector). If the matrix should be filled up row after row the argument _`byrow`_`= TRUE` is required.

```{r}
B <- matrix(a, ncol=2, byrow=TRUE) # matrix filled-up rowwise
B
```

## Matrix actions

Checking the number of rows:
```{r}
nrow(B)
```
Checking the number of columns:
```{r}
ncol(B)
```
Checking the dimension `[nrow, ncol]`:
```{r}
dim(B)        
```

Combine two matrices:
```{r}
D.wide <- cbind(A,A)
D.wide
```

```{r}
D.long <- rbind(A,A)
D.long
```

`D <- cbind(D.wide, D.long)`

~~~
Error in cbind(D.wide, D.long) : object 'D.wide' not found
~~~

NOTE! Two matrices of unequal dimensions (number of rows or number of columns) cannot be combined.

## Matrix arithmetic

Matrix addition:
```{r}
B + B
```
Scalar multiplication:
```{r}
B * 2
```
Elementwise multiplication:
```{r}
B * B
```
Matrix multiplication:
```{r}
B %*% B
```

**More matrix arithmetic**

- Transpose **`t()`**
```{r}
D.wide
t(D.wide)
```

- Determinant **`det()`** 
```{r}
det(B)
```
- Inverse **`solve()`** (only if **`det()`** $\neq$ 0)
```{r}
solve(B)
```
- Eigenvalues **`eigen()`** (only for square and symmetric matrices)
```{r}
eigen(B)
```

# Data frames

```{r}
dbl <- c(0.5, 0.6, 0.25, 1.2, 0.333)      # double
int <- c(9L, 10L, 11L, 12L, 13L)          # integer
lgl <- c(TRUE, FALSE, FALSE, TRUE, TRUE)  # logical
chr <- c("a", "b", "c", "d", "e")         # character
df <- data.frame(dbl,int,lgl,chr)
df
```

## Data frame actions

Checking the number of rows:
```{r}
nrow(df)
```
Checking the number of columns:
```{r}
ncol(df)
```
Checking the dimension `[nrow, ncol]`:
```{r}
dim(df)        
```

## Lists

```{r}
a <- 1L                                   # scalar
dbl <- c(0.5, 0.6, 0.25, 1.2, 0.333)      # numeric vector of length 5
chr <- c("a", "b", "c"        )           # charcter vector of length 3
v <- c(1.1, 1.2, 1.3, 1.4)
mat <- matrix(v, ncol=2)                  # 2 x 2 matrix

l <- list(a, dbl, chr, mat)
l
```
