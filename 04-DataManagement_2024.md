# SECTION 3. Basic data management

## Accessing information and creating new variables

In a typical research project, you'll need to create new variables and transform existing ones. This is accomplished with statements of the form:


``` r
variable <- expression
```

* Example

Let's say that you have a data frame named mydata, with variables `x1` and `x2`,


``` r
mydata <- data.frame(x1=runif(100), x2=runif(100))
head(mydata)
```

```
##          x1        x2
## 1 0.8912161 0.2615591
## 2 0.1758721 0.7891567
## 3 0.9915782 0.4192491
## 4 0.9009444 0.4230279
## 5 0.6072066 0.1008157
## 6 0.9863070 0.7549054
```

and you want to create a new variable `sumx` that adds these two variables and a new variable called `meanx` that averages the two variables. If you use the code

``` r
sumx <- x1 + x2
meanx <- (x1 + x2)/2
```

you'll get an error, because R doesn't know that `x1` and `x2` are from data frame `mydata`.

If you use this code instead, the statements will succeed but you'll end up with a data frame (`mydata`) and two separate vectors (`sumx` and `meanx`). 

``` r
sumx <- mydata$x1 + mydata$x2
meanx <- (mydata$x1 + mydata$x2)/2
head(mydata)
```

```
##          x1        x2
## 1 0.8912161 0.2615591
## 2 0.1758721 0.7891567
## 3 0.9915782 0.4192491
## 4 0.9009444 0.4230279
## 5 0.6072066 0.1008157
## 6 0.9863070 0.7549054
```

Ultimately, you want to incorporate new variables into the original data frame:

``` r
mydata <- data.frame(x1 = c(2, 2, 6, 4), x2 = c(3, 4, 2, 8))
head(mydata)
```

```
##   x1 x2
## 1  2  3
## 2  2  4
## 3  6  2
## 4  4  8
```

``` r
mydata$sumx <- mydata$x1 + mydata$x2
mydata$meanx <- (mydata$x1 + mydata$x2)/2
head(mydata)
```

```
##   x1 x2 sumx meanx
## 1  2  3    5   2.5
## 2  2  4    6   3.0
## 3  6  2    8   4.0
## 4  4  8   12   6.0
```

An alternative is to use the function `attach()` which  specifies the data frame to be used and variables in the data frame can be accessed by simply giving their names. 


``` r
attach(mydata)
```

```
## The following objects are masked _by_ .GlobalEnv:
## 
##     meanx, sumx
```

``` r
x1 + x2
```

```
## [1]  5  6  8 12
```

The function `detach()` removes the attachment: 


``` r
detach(mydata)
```

Also, we can use `with()`:


``` r
mean(mydata$x1)
```

```
## [1] 3.5
```

``` r
with(mydata,mean(x1))
```

```
## [1] 3.5
```

## Data types and data conversions

R provides a set of functions to identify an object's data type and convert it to a different data type.
Type conversions in R work in a similar fashion to those in other statistical programming languages. 
Some functions that you can use are:

* `class()`
* `is.numeric()` - `as.numeric()`
* `is.character()` - `as.character()`
* `is.vector()` - `as.vector()`
* `is.matrix()` - `as.matrix()`
* `is.data.frame()` - `as.data.frame()`
* `is.factor()` - `as.factor()`
* `is.logical()` - `as.logical()`

Functions of the form `is.datatype()`, return `TRUE` or `FALSE`, whereas `as.datatype()` converts the argument to that type.

* Examples

Is "a" numeric, integer, vector? 

``` r
a <- c(1.3,2.1,3.8)
is.numeric(a)
```

```
## [1] TRUE
```

``` r
is.integer(a)
```

```
## [1] FALSE
```

``` r
is.vector(a)
```

```
## [1] TRUE
```

If we force "a" to be transformed to something else...

``` r
a <- as.character(a)
a
```

```
## [1] "1.3" "2.1" "3.8"
```

``` r
is.numeric(a)
```

```
## [1] FALSE
```

``` r
is.vector(a)
```

```
## [1] TRUE
```

``` r
is.character(a)
```

```
## [1] TRUE
```

And again...


``` r
a <- as.integer(a)
a
```

```
## [1] 1 2 3
```

``` r
is.numeric(a)
```

```
## [1] TRUE
```

``` r
is.integer(a)
```

```
## [1] TRUE
```

``` r
is.vector(a)
```

```
## [1] TRUE
```

Notice that: 

*An object can be more than one "type" of data. For instance, vector AND numeric, vector AND character, vector AND numeric AND integer
*R will always identify characters with the following notation: "something". It doesn't matter if inside the quotation marks there are is numbers or letters. 

In R, `factor()` is used to convert a categorical variable into a factor in R: 


``` r
a
```

```
## [1] 1 2 3
```

``` r
a <- factor(x = a, levels = c(1,2,3),labels = c("Group1","Group2","Group3"))
a
```

```
## [1] Group1 Group2 Group3
## Levels: Group1 Group2 Group3
```

