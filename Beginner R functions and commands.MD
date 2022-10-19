# Useful R functions and commands

These notes are based off material from the University of Glasgow's [Statistical Programming module](https://www.gla.ac.uk/coursecatalogue/course/?code=STATS5078) (only weeks 1-3 so far).

## Getting started
`install.packages()` - Install new R package

`ls()` - List all variables in your workspace

## Basic commands

`<-` - assignment operator for declaring variables

## Vectors

`c()` - Create a vector

`names()` - name vector entries e.g. `names(a) <- c("first", "second", "third")`

`seq(from, to, by)` - create a sequence e.g. `seq(1, 2, by=0.2)`

`seq(from, to, length.out=n)` - another way of creating a sequence of length `n` 

`rep(x, times=n)` - repeats the vector

`rep(x, each=n)` - repeats each element of the vector

`numeric(n)` - creates a vector of length `n` made of 0s

`length()` - Returns the number of elements in a vector

`unique()` - Returns the number of unique elements

`rev()` - Reverses a vector

`sort()` - Sort a vector, add `decreasing=TRUE` to sort in descending order

`order()` - Returns the permutation required to sort the vector (i.e. first element then third, then fifth, then 6th etc.)


## Scalar summary functions

`min, max, sum, prod, mean, median, sd, var`

## Ways to generate random variables

`rnorm(n)`  - Get sample of size `n` from the standard normal distribution

`pnorm()` - distribution function [I don't understand this yet!]

## Strings

`cat()` - Print a string

`print()` - Also prints a string?

`paste()` - Concatenates strings but use tidyverse `str_c()` if allowed

```R
library(stringr)
str_c("Two strings", "put together", sep=" - ")
```

Factors can be useful with categorical variables e.g.
`factor(x, levels=1:5, labels=c("one", "two", "three", "four", "five"))`

##Converting datatypes
`as.character()` - Converts to string
`as.numeric()` - Converts to numeric
`as.integer()` - converts to integer
`as.logical()` - TRUE or FALSE
`as.factor()` - Converts to factor 

## Dealing with missing values `NA`s
`NA` - Missing value

`na.omit(x)` - Ignore `NA`s in calculations e.g. `sum(na.omit(x))`

Can also use `mean(x, na.rm=TRUE)` but not all functions can use this argument so `na.omit(x)` is more reliable.

`is.na()` - Checks whether value is missing


## Maths functions
`sqrt()` - Square root

`exp()` - Exponential

`abs()` - Absolute value

`sign()` - Returns the signs of a numerical value

`log()` - Log

`pi` - Pi

`Inf` - Infinity

`sin, cos, tan, asin, acos, atan, gamma, beta, etc.` - Lots more functions!

## Function syntax

```R
#Declare the function
coolNameForAFunction <- function (x,y,z) {
# Do something with your x,y,z variables
doSomething <- x + 1
doSomethingElse <- y + doSomething/z
return(doSomethingElse)
}

#Use the function
coolNameForAFunction(x = 1, y=2, z=17)
```

## Logical variables and binary operators

`TRUE` - True

`FALSE` - False

`!` - Not

`|` - Or

`&` - And

## Comparison Operators

`==` - Exact equality

`!=` - Not exactly equal

`<, <=, >, >= ` - Comparison operators


**Don't use** `==` **for non-integers!** Use `abs(0.3-2*0.1 - 0.1) < 1e-8` or `isTRUE(all.equal(0.3-2*0.1, 0.1))`

## Matrices

`matrix(c(1,2,4,5,6,7), nrow=2)`

```R
rbind(c(1, 2, 3),
       c(4, 5, 6),
       c(7, 8, 9))
```
      
`cbind()` - Like rbind but for columns

`nrow()` - Number of rows

`ncol()` - Number of columns

`dim()` - dimensions of matrix

`length()` - number of entries in the matrix

`diag()` - Create a diagonal matrix e.g. `diag(c(1 ,4 , 4, 6, 5))`

`colnames()` - Name columns

`rownames()` - Name rows

`Matrix1 %*% Matrix2` - Matrix multiplication

`t(A)` - Transpose of matrix `A`

`crossprod()` - Crossproduct of a matrix

`t(A)%*%A` - Will also give you the cross product

`solve(A)` - Compute the inverse of A

## Lists

`list()` - create a list

`c()` - Combine lists e.g. `c(list1, list2)`

`names(list) <- c("element one", "element two")` - name list elements 

## Data frames

`data.frame()` - Create data frame e.g. 

```R
data.frame(age=c(4,11), weight=c(15,28), height=c(101,132), gender=c("f", "m"))
rownames(kids) <- c("Sarah", "John")
```

`as.data.frame()` - convert matrix to data frame

`as.matrix()` - convert data frame to matrix

Use `$` to access columns e.g. `kids$age`

`transform()` - Add a new column e.g. `transform(dataframe, newcolumn = column1 * column2)`

`dataframename[,-2]` - Remove column 2 from dataframename

`dataframename[[2]] <- NULL` - Also removes column 2

`dataframename$colname <- NULL` - Removes the column `colname`

`NULL` - the null object

`is.null(x)` - tests whether `x` is `NULL`


You can subset a data frame like a matrix:

```R
dataset.selection <- dataset[dataset$variable!="criteria",]
dataset.selection
```

It can be easier to use `subset()`:
```R
subset(dataset, variable!="criteria")
```

`order(dataset$variable)` - Use `order()` to sort a dataset by a column variable e.g.

```R
permut <- order(chol$age)      # Create the permutation order by age
chol <- chol[permut,]          # Apply this permutation to the entire data set
chol
```

`which.max()` - find maximum value e.g. `cia[which.max(cia$Life),]`
`which.min()` - find minimum value 

`head()` - get first few rows of dataset

`merge(dataframeA, dataframeB, by.x="keycolumnA", by.y="keycolumnB")` - merge two data frames together, use `all.x=TRUE` to replicate `LEFT JOIN` if needed because `INNER JOIN` is the default behaviour. 

## Getting data in and out of R

`load(url("https.....")` - Load data into R

`save(x, file="MyX.RData")` - save `x` to RData file

`read.table("filename.txt", header=TRUE, sep=",", na.strings=".")` - read in csv or txt file

`read.csv("filename.csv", na.strings=".")` - read in csv, assumes `header=TRUE` and `sep=","`

`str()` - Check column data types

`sapply(dataframename, class)` - Another way to check data types have been read in correctly

`write.table(dataframe, file="filename.csv", sep=",", col.names=TRUE, row.names=FALSE)` - export data as table

`write.csv(dataframe, file="filename.csv", row.names=FALSE)` - export data as csv without specifying separator



