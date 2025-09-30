# SECTION 4. Introduction to Functions

## What is a function?

A function is an interface to the code, explicitly specified by a set of parameters, that allows to encapsulate an R code that need to be executed numerous times (i.e., reusable blocks), perhaps under slightly different conditions (thanks to the arguments). 

In R, we can distinguish between different types of functions: built-in R functions, functions defined in R packages and user-defined functions. 

### Built-in R functions

Built-in R functions are those functions that you can call in R without the need of installing or loading any external package from an on-line repository. All built-in functions are contained in the `{base}` R package which is automatically loaded when you start an R session. Overall, built-in functions in R are thought to compute basic operations, data manipulation, arithmetic, statistical analysis. You already know some of them: 

* `sum()`
* `mean()`
* `str()`
* `head()`
* `length()`
* `dim()`
* `order()`
* `boxplot`
* `class()`

Functions are R objects. Then, the class of a function is, of course, a "function":


``` r
class(sum)
```

```
## [1] "function"
```
### Functions defined in R packages

Packages are collections of functions that typically serve specific purposes. For example, `{ggplot2}` is a package which contains functions for data visualization, and `{readxl}` facilitates working with Excel files. R packages can be obtained from various online repositories like CRAN, Bioconductor, or GitHub.

When you install R packages, they are stored in a designated directory or folder on your computer, known as the "library" or "library path". By default, R utilizes two libraries: a system library for base packages (located in the R installation folder) and a user library for external repository packages. You can find out where your packages are installed by default by running `.libPaths()`.

To use the functions provided by R packages, you need to load the specific package into your current R session. This can be done using either `library(package.name)` or `require(package.name)`.

### User-defined functions

User-defined functions in R are functions that you, as a programmer, create and define to perform specific tasks or operations that are not already covered by built-in functions. These functions are written in R script and allow you to encapsulate a series of operations into a reusable unit. 

Learning to create user-defined functions in R is a significant milestone on your journey to becoming a programmer, not merely a consumer of the R language. It empowers you to write your own code, tailor R to your specific needs, and solve complex problems with custom solutions.

As you become more proficient in creating user-defined functions, you may discover that your functions are not only valuable to you but also to the broader R community. You can package your functions, documentation, and data into your own R packages.

## Writing your own functions

You can program your own functions in R by using the language construct `function()`. You can use this construct to specify the function's name, the arguments or parameters between brackets and the body of your function between curly brackets.


``` r
functionName <- function(arguments){
  statements
}
```

* `functionName`: This is the name you choose for your function. It should be a valid R variable name.
* `arguments`: Variables or parameters that you pass to the function when you call it. They allow you to provide input to a function and customize its behaviour. 
* `statements`: The body of the function which is enclosed within curly brackets `{}` and contains the code that performs the desired computations.

You can execute the function by calling it with the specified arguments:


``` r
functionName(arguments)
```

Let's create a function that return the square of a number: 


``` r
square <- function(x) {
  x*x
}
```

Now, `square()` is stored in your global environment. Once a function is written, it can be used just by invoking the name of the function followed by the values that we pass to the arguments, separated by commas and between parentheses. 


``` r
square(x = 5)
```

```
## [1] 25
```

## Arguments of a function 

There are different ways to know which are the arguments of a function. We can press the the *tab key* while writing the arguments or, alternatively, use the directive `args()`.


``` r
args(square)
```

```
## function (x) 
## NULL
```

But, in case that we need to know more things about the arguments of a built-in function from a package, you can use the Help page in R
(Function `lm` from the package `stats`):


``` r
args(lm)
help(lm)
?lm
```

## Matching Arguments

Calling an R function with arguments can be done in two main different ways. Arguments can be matched either **by position**, where the first value is assigned to the first argument, the second value to second argument, and so on, or **by name**, where there is no matter in what order the argument is specified. Notice the difference between passing a numerical vector to the first (and unique) argument of our `square()` function... 


``` r
square(c(1, 3, 5))
```

```
## [1]  1  9 25
```

and this: 


``` r
square(1, 3, 5)
```

```
## Error in square(1, 3, 5): los argumentos no fueron usados (3, 5)
```

Imagine that we would like to generate 50 numbers at random with a normal distribution such that its mean is 1 and its standard deviation is 20. Then, by applying the function `rnorm()`, 50 is assigned to the `n` argument, 1 is assigned to the `mean` argument, and 20 is assigned to the `sd` argument, all by positional matching.


``` r
set.seed(12345)
rnorm(50,1,20)
```