In this example, we start with a numeric vector a, and we use `factor()` to convert it into a factor. We specify custom levels and labels to map numeric values to meaningful categories. By default the levels of a factor are ordered alphabetically, being the first level the "reference" level. However you may want to use `relevel()` to change the reference level of a factor variable. It can be useful when you want to set a specific category as the reference for modelling or plotting:


``` r
b <- as.factor(c("male","male","female","male","female"))
b
```

```
## [1] male   male   female male   female
## Levels: female male
```

``` r
b2 <- relevel(x=b, ref="male")
b2
```

```
## [1] male   male   female male   female
## Levels: male female
```
Notice that changing the reference levels does not affect to the original vector. 

We can also use the function `str()` to check the different data types contained in a data frame (i.e., structure). First, load 'rdatos' data frame.


``` r
rdatos <- read.table("RDatosLesson2.txt",header = T,sep=" ")
names(rdatos)
```

```
##  [1] "ID"                       "Age"                     
##  [3] "Gender"                   "Edu"                     
##  [5] "Systolic.Blood.Pressure"  "Diastolic.Blood.Pressure"
##  [7] "Sleep.Problems"           "Caregiver.."             
##  [9] "Anxiety"                  "Country"
```

``` r
dim(rdatos)
```

```
## [1] 1000   10
```


``` r
str(rdatos)
```

```
## 'data.frame':	1000 obs. of  10 variables:
##  $ ID                      : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Age                     : int  53 53 63 52 58 56 57 67 72 55 ...
##  $ Gender                  : int  2 2 2 2 2 1 2 2 2 1 ...
##  $ Edu                     : int  22 11 4 13 14 11 8 12 8 15 ...
##  $ Systolic.Blood.Pressure : int  NA 100 114 96 117 116 134 137 148 88 ...
##  $ Diastolic.Blood.Pressure: int  76 63 72 64 68 64 70 72 75 64 ...
##  $ Sleep.Problems          : int  2 1 2 1 2 1 2 1 1 1 ...
##  $ Caregiver..             : int  2 1 2 1 2 1 1 1 1 1 ...
##  $ Anxiety                 : int  2 2 2 2 2 1 2 2 1 2 ...
##  $ Country                 : chr  "Spain" "Spain" "Spain" "Spain" ...
```

Remember that a very convenient function for having a general idea of our data is: 


``` r
summary(rdatos)
```

```
##        ID              Age           Gender           Edu       
##  Min.   :   1.0   Min.   :40.0   Min.   :1.000   Min.   : 1.00  
##  1st Qu.: 250.8   1st Qu.:54.0   1st Qu.:1.000   1st Qu.: 8.00  
##  Median : 500.5   Median :58.0   Median :2.000   Median :10.00  
##  Mean   : 500.5   Mean   :57.9   Mean   :1.596   Mean   :10.01  
##  3rd Qu.: 750.2   3rd Qu.:61.0   3rd Qu.:2.000   3rd Qu.:12.00  
##  Max.   :1000.0   Max.   :74.0   Max.   :2.000   Max.   :22.00  
##                   NA's   :28     NA's   :22      NA's   :24     
##  Systolic.Blood.Pressure Diastolic.Blood.Pressure Sleep.Problems 
##  Min.   : 54.0           Min.   :55.00            Min.   :1.000  
##  1st Qu.:106.0           1st Qu.:66.00            1st Qu.:1.000  
##  Median :119.0           Median :70.00            Median :1.000  
##  Mean   :119.6           Mean   :69.82            Mean   :1.307  
##  3rd Qu.:132.0           3rd Qu.:73.00            3rd Qu.:2.000  
##  Max.   :186.0           Max.   :85.00            Max.   :2.000  
##  NA's   :22              NA's   :24               NA's   :20     
##   Caregiver..       Anxiety        Country         
##  Min.   :1.000   Min.   :1.000   Length:1000       
##  1st Qu.:1.000   1st Qu.:1.000   Class :character  
##  Median :1.000   Median :2.000   Mode  :character  
##  Mean   :1.399   Mean   :1.637                     
##  3rd Qu.:2.000   3rd Qu.:2.000                     
##  Max.   :2.000   Max.   :2.000                     
##  NA's   :30      NA's   :23
```

As you can observe, the columns 'ID,' 'Gender,' 'Sleep Problems,' 'Caregiver,' and 'Anxiety' are currently represented as numeric vectors in our dataset. However, it's important to note that these columns are essentially [dummy variables](https://bookdown.org/carillitony/bailey/chp6.html) (with the exception of the 'ID' column). Dummy variables typically encode categorical information using numeric values, where each unique category corresponds to a specific number. As part of this lesson, we will explore the process of recoding dummy variables to enhance the informativeness of our dataset. 

## Subsetting datasets
R has powerful indexing features for accessing the elements of an object. These features can be used to select and exclude variables, observations, or both. 

### Selecting (keeping) variables
It's a common practice to create a new dataset from a limited number of variables chosen from a larger dataset. The elements of a data frame are accessed using the notation data frame[row indices, column indices]. You can use this to select variables. 

