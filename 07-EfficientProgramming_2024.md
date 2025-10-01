# SECTION 6. Tips for efficient programming





A major benefit of using R (as opposed to other programming languages), is that coding time is greatly reduced. However if we are not careful, it's very easy to write programs that are incredibly slow. While optimization such as going parallel can easily double speed, poor code can easily run 100s of times slower. For this reason a priority of an efficient programmer should be to avoid the following common mistakes. If you spend any time programming in R, this [book](https://csgillespie.github.io/efficientR/index.html) should be considered essential reading.

## Top tips for efficient programming

Let's explore some fundamental strategies to optimize the efficiency of your R code:

*1.* Vectorization:

Tip: Leverage vectorized operations for faster and more concise code.


``` r
result_vectorized <- (1:10) + 1
```

*2.* Avoid growing vectors:

Tip: Preallocate memory for vectors to avoid unnecessary copying and enhance performance.


``` r
result <- numeric(10)
for (i in 1:10) {
  result[i] <- i + 1
}
```

*3.* Use apply functions instead of loops:

Tip: Replace explicit loops with apply functions for improved readability and efficiency.


*4.* Explore other packages (reutilize others work!!):

Tip: Investigate specialized packages like `data.table`, `tidyverse`, and `dplyr` for optimized data manipulation.

*5.* Utilize helpful tools:

Tip: Employ tools such as `microbenchmark`, `compiler`, and `memoise` for benchmarking, code compilation, and caching, respectively. Use `rm(list=ls())` to empty your working environment, `rm(list=setdiff(ls(), "x"))` to remove everything except "x" and `gc()` for garbage collection and free up memory.


## Optimizing functions calls

Efficient R programming involves tapping into underlying C/Fortran routines swiftly. Every R function call eventually triggers corresponding C/Fortran code. For instance, the base R function `runif()` is essentially a call to `C_runif()`.

Consider the following illustration using a standard R vector x of length n:


``` r
x= 1:10
x = x + 1
```

In this example, a single function call to the `+` function efficiently handles the vector operation. Conversely, using a loop involves multiple function calls for each element, resulting in slower performance due to the cumulative effect of numerous calls.


``` r
for(i in 1:n) {
  x[i] <- x[i] + 1 
}
```

While the individual function calls in the loop are quick, the aggregate effect of multiple calls can significantly impact performance. Therefore, minimizing function calls and embracing vectorized operations contribute to a more efficient and faster execution of R code.

## Memory allocation 

An essential aspect of writing efficient R code is prudent memory allocation. Pre-allocating memory, in particular, stands out as a crucial practice. Let's delve into three methods of creating a sequence of numbers and compare their performance.

* Method 1: Dynamic growth


``` r
method1 <- function(n) {
  myvec <- NULL
  for(i in 1:n) {
    myvec <- c(myvec, i)
  }
  return(myvec)
}
```

* Method 2: Pre-allocation with subscripting 


``` r
method2 <- function(n) {
  myvec <- numeric(n) 
  for(i in 1:n) {
    myvec[i] <- i
  }
  return(myvec)
}
```

* Method 3: Direct creation of the final object


``` r
method3 <- function(n) {
  myvec <- 1:n
  return(myvec)
}
```

To compare the three methods we use the `microbenchmark` function from the previous chapter



By adopting memory-efficient practices, such as pre-allocating vectors (or direct creation of the final object), you can significantly enhance the performance of your R code, especially when dealing with large datasets or extensive computations.


``` r
n = 10000
summary(microbenchmark(method1(n), method2(n), method3(n))) ## time in nanoseconds
```

```
##         expr       min        lq      mean    median        uq       max neval
## 1 method1(n) 178660800 190593850 205741501 200510450 214886200 295400900   100
## 2 method2(n)    358500    413250    485332    431250    456950   3398300   100
## 3 method3(n)       400       900     18493      2850      8800   1343700   100
##   cld
## 1  a 
## 2   b
## 3   b
```

In the output you can see different columns: 

*expr:* names of the expressions being benchmarked

*min:* minimum execution time over `times` evaluations

*lq:* lower quartile (Q1-25th percentile) of execution time, i.e., 25% of executions took this time or less

*mean:* average time over `times` evaluations

*median:* median time (50th percentile) of execution time

*uq:* upper quartile (Q3-75th percentile) of execution time, i.e., 75% of executions took this time or less

*max:* maximum execution time over `times` evaluations

*neval:* number of evaluations 

*cld:* compact letter display tells you which methods are statistically significant different based on performance. Methods in group `a` are significantly slower than methods in group `b`


## Embrace R's vectorized paradigm 

Consider the following examples to illustrate the shift towards a more vectorized approach. For example:


``` r
x <- runif(1000) + 1
logsum <- 0
for(i in 1:length(x)) {
  logsum <- logsum + log(x[i]) 
}
```

is a piece R code that has a strong, unhealthy influence from C. Instead we should write


``` r
logsum <- sum(log(x))
```

The vectorized approach is not only more concise but also faster, taking full advantage of R's strengths.


``` r
summary(microbenchmark(
  loop_approach = {
    logsum <- 0
    for(i in 1:length(x)) {
      logsum <- logsum + log(x[i])
    }
  },
  vectorized_approach = {
    logsum <- sum(log(x))
  }
))
```

```
##                  expr    min     lq     mean  median      uq    max neval cld
## 1       loop_approach 2448.1 2589.0 3159.228 2844.45 3405.85 7643.4   100  a 
## 2 vectorized_approach   34.1   35.3   39.448   38.55   40.80   86.9   100   b
```