```
##  [1]  12.710576  15.189320  -1.186066  -8.069943  13.117749 -35.359119
##  [7]  13.601971  -4.523682  -4.683195 -17.386440  -1.324956  37.346241
## [13]   8.412557  11.404329 -14.010640  17.337997 -16.727150  -5.631552
## [19]  23.414253   6.974474  16.592438  30.115702 -11.886569 -30.062748
## [25] -30.954190  37.101950  -8.632947  13.407596  13.242470  -2.246220
## [31]  17.237464  44.936671  41.983807  33.648913   6.085424  10.823766
## [37]  -5.481732 -32.241005  36.354677   1.516021  23.570217 -46.607161
## [43] -20.205311  19.742811  18.089034  30.214588 -27.261976  12.348065
## [49]  12.663753 -25.135977
```

It is also possible to mix both methods. Thus, when an argument is matched by name, it is "taken out" from the argument list, and the remaining unnamed arguments are matched in the order that they are listed in the function definition.


``` r
set.seed(12345)
rnorm(50, mean=1, 20)
```

```
##  [1]  12.710576  15.189320  -1.186066  -8.069943  13.117749 -35.359119
##  [7]  13.601971  -4.523682  -4.683195 -17.386440  -1.324956  37.346241
## [13]   8.412557  11.404329 -14.010640  17.337997 -16.727150  -5.631552
## [19]  23.414253   6.974474  16.592438  30.115702 -11.886569 -30.062748
## [25] -30.954190  37.101950  -8.632947  13.407596  13.242470  -2.246220
## [31]  17.237464  44.936671  41.983807  33.648913   6.085424  10.823766
## [37]  -5.481732 -32.241005  36.354677   1.516021  23.570217 -46.607161
## [43] -20.205311  19.742811  18.089034  30.214588 -27.261976  12.348065
## [49]  12.663753 -25.135977
```
Notice that if `mean ` or `sd` are not specified they assume the default values of `0` and `1`. These are **default parameters values** that you can specify in the arguments list when creating your own user-defined function. 


``` r
set.seed(12345)
rnorm(50)
```

```
##  [1]  0.58552882  0.70946602 -0.10930331 -0.45349717  0.60588746 -1.81795597
##  [7]  0.63009855 -0.27618411 -0.28415974 -0.91932200 -0.11624781  1.81731204
## [13]  0.37062786  0.52021646 -0.75053199  0.81689984 -0.88635752 -0.33157759
## [19]  1.12071265  0.29872370  0.77962192  1.45578508 -0.64432843 -1.55313741
## [25] -1.59770952  1.80509752 -0.48164736  0.62037980  0.61212349 -0.16231098
## [31]  0.81187318  2.19683355  2.04919034  1.63244564  0.25427119  0.49118828
## [37] -0.32408658 -1.66205024  1.76773385  0.02580105  1.12851083 -2.38035806
## [43] -1.06026555  0.93714054  0.85445172  1.46072940 -1.41309878  0.56740325
## [49]  0.58318765 -1.30679883
```
It is often useful specifying an argument by its name when a function has many arguments. For example, a function with a large list of arguments is `lm()`, that allows to fit a linear model to a dataset. Other functions with a huge number of arguments are those functions used for generating plots (e.g. `plot`, `hist`, `boxplot`,...)

## Output of a Function

As a result, a function usually should return an object.

``` r
f1 <- function(x, a = 3, b = 1){ 
  a * x + b 
}
f1(3,5,8)
```

```
## [1] 23
```

Notice that, objects created within a function are in a local environment: 


``` r
f2 <- function(x, a = 3, b = 1){
  y <- a * x + b
}
f2(3)
```

They can be returned from the function to the global environment by keeping it in a variable, and then, print the variable:


``` r
num <- f2(3)
print(num)
```

```
## [1] 10
```
In addition, in the body of function, `return()` can be used to specify the output of the  function. 


``` r
functionName <- function(arguments){
  statements
  return(something)
}
```


``` r
f3 <- function(x, a = 3, b = 1){ 
  y <- a * x + b
  return(y)
}
f3(3)
```

```
## [1] 10
```


### Examples of functions

The following function computes the perimeter of a circumference. The function's name is "Perimeter"", the argument is the radius of the circumference 

``` r
Perimeter <- function(x) {
  p <- 2 * pi * x
  return(p)
}
Perimeter(4)
```

```
## [1] 25.13274
```

You can use the perimeter function to calculate the perimeter of a circumference of any radius x.

``` r
Perimeter(x = 3)
```

```
## [1] 18.84956
```


``` r
Perimeter(x = 5)
```

```
## [1] 31.41593
```

Function to compute the area of a rectangle:

``` r
Area <- function(x,y) {
  x*y ## length*width
}

Area(10,7)
```

```
## [1] 70
```


**Try to create this function:** Imaging we want to create a function to compute both the perimeter (`2*x+2*y`) and area of a rectangle. In this case we will need to return two outputs. 





## Basic R Programming