The statement:


``` r
newdata <- rdatos[, c(1,3)]
```

selects the two columns from the 'rdatos' data frame and saves them to 'newdata' data frame. Leaving the row indices blank [,] (before the comma) selects all the rows by default.

The statement 

``` r
myvars <- c("ID", "Gender")
newdata <-rdatos[myvars]
```
accomplish the same variable selection. Here, variable names (in quotes) have been entered as column indices, thereby selecting the same columns.

### Excluding (dropping) variables
There are many reasons to exclude variables. For example, if a variable has several missing values, you may want to drop it prior to further analyses. You could exclude variable Gender with the statement:


``` r
myvars <- names(rdatos) %in% c("Gender","Age")
newdata <- rdatos[!myvars]
```

Remember that we use `%in%` to create a logical vector of the columns names that match `Gender`. Then, the exclamation mark (!) is used to negate or reverse the logical values in a logical vector. In other words, it flips `TRUE` values to `FALSE` and `FALSE` values to `TRUE`:


``` r
myvars
```

```
##  [1] FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

``` r
!myvars
```

```
##  [1]  TRUE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
```

Knowing that Gender is the 3rd variable, you could exclude it by using its index: 

``` r
newdata <- rdatos[,-3]
```

### Selecting or excluding observations
Selecting or excluding observations (rows) is typically a key aspect of successful data preparation and analysis. 

* Examples

Ask for rows 1 to 3 (first three observations). Notice that after subsetting, the 'newdata' data frame has only 3 rows and 10 columns, since we haven't specify anything after the comma in `[,]`.

``` r
newdata <- rdatos[1:3,] 
```


### Conditional selection of data
In data analysis, it's often crucial to select specific rows from our dataset based on certain conditions. This allows us to focus on the data that is relevant to our analysis. In R, we use logical operators to create these conditions. Here are some common logical operators:

* `<`: Less than
* `<=`: Less than or equal to
* `>`: Greater than
* `>=`: Greater than or equal to
* `==`: Equal to
* `!=`: Not equal to
* `!x`: Not x
* `x & y`: x AND y
* `x | y`: x OR y

For example, if we want to select individuals who are older than 55 years old:


``` r
selected_data <- rdatos[which(rdatos$Age > 55),] 
```

In this example, the logical condition `rdatos$Age > 55` produces a logical vector of `TRUE` and `FALSE` values for each row. We then use the `which()` function to identify the row indices where the condition is met (i.e., where it's `TRUE`), and we store the selected data in a new variable.

You can also use logical operators to select data based on multiple conditions. For example, to select individuals who are both older than 55 and have a 'Gender' value of 1:


``` r
newdata <- rdatos[which(rdatos$Age > 55 & rdatos$Gender == 1),]
```

By combining logical conditions with & (AND) or | (OR), you can create more complex conditions to filter and select the data that meets your analysis requirements.

#### The subset() function in R

In R, the subset() function is a versatile tool for selecting specific variables (columns) and observations (rows) from your data. It offers a simple way to filter and extract the data you need. The general format of the `subset()` function is as follows:


``` r
subset(object, subset = logical_expression, select = columns)
```

You can use this function to subset data frames, matrices, or vectors. Here's a breakdown of its components:

* `object`: The data object you want to subset.
* `subset`: The logical condition that specifies which rows to keep.
* `select`: The columns you want to select from the data.

Let's explore some practical examples to understand how the `subset()` function works.

Retrieve all rows where the 'Age' column contains values less than 70. We will keep only the 'ID,' 'Age,' and 'Gender' variables.

``` r
newdata1 <- subset(rdatos, Age < 70, select=c(ID, Gender, Age)) 
```

#### Logical OR operator `|`

Retrieve all rows where the 'Age' column contains values either lesser than or equal to 55, **OR** less than 70. 

``` r
newdata2 <- subset(rdatos, Age <= 55 | Age < 70, select=c(ID, Gender, Age)) 
```
Notice that we subset exactly the same individuals by using the two expression above: 


``` r
identical(newdata1,newdata2)
```

```
## [1] TRUE
```

That's because we are using the `|` operator, for which only one of the two logical values needs to be `TRUE` for the entire `OR` operation to evaluate to `TRUE`:


``` r
TRUE | FALSE
```

```
## [1] TRUE
```

``` r
FALSE | TRUE
```

```
## [1] TRUE
```

``` r
TRUE | TRUE
```

```
## [1] TRUE
```

``` r
FALSE | FALSE
```

```
## [1] FALSE
```
This means that we'll only remove individuals that do not met any of the both conditions: individuals older than 70.

#### Logical AND operator `&`

Now, we'll retrieve all rows where the 'Age' column contains values greater than or equal to 55, **AND** less than 70. 

``` r
newdata4 <- subset(rdatos, Age >= 55 & Age < 70, select=c(ID, Gender, Age)) 
```
With the expression above, we have filtered and selected individuals who satisfy both conditions: those aged 55 or older and those younger than 70.

Finally, we'll retrieve all rows where the 'Age' column contains values either lesser than or equal to 55, **AND** less than 70. 

``` r
newdata5 <- subset(rdatos, Age <= 55 & Age < 70, select=c(ID, Gender, Age)) 
```

With the expression above, we've selected individuals who satisfy both conditions. This last expression subsets the same data as the following one, as all individuals aged 55 or younger also meet the condition of being younger than 70:


``` r
newdata6 <- subset(rdatos, Age <= 55, select=c(ID, Gender, Age)) 