Another common example is sub-setting a vector.


``` r
ans <- NULL
for(i in 1:length(x)) {
  if(x[i] < 0) {
    ans <- c(ans, x[i])
  }
}
```

This can be done simply with:

``` r
ans <- x[x < 0]
```


## Efficient functions: Do as little as possible

Improving the efficiency of your functions often boils down to making them do less work. Here are some strategies and examples to optimize your functions:

*1.* Use specific vectorized functions:

Functions like `rowSums()`, `colSums()`, `rowMeans()`, and `colMeans()` outperform equivalent operations using `apply()` because they are inherently vectorized.

Example:


``` r
matrix_data <- matrix(1:20, nrow = 4, ncol = 5, byrow = TRUE)
summary(microbenchmark(rowSums(matrix_data),apply(matrix_data, 1, sum)))
```

```
##                         expr  min    lq   mean median    uq   max neval cld
## 1       rowSums(matrix_data)  4.2  4.55  6.785    5.4  5.80  83.0   100  a 
## 2 apply(matrix_data, 1, sum) 24.6 25.00 29.643   25.4 27.45 124.8   100   b
```

*2.* Choose appropriate function for specific tasks:

Opt for specialized functions like `vapply()` over general-purpose ones like `sapply()`.

Example:


``` r
data <- list(a = c(1, 2, 3), b = c(4, 5, 6), c = c(7, 8, 9))

vapply_result <- vapply(data, function(x) mean(x), FUN.VALUE=numeric(1))
vapply_result
```

```
## a b c 
## 2 5 8
```

``` r
sapply_result <- sapply(data, function(x) mean(x))
sapply_result
```

```
## a b c 
## 2 5 8
```

``` r
summary(microbenchmark(vapply(data, function(x) mean(x), numeric(1)),sapply(data, function(x) mean(x))))
```

```
##                                            expr  min   lq   mean median    uq
## 1 vapply(data, function(x) mean(x), numeric(1)) 23.4 24.3 27.179   25.0 25.85
## 2             sapply(data, function(x) mean(x)) 37.4 38.3 42.629   39.1 40.25
##     max neval cld
## 1  93.5   100  a 
## 2 188.4   100   b
```

The `numeric(1)` argument specifies that the output should be of type numeric and length 1. This specification enhances performance and makes it faster. 

*3.* Optimize equality testing: 

If you want to see if a vector contains a single value, `any(x == 10)` is much faster than `10 %in% x`. This is because testing equality is simpler than testing inclusion in a set.

*4.* Understand data type coercion:

Some functions coerce inputs into specific types. Thus, if your input is not the right type, the function has to do extra work. Choose functions that work with your data as it is or consider adapting your data format.The most common example of this problem is using `apply()` on a data frame. `apply()` always turns its input into a matrix. Not only is this error prone (because a data frame is more general than a matrix), it is also slower.

*5.* Provide additional information: 

Other functions will do less work if you give them more information about the problem. It's always worthwhile to carefully read the documentation and experiment with different arguments. Some examples that I've discovered in the past include:

* `read.csv()`: specify known column types with `colClasses` can enhance parsing efficiency.

* `factor()`: specify known levels with levels ensures that the factor levels are correctly assigned, avoiding potential errors and enhancing the function's accuracy.

* When unlisting a nested list, using `use.names = FALSE` in `unlist()` can significantly improve performance by avoiding the preservation of the names of the list elements, which slow down the operation.


``` r
l.ex <- list(a = list(1:5, LETTERS[1:5]), b = "Z", c = NA)
l.ex
```

```
## $a
## $a[[1]]
## [1] 1 2 3 4 5
## 
## $a[[2]]
## [1] "A" "B" "C" "D" "E"
## 
## 
## $b
## [1] "Z"
## 
## $c
## [1] NA
```

``` r
unlist(l.ex)
```

```
##  a1  a2  a3  a4  a5  a6  a7  a8  a9 a10   b   c 
## "1" "2" "3" "4" "5" "A" "B" "C" "D" "E" "Z"  NA
```

``` r
unlist(l.ex,use.names=F)
```

```
##  [1] "1" "2" "3" "4" "5" "A" "B" "C" "D" "E" "Z" NA
```

* `interaction()`: if you only need combinations that exist in the data, use `drop = TRUE`.


``` r
factor_1 <- factor(c("A", "B", "A"))
factor_2 <- factor(c("X", "Y", "X"))

interaction(factor_1, factor_2)
```

```
## [1] A.X B.Y A.X
## Levels: A.X B.X A.Y B.Y
```

``` r
interaction(factor_1, factor_2,drop =T)
```

```
## [1] A.X B.Y A.X
## Levels: A.X B.Y
```

## References
* Quick intro into Parallel Computing in R: https://nceas.github.io/oss-lessons/parallel-computing-in-r/parallel-computing-in-r.html 
* Parallel Programming in R: https://www.geeksforgeeks.org/parallel-programming-in-r/  
* "Efficient R programming" book by Colin Gillespie and Robin Lovelace: https://csgillespie.github.io/efficientR/index.html 
* "The R Language: An Engine for Bioinformatics and Data Science" by Giorgi et al.: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9148156/ 
* "Advanced R": https://adv-r.hadley.nz/
* Rcpp: Seamless R and C++ Integration: https://www.jstatsoft.org/article/view/v040i08
* Benchmarks comparisons: https://github.com/Rdatatable/data.table/wiki/Benchmarks-%3A-Grouping 


![](logouvic.png)