In this section we'll learn about control-flow constructs in R. These constructs are used to control the execution and flow of the program based on conditions provided. 

### Conditionals

#### `if()` statement:

One of the most basic programming concepts involves evaluating conditional statements and then performing some action based on the evaluation. Let's write a very simple conditional using the `if()` control-flow construct. The `if()` construct is simply a control statement that takes a conditional statement (involving logical operators) as its argument and then depending on the evaluation initiates some other function.


``` r
if(1 + 1 == 2) { ##logical statement!
  print ("Perfect!")
}
```

```
## [1] "Perfect!"
```

You can see that in this case the `if()` statement evaluated to TRUE and it activated the `print()` function. If the `if()` statement were FALSE nothing would happen. 


``` r
if(1 + 1 == 3) {
  print("Perfect!")
}
```

#### `else()` statement:

If we wanted R to also do something if the conditional is FALSE we would have to add the else statement.


``` r
if(1 + 1 == 3) {
  print ("Perfect!")  
}else{ print("Wrong")}
```

```
## [1] "Wrong"
```


#### `ifelse()` statement

Instead of using the `if()` and else control statements, you can just use the `ifelse()` function. The function takes the arguments test, yes and no.


``` r
Z <- 1:10
Z.new <- ifelse(test = Z > 2 & Z < 8 , yes = " Yes " , no = " No")
Z.new
```

```
##  [1] " No"   " No"   " Yes " " Yes " " Yes " " Yes " " Yes " " No"   " No"  
## [10] " No"
```

### Loops

#### `for()` statement

A `for` loop is a programming construct used when you want to perform a task repeatedly, such as iterating through a sequence, a list, or a range of numbers. It automates repetitive tasks, saving time and effort. The structure of a `for` loop contains: 


``` r
for (loop_variable in sequence) {
  # Loop body: Code to be executed in each iteration
}
```

* loop_variable: this value is used to keep track of the current iteration
* sequence: defines the range of values that the loop variable will take on each iteration
* loop body: contains the code to be executed in each iteration

The easiest way to understand how to use the `for()` operator is by example:


``` r
x <- c(20:30) 
for(i in x) { 
  print(i)
}
```

```
## [1] 20
## [1] 21
## [1] 22
## [1] 23
## [1] 24
## [1] 25
## [1] 26
## [1] 27
## [1] 28
## [1] 29
## [1] 30
```
As you can see, the `for` loop start by initializing the **loop variable** with the first value from the **sequence**. As output, we  don't obtain the numeric vector 'x', but rather we consecutively print each one of its values. 

``` r
x
```

```
##  [1] 20 21 22 23 24 25 26 27 28 29 30
```

**Try to solve this problem with a for loop:** provide the sum of the first 10 digits, that's, 1 + 2 + 3 + 4 + 6 + 7 + 8 + 9 + 10. 



**Try to solve this problem with a for loop:** generate 100 values from a standard normal distribution and sum up those values that are larger than 1.3



For and if loops are useful but are computationally not very efficient. You should always think whether there is an alternative R code that does not require loops.

#### The `while()` statement

The `while` loop starts by evaluating the condition inside the parentheses. If the condition is `TRUE`, the code inside the loop is executed.