identical(newdata5,newdata6)
```

```
## [1] TRUE
```

### Managing missing values
Data sets are likely to be incomplete because of missed questions, faulty equipment, or improperly coded data. In R, missing values are represented by the symbol `NA` (not available). Impossible values (for example, dividing by 0) are represented by the symbol `NaN` (not a number).

`R` provides a number of functions for identifying observations that contain missing values. The function `is.na()` allows you to test for the presence of missing values.

* Example

Assume that you have a vector:


``` r
y <- c(1, 2, 3, NA)
is.na(y)
```

```
## [1] FALSE FALSE FALSE  TRUE
```
then the function `is.na(y)` returns `c(FALSE, FALSE, FALSE, TRUE)`.

Now, we can easily count how many 'NA' there are in our data frame with this expression: 

``` r
colSums(is.na(rdatos))
```

```
##                       ID                      Age                   Gender 
##                        0                       28                       22 
##                      Edu  Systolic.Blood.Pressure Diastolic.Blood.Pressure 
##                       24                       22                       24 
##           Sleep.Problems              Caregiver..                  Anxiety 
##                       20                       30                       23 
##                  Country 
##                        0
```

Once you've identified the missing values, you need to eliminate them in some way before analysing your data further. The reason is that arithmetic expressions and functions that contain missing values yield missing values. For example, consider the following code:

``` r
x <- c(1, 2, NA, 3)
y <- x[1] + x[2] + x[3] + x[4]
y
```

```
## [1] NA
```

``` r
z <- sum(x)
z
```

```
## [1] NA
```

Both `y` and `z` will be `NA` (missing) because the third element of `x` is missing. Luckily, most numeric functions have a `na.rm=TRUE` option that removes missing values prior to calculations and applies the function to the remaining values:


``` r
x <- c(1, 2, NA, 3)
y <- sum(x, na.rm=TRUE)
y
```

```
## [1] 6
```

You may also want to maintain only complete cases for further analyses... 


``` r
rdatos_noNA <- na.omit(rdatos)

rdatos_cc <- rdatos[complete.cases(rdatos),]
```

or only remove rows that contain NA in specific columns: 


``` r
rdatos_noNA_Age <- rdatos[complete.cases(rdatos[,"Age"]),]
colSums(is.na(rdatos))
```

```
##                       ID                      Age                   Gender 
##                        0                       28                       22 
##                      Edu  Systolic.Blood.Pressure Diastolic.Blood.Pressure 
##                       24                       22                       24 
##           Sleep.Problems              Caregiver..                  Anxiety 
##                       20                       30                       23 
##                  Country 
##                        0
```

``` r
colSums(is.na(rdatos_noNA_Age))
```

```
##                       ID                      Age                   Gender 
##                        0                        0                       22 
##                      Edu  Systolic.Blood.Pressure Diastolic.Blood.Pressure 
##                       24                       22                       24 
##           Sleep.Problems              Caregiver..                  Anxiety 
##                       20                       30                       23 
##                  Country 
##                        0
```

## Recoding variables

Recoding variables involves transforming existing values of a variable based on specific conditions. This process allows us to create new categories or values, replace incorrect entries, or reformat data to better suit our analysis needs. Here are some common scenarios where recoding is useful:

*Converting a continuous variable into categories for easier interpretation.
*Creating binary variables like "pass/fail" based on predefined cut-off scores.
*Correcting or replacing misclassified or miscoded values.

We can recode variables in R using various methods. Let's explore a few examples:

### Subsetting and assigning 
One way to recode variables is by subsetting rows based on specific conditions and assigning new values. For instance, let's create a new categorical column 'Agecat' based on the 'Age' column:

``` r
rdatos$Agecat[rdatos$Age > 70] <- "Elder"
rdatos$Agecat[rdatos$Age  >= 50 & rdatos$Age <= 70] <- "Middle Aged"
rdatos$Agecat[rdatos$Age < 50] <- "Young"
table(rdatos$Agecat)
```

```
## 
##       Elder Middle Aged       Young 
##          10         912          50
```


### Using the `within()` function
The `within()` function allows you to make multiple changes within a data frame. Here, we create 'Agecat' and 'GenderRN' columns based on conditions:

``` r
rdatos <- within(rdatos,{ ## note: curly brackets indicate several changes will be made
Agecat <- NA
Agecat[Age > 70] <- "Elder"
Agecat[Age >= 50 & Age <= 70] <- "Middle Aged"
Agecat[Age < 50] <- "Young" })
table(rdatos$Agecat)
```

```
## 
##       Elder Middle Aged       Young 
##          10         912          50
```

``` r
rdatos <- within(rdatos,{
  GenderRN <- NA
  GenderRN[Gender==2] <- "Female"
  GenderRN[Gender==1] <- "Male"})

