# SECTION 2. Introduction to R 

## Why R?

R has a number of advantages compared to other statistical software tools (i.e. SAS, SPSS, Stata, etc ...).

* Open-source (It is free!!!)

* Cross-platform (Windows, Mac, Linux)

* Updated regularly

* Extremely flexible and can do or be made to do joust about anything

* Amazing graphical capabilities


## Creating a new project directory in RStudio

Let's create a new project directory for our "Introduction to R" lesson today. 

1. Open RStudio
2. Go to the `File` menu and select `New Project`.
3. In the `New Project` window, choose `New Directory`. Then, choose `Empty Project`. Name your new directory `Intro-to-R` and then "Create the project as subdirectory of:" the Desktop (or location of your choice).
4. Click on `Create Project`.
5. After your project is completed, if the project does not automatically open in RStudio, then go to the `File` menu, select `Open Project`, and choose `Intro-to-R.Rproj`.
6. When RStudio opens, you will see three panels in the window.
7. Go to the `File` menu and select `New File`, and select `R Script`. 

## RStudio Interface

**The RStudio interface has four main panels:**

1. **Console**: where you can type commands and see output. *The console is all you would see if you ran R in the command line without RStudio.*
2. **Script editor**: where you can type out commands and save to file. You can also submit the commands to run in the console.
3. **Environment/History**: environment shows all active objects and history keeps track of all commands run in console
4. **Files/Plots/Packages/Help**

## Organizing your working directory & setting up

### Viewing your working directory

Before we organize our working directory, let's check to see where our current working directory is located by typing into the console:


``` r
getwd()
```

If you wanted to choose a different directory to be your working directory, you could navigate to a different folder in the `Files` tab, then, click on the `More` dropdown menu and select `Set As Working Directory`or typing into the console:


``` r
setwd(path)
```

### Console command prompt

Interpreting the command prompt can help understand when R is ready to accept commands. Below lists the different states of the command prompt and how you can exit a command:

**Console is ready to accept commands**: `>`.

If R is ready to accept commands, the R console shows a `>` prompt. 