``` r
count <- 1  
while (count <= 5) {
  print(count)
  count <- count + 1 
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

``` r
count
```

```
## [1] 6
```
Be cautious about the potential for infinite loops, and include safeguards like a maximum iteration count or an escape condition (use `break`):


``` r
count <- 1
while (is.numeric(count)) {
  print(count)
  if (count == 10) {
    break
  }
  count <- count + 1
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
## [1] 6
## [1] 7
## [1] 8
## [1] 9
## [1] 10
```

#### Avoid loops whenever possible

R is a vector-oriented language, which often allows us to avoid writing explicit loops.

Instead of summing two vectors with the following code:


``` r
for(i in 1:length(a)) {
  c[i] = a[i] + b[i]
}
```

R is designed to do all this in a single vectorized operation:


``` r
c = a + b
```

Compare the execution time of these two processes:


``` r
x <- runif(1000000)
y <- runif(1000000)
system.time(z <- x + y)
```

```
##    user  system elapsed 
##       0       0       0
```

``` r
system.time(for (i in 1:length(x)) z[i] <- x[i] + y[i])
```

```
##    user  system elapsed 
##    0.08    0.00    0.08
```

### Useful functions to avoid loops

The `apply()` family of functions in R is a set of powerful tools for applying a function to the rows or columns of matrices, arrays, or data frames and avoid using loops in R whenever possible.

#### Function `apply()`

 `apply( )` is used when we want to apply the same function to every row or column of a matrix or array. 

This function has 3 arguments: `arg1=name of the matrix`, `arg2=1` if the function is to be applied to the rows or `arg2=2` if the function is to be applied to the columns, `arg3=function`

`apply(array, margin, function, ...)`

Note that an array in R is a very generic data type; it is a general structure of up to eight dimensions. For specific dimensions there are special names for the structures. A zero dimensional array is a scalar or a point; a one dimensional array is a vector; and a two dimensional array is a matrix, etc.


**Example:** Assume that `x` is a matrix of numbers with 4 rows and 3 columns as defined below

``` r
mat<-matrix(seq(1:12), nrow=4)
mat
```

```
##      [,1] [,2] [,3]
## [1,]    1    5    9
## [2,]    2    6   10
## [3,]    3    7   11
## [4,]    4    8   12
```

To compute the mean of the rows

``` r
apply(mat,1,mean) 
```

```
## [1] 5 6 7 8
```

To compute the mean of the columns

``` r
apply(mat,2,mean) 
```

```
## [1]  2.5  6.5 10.5
```

Using a user defined function

``` r
sum.plus.2 <- function(x){
	sum(x) + 2
}

apply(mat, 1, sum.plus.2) ##sum by row & +2
```

```
## [1] 17 20 23 26
```

Also, the function can be defined inside the apply function as an **anonymous function**. Note the lack of curly brackets:

``` r
apply(mat, 1, function(x) sum(x) + 2)
```

```
## [1] 17 20 23 26
```

More generalized


``` r
apply(mat, 1, function(x, y) sum(x) + y, y=3)
```

```
## [1] 18 21 24 27
```


#### Function `lapply()`

`lapply()` function applies a function to elements in a list or a vector and returns the results in a list.

The `lapply()` function becomes especially useful when dealing with data frames. In R the data frame is considered a special type of list, where each column is considered an element of the list. Notice, how we can access to the elements contained in 'mat.list' and 'mat1.df' in a similar ways: 


``` r
mat.list <- list( X1 = 1:4, X2=5:8, X3 = 9:12)
mat.list
```

```
## $X1
## [1] 1 2 3 4
## 
## $X2
## [1] 5 6 7 8
## 
## $X3
## [1]  9 10 11 12
```

``` r
mat.list$X1
```

```
## [1] 1 2 3 4
```

``` r
mat.list[2]
```

```
## $X2
## [1] 5 6 7 8
```


``` r
mat1.df <- data.frame(mat)
mat1.df
```

```
##   X1 X2 X3
## 1  1  5  9
## 2  2  6 10
## 3  3  7 11
## 4  4  8 12
```

``` r
is.list(mat1.df)
```

```
## [1] TRUE
```

``` r
mat1.df$X1
```

```
## [1] 1 2 3 4
```

``` r
mat1.df[[2]]
```

```
## [1] 5 6 7 8
```


We can therefore apply a function to all the variables in a data frame by using the `lapply` function. Note that unlike in the apply function there is no margin argument since we are just applying the function to each component of the list.

In the data frame `mat1.df` the variables `mat1.1 - mat1.3` are elements of the list `mat1.df`. These variables can thus be accessed by `lapply` for example to obtain the sum of each variable in `mat1.df`


``` r
lapply(mat1.df, sum)
```

```
## $X1
## [1] 10
## 
## $X2
## [1] 26
## 
## $X3
## [1] 42
```

``` r
class(lapply(mat1.df, sum)) ## returns list
```

```
## [1] "list"
```

``` r
apply(mat1.df, 2, sum)
```

```
## X1 X2 X3 
## 10 26 42
```

``` r
class(apply(mat1.df,2, sum)) ## returns integer vector
```

```
## [1] "integer"
```

#### Function `sapply()`

Applies a function to elements in a list or a vector and returns the results in a vector, matrix or a list.

`sapply(list, function, ..., simplify)`

When the argument `simplify=F` then the `sapply` function returns the results in a list just like the `lapply` function. However, when the argument `simplify=T`, the default, then the `sapply` function returns the results in a simplified form if at all possible. If the results are all scalars then `sapply` returns a vector. If the results are all of the same length then `sapply` will return a matrix with a column for each element in list to which function was applied.


``` r
lapply(mat.list, function(x, y) sum(x) + y, y = 5)
```

```
## $X1
## [1] 15
## 
## $X2
## [1] 31
## 
## $X3
## [1] 47
```

``` r
sapply(mat1.df, function(x, y) sum(x) + y, y = 5)
```

```
## X1 X2 X3 
## 15 31 47
```

 

## References 
1. [Explore Datacamp tutorials](https://www.datacamp.com/tutorial/functions-in-r-a-tutorial)
2. [Control Statements in R Programming](https://www.geeksforgeeks.org/control-statements-in-r-programming/?ref=rbp)

--- 

![](logouvic.png)