head(rdatos[,c("ID","Age","Agecat","Gender","GenderRN")])
```

```
##   ID Age      Agecat Gender GenderRN
## 1  1  53 Middle Aged      2   Female
## 2  2  53 Middle Aged      2   Female
## 3  3  63 Middle Aged      2   Female
## 4  4  52 Middle Aged      2   Female
## 5  5  58 Middle Aged      2   Female
## 6  6  56 Middle Aged      1     Male
```

### Using the `ifelse()` function.
The `ifelse()` function is handy for recoding based on conditions.It allows you to create new values for a variable depending on whether a specified condition is true or false. 


``` r
rdatos <- within(rdatos,{
  GenderRN <- NA
  GenderRN <- ifelse(Gender == 2, "Female", "Male")
})
```

If the condition `Gender == 2` is met, it assigns "Female" to 'GenderRN' for that observation. If the condition is not met, it assigns "Male". 


This method is particularly useful when you want to apply different transformations to your data based on various conditions:

``` r
rdatos <- within(rdatos,{
  Agecat <- NA
  Agecat <- ifelse(Age > 70, "Elder", ifelse(Age >= 50 & Age <= 70, "Middle Aged", "Young"))
})
```

The ifelse() function evaluates each condition sequentially. If the first condition is met (Age > 70), it assigns "Elder" to 'Agecat' for that observation. If the first condition is not met, it moves to the second condition (Age >= 50 & Age <= 70) and assigns "Middle Aged" if it's true. If neither of the first two conditions is met, it defaults to "Young."

## Merging datasets
When your data is scattered across multiple locations or sources, it's essential to combine them into a single dataset for comprehensive analysis. In R, you can achieve this through the process of merging datasets. Merging involves both adding columns horizontally and adding rows vertically, depending on your data integration needs.

### Adding columns (Horizontal merge)
To merge two datasets horizontally, you can use the `merge()` function. Typically, datasets are joined using one or more common key variables that exist in both datasets. Here's an example of how to do this:

**Example:**
Suppose we have two data frames, dataframeA and dataframeB, each containing information about individuals, including an 'ID' variable:

``` r
## Create dataframeA
dataframeA <- data.frame(ID= seq(1,100,1), ID2= seq(1,100,1), X1=runif(100), X2=rnorm(100), Country=rep(c("Spain", "EEUU", "Iceland"), times=c(50,25,25)))

## Create dataframeB
dataframeB <- data.frame(ID= seq(1,100,1), Y1=rnorm(100), Y2=rnorm(100),  Country=rep(c("Spain", "EEUU", "Iceland"), times=c(30,45,25)))
```

To merge dataframeA and dataframeB based on the 'ID' variable:

``` r
total <- merge(dataframeA, dataframeB, by="ID")
```

This merges the two data frames using 'ID' as the common key, combining their columns horizontally. The result is stored in the total data frame.

You can also merge data frames using different key variables, as shown below:

``` r
total <- merge(dataframeA, dataframeB, by.x="ID2", by.y="ID")
```

Additionally, you can merge data frames using multiple key variables by specifying a vector:

``` r
total <- merge(dataframeA, dataframeB, by=c("ID","Country"),all=T)
```

Another method to merge datasets horizontally, particularly when you don't need to specify a common key, is to use the `cbind()` function. This function concatenates columns from multiple data frames, combining them side by side. It's especially useful when you want to add new variables to an existing data frame or combine data frames with different observations **but matching rows**.


``` r
total <- cbind(dataframeA, dataframeB)
```

The `cbind()` function simply combines the columns of dataframeB next to the columns of dataframeA to create the total data frame. It's important to note that the order of columns in the resulting data frame will follow the order in which you pass the data frames to `cbind()`.

This method is convenient when you have datasets with different variables but want to append them horizontally, such as adding new variables to an existing dataset. However, if you have a common key to join datasets based on matching rows, the `merge()` function, as explained earlier, is more appropriate.

### Adding rows (Vertical merge)

Vertical concatenation is typically used to add observations to a data frame. To join two data frames (datasets) vertically, use the rbind() function. 

``` r
total <- rbind(dataframeA, dataframeB)
```

when using `rbind()` to merge data frames vertically, the columns in both data frames must have the same names to work correctly. If one data frame has columns that are missing in the other data frame, `rbind()` may produce incomplete results. For example:


``` r
# Create dataframeA
dataframeA <- data.frame(ID = 1:3, Name = c("Alice", "Bob", "Charlie"))

# Create dataframeB with an additional column
dataframeB <- data.frame(ID = 4:6, Age = c(25, 30, 22))