When the console receives a command (by directly typing into the console or running from the script editor (`Ctrl-Enter`), R will try to execute it.

After running, the console will show the results and come back with a new `>` prompt to wait for new commands.


**Console is waiting for you to enter more data**: `+`.

If R is still waiting for you to enter more data because it isn't complete yet,
the console will show a `+` prompt. It means that you haven't finished entering
a complete command. Often this can be due to you having not 'closed' a parenthesis or quotation. 

**Escaping a command and getting a new prompt**: `esc`

If you're in Rstudio and you can't figure out why your command isn't running, you can click inside the console window and press `esc` to escape the command and bring back a new prompt `>`.



## Simple computations with R

R can be used as a calculator. You can just type your equation and execute the
command:



``` r3
2+2

1+2*3-4/5

(19465*0.25)^23

5%%2
```


**Example:** Addition of two values


``` r
3 + 6
```

```
## [1] 9
```
```
# The output is always preceded by a number between brackets: [1]
```


## Math functions

* `log(x)` Natural log. 

* `sum(x)` Sum.

* `exp(x)` Exponential. 

* `mean(x)` Mean.

* `max(x)` Largest element. 

* `median(x)` Median.

* `min(x)` Smallest element. 

* `quantile(x)` Percentage quantiles.

* `round(x, n)` Round to n decimal places.

* `rank(x)` Rank of elements.

* `var(x)` The variance.

* `cor(x,y)` Correlation. 

* `sd(x)` The standard deviation.


 
## Variable Assignment

You can assign a number to a name.

``` r
x <- 3
```
Now "x" is called a variable and it appears in the **workspace window**, which means
that R stores the value of "x" in its memory and it can be used later.

In general, by using the <-, you can assign a value to an object

If you type the name of a variable, the current value of the variable will be printed

``` r
x
```

```
## [1] 3
```
There are variables that are already defined in R, like variable "pi"

``` r
pi
```

```
## [1] 3.141593
```
Calculating the perimeter of the circumference with radius 3

``` r
2 * pi * x
```

```
## [1] 18.84956
```
Changing the value of radius and reusing the code

``` r
x <- 5
2 * pi * x
```

```
## [1] 31.41593
```

**Remarks**

1. R is case sensitive

``` r
A <- 33
a <- 44
A 
```

```
## [1] 33
```

``` r
a
```

```
## [1] 44
```

2. The tag # indicates a comment



## Data types

* Numeric data: 1, 2, 3

``` r
x <- c(1, 2, 3); 
x
```

```
## [1] 1 2 3
```

``` r
is.numeric(x)
```

```
## [1] TRUE
```

``` r
class(x)
```

```
## [1] "numeric"
```

* Character data: "a", "b", "c"


``` r
x <- c("1", "2", "3"); x
```

```
## [1] "1" "2" "3"
```

``` r
is.character(x)
```

```
## [1] TRUE
```


* Logical data

``` r
x <- 1:10 < 5
x
```

```
##  [1]  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
```

``` r
!x
```

```
##  [1] FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
```

``` r
which(x) # Returns index for the 'TRUE' values in logical vector
```

```
## [1] 1 2 3 4
```


* Factor : Character strings with preset levels. (Needed for some statistical models).


``` r
factor(c("1", "0", "1", "0", "1"))
```

```
## [1] 1 0 1 0 1
## Levels: 0 1
```


## Create new data

Using functions as  `seq`, `rep` and `sample` we can generate new information. 


``` r
seq(10,80, 2)
```

```
##  [1] 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40 42 44 46 48 50 52 54 56 58
## [26] 60 62 64 66 68 70 72 74 76 78 80
```

``` r
rep(c("A","B"), 2); rep(c("A","B"), each=2)
```

```
## [1] "A" "B" "A" "B"
```

```
## [1] "A" "A" "B" "B"
```

``` r
sample(seq(10,80, 2),5)
```

```
## [1] 80 62 38 76 56
```


## Structures

* Vectors (1D)

``` r
myVec <- 1:10; names(myVec) <- letters[1:10]

#Subsetting by index/position numbers
myVec[1:5]
```

```
## a b c d e 
## 1 2 3 4 5
```

``` r
myVec[c(2,4,6,8)]
```

```
## b d f h 
## 2 4 6 8
```

``` r
# Subsetting by field names
myVec[c("b", "d", "f")]
```

```
## b d f 
## 2 4 6
```

``` r
#Subsetting by same length logical vectors
myLog <- myVec > 10


##Generating new vectors
vec1<-seq(10,20,5)
vec2<- (1:9)
vec3<- c(vec1, vec2)

##combining information
x<-"Hello"
paste0(x,"World")
```

```
## [1] "HelloWorld"
```

``` r
paste(x,"World", sep=" ")
```

```
## [1] "Hello World"
```


* Matrices (2D): two dimensional structures with data of same type

``` r
myMA <- matrix(1:30, 3, 10, byrow = TRUE)
class(myMA)
```

```
## [1] "matrix" "array"
```

``` r
# Subsetting by rows:
myMA[1:2,]
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
## [1,]    1    2    3    4    5    6    7    8    9    10
## [2,]   11   12   13   14   15   16   17   18   19    20
```

* Data Frames (2D): two dimensional structures with variable data types

``` r
myDF <- data.frame(Col1=1:10, Col2=10:1)

# Subsetting by rows:
myDF[1:2, ]
```

```
##   Col1 Col2
## 1    1   10
## 2    2    9
```

``` r
# Subsetting by field names
myDF$Col1
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

* Lists: containers for any object type

``` r
myL <- list(name="Fred", wife="Mary", no.children=3, child.ages=c(4,7,9))
myL
```

```
## $name
## [1] "Fred"
## 
## $wife
## [1] "Mary"
## 
## $no.children
## [1] 3
## 
## $child.ages
## [1] 4 7 9
```

``` r
myL[[4]][1:2]
```

```
## [1] 4 7
```



## The environment


* `ls()` List all variables in the environment.

* `rm(x)` Remove x from the environment.

* `rm(list = ls())` Remove all variables from the environment.


**Remarks**
You can use the environment panel in RStudio to browse variables in your environment.




## Using scripts

* However, instead of working directly on the R console it is usually more convenient to use R scripts.

An R script is a text file where you can type the commands that you want to
execute in R. 

Scripts have file names with the extension .R, for instance, "myscript.R". The R script is where you keep a record of your work. Using R scripts is very convenient because all R commands used in a session are saved in the script file and can be executed again in a future session.

To create a new R script go to

*File -> New -> R Script* 

To open an existing R script go to

*File -> Open -> R Script->select your script*

If you want to run a line from the script window, you place the cursor in any place of the line and click *Run* or press *CTRL+ENTER* if you are using Windows/Linux or  *Command+Enter* if you are using MAC.

You can execute part of a script (or the whole script) by selecting the corresponding lines and pressing *Run* or 
*CTRL+ENTER* or *Command+Enter*. 

You can also execute the whole script by using the R function `source( )`
```
source("scriptname.R")
```

*  Read Section 5 "Scripts" from document ["A (very) short introduction to R"](http://cran.r-project.org/doc/contrib/Torfs+Brauer-Short-R-Intro.pdf)
by P. Torfs and C. Brauer





## R basic base functions

Usual tasks in R involve functions. R comes with a slew of pre-installed functions. These functions are installed as part of the base package which is located in your ''library' directory. 

An R function is used by typing its name followed by its arguments (also called parameters) between parentheses.

**Example:** 
`seq( )` is a function for generating a sequence of numbers. Its arguments are `arg1=from`, which specifies the first number of the sequence, `arg2= to`, last number of the the sequence,  and `arg3=by`, the increment of the sequence.


``` r
seq(10,80, 2) # generates a sequence from 10 to 20 with increment 2  
```

```
##  [1] 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40 42 44 46 48 50 52 54 56 58
## [26] 60 62 64 66 68 70 72 74 76 78 80
```

The number between brackets at the beginning of each line of the output indicates the position of the first element of each row: 10 is the first element of the output and 56 is the 24th element of the output.


* R treats all functions like 'objects'.
* All functions have names and take arguments in parentheses: 'function()'

For example, complex operations often requires an input and gives and output. This is done with the function 'print()'.


``` r5
print("Hello world")
print(exp)
```


## Reading and writing data files


* `read.table( )`: this function reads a text data file and stores the values into a data frame  

`header=T` : first row of the data file contains the names of the columns or variables
`sep=""` : the values of each row are separated by a space
`sep="\t"` : the values of each row are separated by a tabulation
`sep=","` : the values of each row are separated by a comma
`dec="."`: the decimal symbol is a point


**Example**


``` r
example <- read.table("treatment.txt", header=F, sep="")
# This instruction reads file "treatment.txt" and creates the dataframe "example"
```


* `write.table( )`: this function writes a data frame into a text file

**Example**


``` r
write.table(treatment, file="treatment.txt", row.names=FALSE)
# This instruction writes the dataframe "treatment" in file "treatment.txt"
# row.names=FALSE prevents R for printint the names of the rows (or just the row numbers) in the output file
```


## Libraries/Packages
R comes with a standard set of packages. Others are available for download and
installation.

**Standard packages:** The standard (or base) packages are considered part of the R
source code. They contain the basic functions that allow R to work, and the
datasets and standard statistical and graphical functions. They should be
automatically available in any R installation.

**Contributed packages:** There are thousands of contributed packages for R,
written by many different authors. Some of these packages implement specialized
statistical methods. Some (the recommended packages) are distributed with every
binary distribution of R.

Most packages are available for download from **CRAN** and other repositories such as
**Bioconductor**, a large repository of tools for the analysis and comprehension of
high-throughput genomic data.

Once installed, a package has to be loaded into the session to be used.

### Install packages from CRAN

Using an R package or library for the first time requires two steps: installing the library and loading the library with the following functions:

`install.packages()`:  Install the package

`library()`:  load the package

**Example:** How to install and load the library "survival"?


``` r
install.packages("survival") # you only need to do this once
library(survival) # load library
```


### Install packages from GitHub

To install packages grom GitHub make sure you have the newest version of the `devtools` package. To do so run:


``` r
install.packages("devtools")
```

Then you have two options:

* Use the `install_github()` function.


``` r
install_github("Momocs",username="jfpalomeque")
```

* Another way is trying to download the zip file and install it with the normal `install.packages()` function in R with:


``` r
install.packages(file_name_and_path, repos = NULL, type="source")
```


## Help and documentation

`help( )` or `?` : provides information about a function or an object

**Example:**


``` r
help(mean)
?mean
```

`help.search( )`: provides information about a topic

**Example:** 


``` r
help.search("logistic regression")
```



## Interesting references:

[R Tutorial. An R Introduction to Statistics](http://www.r-tutor.com/)

[QUICK-R](http://www.statmethods.net/index.html)

[Cookbook for R](http://www.cookbook-r.com/)

[Gaston Sanchez blog](http://gastonsanchez.com/blog/)

[An Introduction to R from R Development Core Team](https://cran.r-project.org/doc/manuals/r-release/R-intro.html)

[Programming in R by T.Girke](https://sites.google.com/a/bioinformatics.ucr.edu/bioinformatics-manuals/home/programming-in-r)

[R & Bioconductor  by T.Girke](http://manuals.bioinformatics.ucr.edu/home/R_BioCondManual)



## Exercises 

### Part I

Create a new script called *_"intro_R_xxx.R"_* (replace xxx by your surname) that contains the code of the following exercises. 

1. Code to execute a script called "myscript.R"


2. Code to assign the value A to a variable x 


3. Code to generate a sequence from 7 to 30 with increment 3


4. Code to obtain information about function glm


5. Code to list all the objects in the current environment



6. Code to remove all objects 


7. Code to specify the following path to the working directory: C:\Desktop\Biostatistics 




8. Create a vector x containing the numbers 1, 2, 1, 1, 1, 2 


9. Create a vector y containing the words yes, no, no, yes, no 


10. Compute the number of elements in vector y


11. Code to obtain the sequende of integer numbers from 10 to 25


12. Use the function rep() to generate the sequence 1, 2, 1, 2, 1, 2 


13. Code to generate the sequence 1, 1, 1, 2, 2, 2


14. Code to generate a sequence containing 7 yes and 5 no


15. Code to obtain the sequence 40, 35, 30, 25, 20, 15, 10



### Part II

This exercise uses data describing 128 microarray samples as a basis for exploring R functions. Covariates such as age, sex, type, stage of the disease, etc., are in a data file `pData.txt`. 

* Input the text file using `read.table`, assigning the input to a variable pdata.





* Find the class of the variables *pheno* and *sex*. Convert them into factors using `as.factor`.




* Show the 10 first values of the variable "age". 



* Repeat the previous values, each 3 times.



* Create a new data.frame "pdata_subset" containing the first 20 rows. 



* Add in the previous dataset a new column of random values "Values", that goes from 0.05 to 0.95.  



* Create a new matrix "Data_matrix" containing the information of the variables "X1", "X3" and "X5", uniquely for the last 10 observations of the original "pdata" dataset.



* Print a sentence indicating the dimensions (rows and columns) of the matrix. Use the function `print`, `paste`, `nrow` and `ncol` to do so. The sentence should be "The matrix has XXXX rows and XXX columns".







---

![](logouvic.png)