# Attempt to merge using rbind()
total <- rbind(dataframeA, dataframeB)
```

If the data types of columns with the same name differ between the data frames, `rbind()` may coerce them into a common data type. This can lead to data loss or unexpected behaviour. For example:


``` r
# Create dataframeA with character ID
dataframeA <- data.frame(ID = c("1", "2", "3"), Name = c("Alice", "Bob", "Charlie"))

# Create dataframeB with numeric ID
dataframeB <- data.frame(ID = c(4, 5, 6), Name = c("Alice", "Bob", "Charlie"))

# Attempt to merge using rbind()
total <- rbind(dataframeA, dataframeB)
```


## Sorting data

Sometimes, arranging your dataset in a specific order can reveal valuable insights. In R, you can easily sort a data frame using the order() function. Sorting allows you to explore your data in an organized and informative manner.

### Sorting by a single variable
You can start by sorting your data frame based on one variable. Here's an example:


``` r
newdata <- rdatos[order(rdatos$Age),]
```
creates a new dataset containing rows sorted from youngest to oldest.

### Sorting alphabetically 
You can also arrange your data alphabetically based on a categorical variable like 'Country':


``` r
table(rdatos$Country)
```

```
## 
## France  Italy  Spain 
##    250    250    500
```

``` r
newdata <- rdatos[order(rdatos$Country),]
```

### Sorting by multiple variables

This code sorts the rows first alphabetically by 'Country' and then within each country by 'Age,' providing a comprehensive view of your dataset.

``` r
newdata <- rdatos[order(rdatos$Country, rdatos$Age),]

head(newdata[, c(1, 2, 10)], 10)
```

```
##      ID Age Country
## 725 725  43  France
## 559 559  47  France
## 583 583  47  France
## 629 629  47  France
## 641 641  49  France
## 675 675  49  France
## 706 706  49  France
## 731 731  49  France
## 630 630  50  France
## 698 698  50  France
```
This code sorts the data by 'Age' in ascending order and then, within each age group, by 'Gender.'

``` r
newdata <-rdatos[order(rdatos$Age, rdatos$Gender),]
head(newdata[, c(1, 2, 3)], 10)
```

```
##      ID Age Gender
## 38   38  40      1
## 725 725  43      1
## 485 485  44      1
## 761 761  44      1
## 324 324  44      2
## 246 246  45      2
## 23   23  46      1
## 895 895  46      1
## 220 220  46      2
## 376 376  46      2
```

## Regular expressions and pattern matching and replacement 

Regular expressions, often referred to as `regex` or `regexp`, are sequence of characters that defines a search pattern and are powerful tools for pattern matching and text manipulation. In R, you can use regular expressions to search, extract, and manipulate text data. Let's dive into the basics of regular expressions in R. Regular expressions are defined using a combination of characters and **special symbols**. 

### Basic functions to detect patterns in R

R provides several functions for working with regular expressions, including:

- `grep(pattern, x, value = FALSE, ignore.case = FALSE)`: Search for a pattern in a character vector or text. It returns a vector of the **indeces** of the element that yielded a match. To return the value of the element that yielded a match, set `value =T`. For non-case-sensitive matching set `ignore.case = TRUE`.
- `grepl(pattern, x)`: Check if a pattern exists in a character vector or text. It returns a vector of **logical values** (match or not for each element of the vector).

Here are some examples for the use of `grep()` and `grepl()`:


``` r
intro <- c("2025/26","Lesson 2", "Importance of the use of regular expressions in R","4","Omics","","exprress")
```

Notice `grep` returns the indices of the elements contained in the vector 'intro' that match the pattern. 


``` r
grep("2",intro)
```

```
## [1] 1 2
```

``` r
grep("2",intro, value=T)
```

```
## [1] "2025/26"  "Lesson 2"
```

``` r
grepl("2",intro)
```

```
## [1]  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
```

``` r
grep("4",intro)
```

```
## [1] 4
```

``` r
grep("4",intro,value=T)
```

```
## [1] "4"
```

``` r
grepl("4",intro)
```

```
## [1] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
```

- `sub(pattern, replacement, x)`: Replace the first occurrence of a pattern (inside each one of the elements of the vector) with another string.


``` r
new <- sub("2","2.",intro)
new
```

```
## [1] "2.025/26"                                         
## [2] "Lesson 2."                                        
## [3] "Importance of the use of regular expressions in R"
## [4] "4"                                                
## [5] "Omics"                                            
## [6] ""                                                 
## [7] "exprress"
```

- `gsub(pattern, replacement, x)`: Replace all occurrences of a pattern with another string.


``` r
gsub("2","2.",intro)
```

```
## [1] "2.02.5/2.6"                                       
## [2] "Lesson 2."                                        
## [3] "Importance of the use of regular expressions in R"
## [4] "4"                                                
## [5] "Omics"                                            
## [6] ""                                                 
## [7] "exprress"
```


### Special symbols 

Here are some commonly used special symbols and their meanings:

- `.`: Matches any single character element. Thus, we have a problem if we want to look for "." inside my character strings...


``` r
grep(".", new, value =T)
```

```
## [1] "2.025/26"                                         
## [2] "Lesson 2."                                        
## [3] "Importance of the use of regular expressions in R"
## [4] "4"                                                
## [5] "Omics"                                            
## [6] "exprress"
```

``` r
grepl(".", new)
```

```
## [1]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
```

*Notice that `.` does not match the sixth element of the vector.*

Since `.` is a special symbol, for searching or replacing "." within a character string, you should use "\\.":


``` r
grep("\\.",new,value=T)
```

```
## [1] "2.025/26"  "Lesson 2."
```

You can keep in blank the `replacement` argument in order to remove a pattern with `gsub(pattern, replacement, x)`: 


``` r
gsub(pattern = "\\.",replacement = "",x = new)
```

```
## [1] "2025/26"                                          
## [2] "Lesson 2"                                         
## [3] "Importance of the use of regular expressions in R"
## [4] "4"                                                
## [5] "Omics"                                            
## [6] ""                                                 
## [7] "exprress"
```

- `*`: Matches zero or more occurrences of the preceding character or pattern. 

For instance, the regular expression `pp*` can be divided in two: 'p' that will match the character `p` and `p*` that will match zero or more occurrences of 'p'.


``` r
repe <- c("apple", "appl", "appppl", "banana", "bppple", "aple")
grep("pp*",repe, value = TRUE)
```

```
## [1] "apple"  "appl"   "appppl" "bppple" "aple"
```
In contrast, the regular expression `p*` will match zero or more occurrences of 'p'. That is, *everything*.


``` r
grep("p*", repe, value = TRUE)
```

```
## [1] "apple"  "appl"   "appppl" "banana" "bppple" "aple"
```

In other words: 

* `p*`: means zero or more `p` in a row. 

* `pp*`: means one `p` followed by zero or more `p`.

- `+`: Matches one or more occurrences of the preceding character or pattern.


``` r
grep("pp+",repe, value = TRUE)
```

```
## [1] "apple"  "appl"   "appppl" "bppple"
```
That is, `pp+` matches any substring with at least two consecutive `p`.

- `?`: Matches zero or one occurrence of the preceding character or pattern.


``` r
grep("r?ess",new, value = TRUE)
```

```
## [1] "Lesson 2."                                        
## [2] "Importance of the use of regular expressions in R"
## [3] "exprress"
```

``` r
grep("r+ess",new, value = TRUE)
```

```
## [1] "Importance of the use of regular expressions in R"
## [2] "exprress"
```

- `^`matches the start of a line and `$` matches the end of a line.


``` r
grep("i",intro,value = T,ignore.case = T)
```

```
## [1] "Importance of the use of regular expressions in R"
## [2] "Omics"
```

``` r
grep("^i",intro,value = T,ignore.case = T)
```

```
## [1] "Importance of the use of regular expressions in R"
```

``` r
grep("\\.",new,value=T)
```

```
## [1] "2.025/26"  "Lesson 2."
```

``` r
grep("\\.$",new,value= T)
```

```
## [1] "Lesson 2."
```

- `[]`: Matches any one character within the brackets.


``` r
grep("[0-9]",new,value =T)
```

```
## [1] "2.025/26"  "Lesson 2." "4"
```

``` r
grep("[a-c]",new,value =T)
```

```
## [1] "Importance of the use of regular expressions in R"
## [2] "Omics"
```

Example: searching full stops that are preceded by digits or slash preceded by digits. 


``` r
grep("[0-9]\\.",new,value=T)
```

```
## [1] "2.025/26"  "Lesson 2."
```

``` r
grep("([0-9]*/)",new,value =T)
```

```
## [1] "2.025/26"
```

- `[^]`: Matches any one character NOT within the brackets.


``` r
grep("[^0-9]",new,value =T)
```

```
## [1] "2.025/26"                                         
## [2] "Lesson 2."                                        
## [3] "Importance of the use of regular expressions in R"
## [4] "Omics"                                            
## [5] "exprress"
```


- Special symbols can be combined to create a regular patterns for efficient pattern matching in R. 



``` r
examples <- c("apple", "appl", "appppl", "banana", "bppple",
              "ress", "rress", "rrress", "ess",
              "data.csv", "report.doc", "my.file.txt")

grep("^r+e.s+p*", examples, value = TRUE)
```

```
## [1] "ress"   "rress"  "rrress"
```

Let's understand each element of the pattern: 
- `^`: the word should start with the following pattern
- `r+`: one or more `r`
- `e`: letter `e`
- `.`: any character
- `s+`: one or more `s`
- `p*`: zero or more `p`

### Basic functions to locate patterns in R 

There are other functions that help us to locate a specific pattern inside a character string by specifying the *start* and *end* positions of your pattern in a given string.


``` r
text <-  "Alejandra, Berta, Alex, Carles, Joan, Alexander"
regexpr(pattern = "Ale", text = text) ## first occurrence of the pattern
```

```
## [1] 1
## attr(,"match.length")
## [1] 3
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

``` r
gregexpr(pattern = "Ale", text = text) ## detect several occurrences 
```

```
## [[1]]
## [1]  1 19 39
## attr(,"match.length")
## [1] 3 3 3
## attr(,"index.type")
## [1] "chars"
## attr(,"useBytes")
## [1] TRUE
```

The package 'stringr' provide efficient functions for the more common string manipulations in R. 

``` r
library(stringr)

str_locate(string = text, pattern = "Ale")
```

```
##      start end
## [1,]     1   3
```

``` r
str_locate_all(string = text, pattern = "Ale")
```

```
## [[1]]
##      start end
## [1,]     1   3
## [2,]    19  21
## [3,]    39  41
```
### Split a string using a pattern 

The `str_split` function from the 'stringr' package is very convenient for renaming and recoding variables. It returns a list of the split strings. If you want to access to each piece of string use the `unlist()` to create a character vector. 


``` r
text
```

```
## [1] "Alejandra, Berta, Alex, Carles, Joan, Alexander"
```

``` r
split_list <- str_split(string = text , pattern = ", ")
split_list[[1]]
```

```
## [1] "Alejandra" "Berta"     "Alex"      "Carles"    "Joan"      "Alexander"
```

``` r
unlist(split_list)[[2]]
```

```
## [1] "Berta"
```

## Exercises Data Management (Optional)

1. Read file `example.txt` and store it in a data frame called "example" 
2. Show rows 5,11,18 and 20 in data "example" 
3. Show variable "sex" for rows from 15 to 50 in data "example"
4. Change the name of the "cc" column to "Case/Control" in data "example"  
5. Export "example" to a "csv" semicolon delimited file without the names of the rows and without quotations 
6. Retrieve the forth element in vector `x=(3, -1, 0, 2, -5, 7, 1)`
7. Retrieve the first, second and fifth elements in vector `x=(3, -1, 0, 2, -5, 7, 1)`
8. Retrieve all the elements in vector `x=(3, -1, 0, 2, -5, 7, 1)` except the second one
9. Change the value of the first and second elements in `x=(3, -1, 0, 2, -5, 7, 1)` by 0  
10. Assign the value 0 to the elements in `x=(3, -1, 0, 2, -5, 7, 1)` that are larger than 2   
11. Create a matrix `M` with 4 rows and 3 columns and fill it by rows with even numbers from 2 to 24
12. Obtain the number of rows and columns of matrix `M`
13. Retrieve the element in the first row and third column of matrix `M`
14. Retrieve all the elements in the third column of matrix `M`
15. Retrieve the third and forth elements in the second column of matrix `M`
16. Retrieve a matrix containing all files in `M` except the first one 
17. Add a new column at the beginning of matrix `M` with the integers from 1 to  4
18. Add a row at the end of matrix `M` with values 2, 4, 8
19. Generate a data frame called `chol` (for cholesterol) containing the following variables (columns): 
`id=(1, 2, 3, 4, 5)`, `gender=(1, 1, 2, 1, 2)`, `LDL=(237, 256, 198, 287, 212)` 
20. In the previous data frame `chol` use the function `rownames()` to assign to each row the name of the patient: John, Peter, Hellen, Mat and Mary
21. Show the first 3 rows in data frame `chol`
22. Retrieve the LDL cholesterol levels of Peter using his position in the data frame
23. Retrieve the LDL cholesterol levels of Peter using his name in the code
24. Save the LDL cholesterol levels of the 5 individuals in a new vector called `ldl_chol` 
25. Create a new data frame named `chol_high` including only those individuals with LDL levels above 240
26. Let's consider the vector `x=(0.6, -1.3, 0.98, -0.4, 0.16)` and perform a `t.test` on `x` for the null hypothesis that the mean is equal to 0 and save the output in an object called `ttestx`
27. Show the attributes of object `ttestx` 
28. From the output of `ttestx` retrieve the confidence interval of the mean
29. Check the data type of gender in data frame `chol` 
30. Transform variable `gender` from data frame `chol` into a factor variable called `gender1` with 1=male and 2=female and with males as the reference group:
31. Transform variable `gender` from data frame `chol` into a factor variable called `gender2` with `1=male` and `2=female` and with females as the reference group: 
32. Write the data frame `chol` into a text file called `cholesterol.txt`
33. Write the data frame `chol` into a csv file called `cholesterol.csv`
34. Write the code to install and load the R package `readxl`` from CRAN
35. Get a numerical summary of `x=(1.5, 2.3, 4, 5.6, 2.1)`
36. Obtain the 40% percentile of `x=(1.5, 2.3, 4, 5.6, 2.1)`
37. Obtain the percentages of males and females in `gender=(1, 1, 2, 1, 2)`
38. Obtain the Pearson and Spearman correlation coefficient between `x=1:10` and `y=x^2` 
39. Test for the equality of variances in LDL cholesterol levels between males and females, assuming that LDL levels are normally distributed
40. Test for differences in LDL cholesterol mean levels between males and females, assuming that LDL levels are normally distributed
41. Test for differences in LDL cholesterol mean levels between males and females, without the assumption of normallity


---

![](logouvic.png)
