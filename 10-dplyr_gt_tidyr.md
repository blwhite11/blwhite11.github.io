# SECTION 9. Data managment using the dplyr package

The `dplyr` and `tidyr` packages are part of `tidyverse`, a “grammar of data manipulation”. The tidyverse comprises a collection of R packages specifically designed for data science including, among others, `ggplot2` (Check tidyverse website and this free online book for useful tutorials: “R for Data Science”).

`dplyr` is primarily focused on **data manipulation** tasks. It provides functions for filtering, selecting, mutating, arranging, and summarizing data. The package is designed to make data wrangling operations fast and intuitive.

`tidyr` is focused on **reshaping and cleaning** data. It provides functions to organize and transform data into a "tidy" format, where each variable is in its own column, each observation is in its own row, and each value is in its own cell.


To install the whole `tidyverse`: `install.packages("tidyverse")`. To install just `dplyr`: `install.packages("dplyr")`. For bug fixing or to use a tool in development: `install.packages("devtools")` and `install_github("tidyverse/dplyr")`.

The `dplyr` package contains very convenient functions for data manipulation which “translate” some Base R functions that we explored in Lesson 2 such as subset(), which(), order() or within(). These functions process data faster than Base R functions and transform your scripts into more “readable” coding. These are some of the most important functions included in `dplyr` package:

| To work with... | Functions in `dplyr` package | To...                           | Base R functions                          |
|---------------|---------------------------|---------------|---------------|
| Rows            | `filter()`                   | pick rows based on their values | `which(); subset()`                       |
| Rows            | `arrange()`                  | sort rows                       | `order()`                                 |
| Rows            | `slice()`                    | pick rows based on location     | `[RowPosition,]`                          |
| Rows            | `recode()`                   | recode dummy/encoded variables  | comparison operators                      |
| Columns         | `select()`                   | include/exclude columns         | `[, "ColumnName"]; subset(..., select())` |
| Columns         | `rename()`                   | change name of columns          | `colnames(); names()`                     |
| Columns         | `mutate()`                   | change/create columns           | `within()`                                |
| Columns         | `relocate()`                 | change columns' order           | `[ , NewColumnPosition ]`                 |
| Columns         | `group_by() + summarise()`   | collapse into a single row      | `mean(); median(); ...`                   |

One of the most useful functions to use together with the above functions is `group_by()` (it’s equivalent to `GROUP BY` in SQL). With this function we will applied the functions only to previously created groups based on one or more columns in the dataset.

Now, it’s time to play a little bit with these functions! First, we will load a dataframe that you will find in the following [link]('https://aules.uvic.cat/pluginfile.php/1906714/mod_folder/content/0/dataset_class.txt?forcedownload=1').




``` r
data<- read.table(file = "./dataset_class.txt",header = T, sep = "\t")
```

We are working with a dataset that contains information of 300 individuals for which we have available information for 8 different variables.

## The `filter()` function and the use of comparison operators

Filter rows based on column values. Similar to what we did with which() or subset(), we can implement comparison operators to this function (e.g \<, \>, !=). For instance, how many individuals are older than 30 and display glucose levels above 65? And how many of them are also women?



``` r
filter1<- dplyr::filter(data,Age >30,Glucose >65)
filter2<- dplyr::filter(data,Age >30,Glucose >65, Sex=="Women")
```

## Sorting with `arrange()`

Sort rows by ascending or descending order. We can reorder the data.frame based on one or more columns. For instance, for sorting our data based on increasing “Age” of individuals we will use:


``` r
df_sorted<- arrange(data, Age, Cholesterol)
```

## Accessing rows with `slice()`

It allows to access rows by their indexes (i.e. integer location).


``` r
slice(data, 5:10)
```

```
##     Sex Age  BMI SBP HADS_Anxiety Cholesterol Glucose Family_History_CVD
## 1 Women  34 19.8 166            7       178.4    95.2                 No
## 2   Men  46 34.8 120            0       187.7   129.7                 No
## 3   Men  43 28.1 133           13       262.9   113.2                 No
## 4   Men  44 27.8 133           10       232.7    83.7                 No
## 5 Women  38 18.2 144           21       165.6    91.3                 No
## 6 Women  24 29.4 121           10       138.4   114.6                 No
```

## Picking columns out with `select()`

We can use `select()`to subset columns using their specific names or properties (type numeric, character, factor). We can combine the use of `select()`with Base R operators (i.e. `:`, `!`,`&`,`c()`) and selection helpers. Some of the most interesting helpers are those which allow the selection of variables according to a given pattern: `starts_with()`, `ends_with()`, `contains()`, `matches()`, `num_range()`. Some examples:


``` r
select(data, c(Sex, starts_with("Glu")))
select(data, c(Age, starts_with("Chol")))
```

## Change names with `rename()`

This function facilities the readability of datasets (i.e. change encoded columns) or make it easy to correct errors in the name of the columns.


``` r
rename(data,Family_History=Family_History_CVD)
```

## Transforming pre-existing columns or creating new ones with `mutate()`


``` r
mutate(data, RandomVariable = SBP + Cholesterol)
```

## To sum up…`group_by()` + `summarise()`


``` r
group_by(data, Sex) %>% 
summarise(Mean_SBP = mean(SBP,na.rm=T))
```

```
## # A tibble: 2 × 2
##   Sex   Mean_SBP
##   <chr>    <dbl>
## 1 Men       120.
## 2 Women     120.
```

We have already incorporated the use of “the pipe, %\>%”. The main advantage of using `dplyr` for data management is that this package offers a very convenient tool to concatenate several operations. So, the whole point of `dplyr` is to make several transformation in your data at once.


``` r
data %>%
  filter(Sex=="Women") %>% 
  group_by(Family_History_CVD) %>%
  summarise(Mean_Age = mean(Age,na.rm=T),
            Mean_cholesterol = mean(Cholesterol, na.rm=T),
            count_BMIobese = sum(BMI>28))
```

```
## # A tibble: 2 × 4
##   Family_History_CVD Mean_Age Mean_cholesterol count_BMIobese
##   <chr>                 <dbl>            <dbl>          <int>
## 1 No                     48.6             201.             21
## 2 Yes                    48.7             198.              8
```

## Data reshaping and cleaning using the tidyr package

Similarly, with `tidyr` we can also work with functions that process data faster than Base R functions. These are some of the most important functions included in `tidyr` package:

| To work with... | Functions in `tidyr` package | To...                                        | Base R functions               |
|----------------|----------------|----------------------|------------------|
| Columns         | `pivot_longer()`             | convert wide data to long format             | `reshape(); stack()`           |
| Columns         | `pivot_wider()`              | convert long data to wide format             | `reshape(); unstack()`         |
| Columns         | `separate()`                 | split one column into multiple columns       | `strsplit()`                   |
| Columns         | `unite()`                    | combine multiple columns into one            | `paste(); paste0()`            |
| Missing Values  | `fill()`                     | fill missing values in a column              | `na.locf()` from `zoo` package |
| Missing Values  | `replace_na()`               | replace missing values with specified values | `is.na(); ifelse()`            |
| Columns         | `drop_na()`                  | remove rows with missing values              | `na.omit(); complete.cases()`  |
| Rows            | `expand()`                   | create all combinations of values            | `expand.grid()`                |
| Rows            | `complete()`                 | complete missing combinations of values      | `expand.grid(); merge()`       |
| Rows/Columns    | `nest()`                     | create nested data frames                    | `list()`                       |
| Rows/Columns    | `unnest()`                   | expand a list-column into multiple rows      | `do.call(rbind, ...)`          |

### Transforming wide to long format with `pivot_longer()`:

We want to put the information for the variable BMI, Cholesterol and Glucose in a long format.


``` r
library(tidyr)
head(data)
```

```
##     Sex Age  BMI SBP HADS_Anxiety Cholesterol Glucose Family_History_CVD
## 1 Women  43 23.1 129            5       223.3    87.2                 No
## 2 Women  59 28.4 154           12       183.4    83.1                 No
## 3 Women  66 28.7 105           11       266.8    87.8                 No
## 4   Men  75 23.7 122            2       221.4    64.2                 No
## 5 Women  34 19.8 166            7       178.4    95.2                 No
## 6   Men  46 34.8 120            0       187.7   129.7                 No
```

``` r
long_format <- data %>%
    pivot_longer(cols = c(BMI, Cholesterol, Glucose),
                 names_to = "Measurement_Type",
                 values_to = "Value")
head(long_format)
```

```
## # A tibble: 6 × 7
##   Sex     Age   SBP HADS_Anxiety Family_History_CVD Measurement_Type Value
##   <chr> <int> <int>        <int> <chr>              <chr>            <dbl>
## 1 Women    43   129            5 No                 BMI               23.1
## 2 Women    43   129            5 No                 Cholesterol      223. 
## 3 Women    43   129            5 No                 Glucose           87.2
## 4 Women    59   154           12 No                 BMI               28.4
## 5 Women    59   154           12 No                 Cholesterol      183. 
## 6 Women    59   154           12 No                 Glucose           83.1
```

### Transforming long to wide format with `pivot_wider()`:

In this example, we use the long_format object generated in the previous example to re-convert it again to wide format.


``` r
# Suppose we have a column "Measurement_Type" with values for "Cholesterol" and "Glucose"
# Transform it back to wide format

head(long_format)
```

```
## # A tibble: 6 × 7
##   Sex     Age   SBP HADS_Anxiety Family_History_CVD Measurement_Type Value
##   <chr> <int> <int>        <int> <chr>              <chr>            <dbl>
## 1 Women    43   129            5 No                 BMI               23.1
## 2 Women    43   129            5 No                 Cholesterol      223. 
## 3 Women    43   129            5 No                 Glucose           87.2
## 4 Women    59   154           12 No                 BMI               28.4
## 5 Women    59   154           12 No                 Cholesterol      183. 
## 6 Women    59   154           12 No                 Glucose           83.1
```

``` r
wide_format <- long_format %>%
    pivot_wider(names_from = Measurement_Type, values_from = Value)
head(wide_format)
```

```
## # A tibble: 6 × 8
##   Sex     Age   SBP HADS_Anxiety Family_History_CVD   BMI Cholesterol Glucose
##   <chr> <int> <int>        <int> <chr>              <dbl>       <dbl>   <dbl>
## 1 Women    43   129            5 No                  23.1        223.    87.2
## 2 Women    59   154           12 No                  28.4        183.    83.1
## 3 Women    66   105           11 No                  28.7        267.    87.8
## 4 Men      75   122            2 No                  23.7        221.    64.2
## 5 Women    34   166            7 No                  19.8        178.    95.2
## 6 Men      46   120            0 No                  34.8        188.   130.
```

### Handling missing values with `replace_na()`

We create a new random variable that will take NA values for those cases where cholesterol is above 230 and then we will replace it with the median value.


``` r
# Replace NA values in Cholesterol with the median Cholesterol
table(is.na(data$Cholesterol))
```

```
## 
## FALSE 
##   300
```

``` r
summary(data$Cholesterol)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   122.7   180.2   200.0   200.6   222.6   280.0
```

``` r
data<- data %>% mutate(Variable_Nas=ifelse(Cholesterol > 230, NA, Cholesterol))
summary(data$Variable_Nas)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   122.7   173.9   193.4   190.8   208.8   229.0      55
```

``` r
data <- data %>%
    mutate(Variable_Nas = replace_na(Variable_Nas, median(Cholesterol, na.rm = TRUE)))
summary(data$Variable_Nas)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   122.7   180.2   200.0   192.5   204.9   229.0
```

## Gt and gtsummary packages

### Basic Summary Table with `tbl_summary()`

The `tbl_summary()` function calculates descriptive statistics for continuous, categorical, and dichotomous variables in R, and presents the results in a beautiful, customizable summary table ready for publication


``` r
library(gtsummary)
library(gt)
data %>%
    tbl_summary(
        by = Sex, # Summarize separately for Men and Women
        missing = "no" # Exclude missing values in summary
    ) %>% as_gt() 
```

```{=html}
<div id="yypurbehai" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#yypurbehai table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#yypurbehai thead, #yypurbehai tbody, #yypurbehai tfoot, #yypurbehai tr, #yypurbehai td, #yypurbehai th {
  border-style: none;
}

#yypurbehai p {
  margin: 0;
  padding: 0;
}

#yypurbehai .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#yypurbehai .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#yypurbehai .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#yypurbehai .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#yypurbehai .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yypurbehai .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yypurbehai .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yypurbehai .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#yypurbehai .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#yypurbehai .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#yypurbehai .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#yypurbehai .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#yypurbehai .gt_spanner_row {
  border-bottom-style: hidden;
}

#yypurbehai .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#yypurbehai .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#yypurbehai .gt_from_md > :first-child {
  margin-top: 0;
}

#yypurbehai .gt_from_md > :last-child {
  margin-bottom: 0;
}

#yypurbehai .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#yypurbehai .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#yypurbehai .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#yypurbehai .gt_row_group_first td {
  border-top-width: 2px;
}

#yypurbehai .gt_row_group_first th {
  border-top-width: 2px;
}

#yypurbehai .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yypurbehai .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#yypurbehai .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#yypurbehai .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yypurbehai .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yypurbehai .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#yypurbehai .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#yypurbehai .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#yypurbehai .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yypurbehai .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yypurbehai .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yypurbehai .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yypurbehai .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yypurbehai .gt_left {
  text-align: left;
}

#yypurbehai .gt_center {
  text-align: center;
}

#yypurbehai .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#yypurbehai .gt_font_normal {
  font-weight: normal;
}

#yypurbehai .gt_font_bold {
  font-weight: bold;
}

#yypurbehai .gt_font_italic {
  font-style: italic;
}

#yypurbehai .gt_super {
  font-size: 65%;
}

#yypurbehai .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#yypurbehai .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#yypurbehai .gt_indent_1 {
  text-indent: 5px;
}

#yypurbehai .gt_indent_2 {
  text-indent: 10px;
}

#yypurbehai .gt_indent_3 {
  text-indent: 15px;
}

#yypurbehai .gt_indent_4 {
  text-indent: 20px;
}

#yypurbehai .gt_indent_5 {
  text-indent: 25px;
}

#yypurbehai .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#yypurbehai div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="label"><span class='gt_from_md'><strong>Characteristic</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_1"><span class='gt_from_md'><strong>Men</strong><br />
N = 146</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_2"><span class='gt_from_md'><strong>Women</strong><br />
N = 154</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Age</td>
<td headers="stat_1" class="gt_row gt_center">50 (35, 65)</td>
<td headers="stat_2" class="gt_row gt_center">49 (34, 64)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">BMI</td>
<td headers="stat_1" class="gt_row gt_center">25.4 (22.7, 27.4)</td>
<td headers="stat_2" class="gt_row gt_center">24.1 (21.5, 27.3)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">SBP</td>
<td headers="stat_1" class="gt_row gt_center">120 (109, 131)</td>
<td headers="stat_2" class="gt_row gt_center">120 (112, 130)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">HADS_Anxiety</td>
<td headers="stat_1" class="gt_row gt_center">10 (5, 16)</td>
<td headers="stat_2" class="gt_row gt_center">11 (5, 16)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Cholesterol</td>
<td headers="stat_1" class="gt_row gt_center">199 (180, 225)</td>
<td headers="stat_2" class="gt_row gt_center">200 (180, 221)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Glucose</td>
<td headers="stat_1" class="gt_row gt_center">100 (84, 112)</td>
<td headers="stat_2" class="gt_row gt_center">97 (85, 109)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Family_History_CVD</td>
<td headers="stat_1" class="gt_row gt_center">34 (23%)</td>
<td headers="stat_2" class="gt_row gt_center">53 (34%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Variable_Nas</td>
<td headers="stat_1" class="gt_row gt_center">199 (180, 203)</td>
<td headers="stat_2" class="gt_row gt_center">200 (180, 207)</td></tr>
  </tbody>
  <tfoot>
    <tr class="gt_footnotes">
      <td class="gt_footnote" colspan="3"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span> <span class='gt_from_md'>Median (Q1, Q3); n (%)</span></td>
    </tr>
  </tfoot>
</table>
</div>
```

### Adding statistical tests with `add_p()`


``` r
data %>%
    tbl_summary(by = Sex) %>%
    add_p() # Adds p-values for group comparisons
```

```{=html}
<div id="mzsxmcwcpa" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#mzsxmcwcpa table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#mzsxmcwcpa thead, #mzsxmcwcpa tbody, #mzsxmcwcpa tfoot, #mzsxmcwcpa tr, #mzsxmcwcpa td, #mzsxmcwcpa th {
  border-style: none;
}

#mzsxmcwcpa p {
  margin: 0;
  padding: 0;
}

#mzsxmcwcpa .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#mzsxmcwcpa .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#mzsxmcwcpa .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#mzsxmcwcpa .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#mzsxmcwcpa .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mzsxmcwcpa .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mzsxmcwcpa .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mzsxmcwcpa .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#mzsxmcwcpa .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#mzsxmcwcpa .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#mzsxmcwcpa .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#mzsxmcwcpa .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#mzsxmcwcpa .gt_spanner_row {
  border-bottom-style: hidden;
}

#mzsxmcwcpa .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#mzsxmcwcpa .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#mzsxmcwcpa .gt_from_md > :first-child {
  margin-top: 0;
}

#mzsxmcwcpa .gt_from_md > :last-child {
  margin-bottom: 0;
}

#mzsxmcwcpa .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#mzsxmcwcpa .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#mzsxmcwcpa .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#mzsxmcwcpa .gt_row_group_first td {
  border-top-width: 2px;
}

#mzsxmcwcpa .gt_row_group_first th {
  border-top-width: 2px;
}

#mzsxmcwcpa .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mzsxmcwcpa .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#mzsxmcwcpa .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#mzsxmcwcpa .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mzsxmcwcpa .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mzsxmcwcpa .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#mzsxmcwcpa .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#mzsxmcwcpa .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#mzsxmcwcpa .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mzsxmcwcpa .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mzsxmcwcpa .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mzsxmcwcpa .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mzsxmcwcpa .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mzsxmcwcpa .gt_left {
  text-align: left;
}

#mzsxmcwcpa .gt_center {
  text-align: center;
}

#mzsxmcwcpa .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#mzsxmcwcpa .gt_font_normal {
  font-weight: normal;
}

#mzsxmcwcpa .gt_font_bold {
  font-weight: bold;
}

#mzsxmcwcpa .gt_font_italic {
  font-style: italic;
}

#mzsxmcwcpa .gt_super {
  font-size: 65%;
}

#mzsxmcwcpa .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#mzsxmcwcpa .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#mzsxmcwcpa .gt_indent_1 {
  text-indent: 5px;
}

#mzsxmcwcpa .gt_indent_2 {
  text-indent: 10px;
}

#mzsxmcwcpa .gt_indent_3 {
  text-indent: 15px;
}

#mzsxmcwcpa .gt_indent_4 {
  text-indent: 20px;
}

#mzsxmcwcpa .gt_indent_5 {
  text-indent: 25px;
}

#mzsxmcwcpa .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#mzsxmcwcpa div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="label"><span class='gt_from_md'><strong>Characteristic</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_1"><span class='gt_from_md'><strong>Men</strong><br />
N = 146</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_2"><span class='gt_from_md'><strong>Women</strong><br />
N = 154</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="p.value"><span class='gt_from_md'><strong>p-value</strong></span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>2</sup></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Age</td>
<td headers="stat_1" class="gt_row gt_center">50 (35, 65)</td>
<td headers="stat_2" class="gt_row gt_center">49 (34, 64)</td>
<td headers="p.value" class="gt_row gt_center">0.5</td></tr>
    <tr><td headers="label" class="gt_row gt_left">BMI</td>
<td headers="stat_1" class="gt_row gt_center">25.4 (22.7, 27.4)</td>
<td headers="stat_2" class="gt_row gt_center">24.1 (21.5, 27.3)</td>
<td headers="p.value" class="gt_row gt_center">0.088</td></tr>
    <tr><td headers="label" class="gt_row gt_left">SBP</td>
<td headers="stat_1" class="gt_row gt_center">120 (109, 131)</td>
<td headers="stat_2" class="gt_row gt_center">120 (112, 130)</td>
<td headers="p.value" class="gt_row gt_center">0.8</td></tr>
    <tr><td headers="label" class="gt_row gt_left">HADS_Anxiety</td>
<td headers="stat_1" class="gt_row gt_center">10 (5, 16)</td>
<td headers="stat_2" class="gt_row gt_center">11 (5, 16)</td>
<td headers="p.value" class="gt_row gt_center">0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Cholesterol</td>
<td headers="stat_1" class="gt_row gt_center">199 (180, 225)</td>
<td headers="stat_2" class="gt_row gt_center">200 (180, 221)</td>
<td headers="p.value" class="gt_row gt_center">>0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Glucose</td>
<td headers="stat_1" class="gt_row gt_center">100 (84, 112)</td>
<td headers="stat_2" class="gt_row gt_center">97 (85, 109)</td>
<td headers="p.value" class="gt_row gt_center">0.6</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Family_History_CVD</td>
<td headers="stat_1" class="gt_row gt_center">34 (23%)</td>
<td headers="stat_2" class="gt_row gt_center">53 (34%)</td>
<td headers="p.value" class="gt_row gt_center">0.034</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Variable_Nas</td>
<td headers="stat_1" class="gt_row gt_center">199 (180, 203)</td>
<td headers="stat_2" class="gt_row gt_center">200 (180, 207)</td>
<td headers="p.value" class="gt_row gt_center">0.6</td></tr>
  </tbody>
  <tfoot>
    <tr class="gt_footnotes">
      <td class="gt_footnote" colspan="4"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span> <span class='gt_from_md'>Median (Q1, Q3); n (%)</span></td>
    </tr>
    <tr class="gt_footnotes">
      <td class="gt_footnote" colspan="4"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>2</sup></span> <span class='gt_from_md'>Wilcoxon rank sum test; Pearson’s Chi-squared test</span></td>
    </tr>
  </tfoot>
</table>
</div>
```

### Stratified Tables with `tbl_strata()`


``` r
data %>%
    tbl_strata(
        strata = Family_History_CVD, # Stratify by family history of CVD
        .tbl_fun = ~ .x %>% tbl_summary(by = Sex)
    )
```

```{=html}
<div id="uuylmtiuvo" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#uuylmtiuvo table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#uuylmtiuvo thead, #uuylmtiuvo tbody, #uuylmtiuvo tfoot, #uuylmtiuvo tr, #uuylmtiuvo td, #uuylmtiuvo th {
  border-style: none;
}

#uuylmtiuvo p {
  margin: 0;
  padding: 0;
}

#uuylmtiuvo .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#uuylmtiuvo .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#uuylmtiuvo .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#uuylmtiuvo .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#uuylmtiuvo .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#uuylmtiuvo .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#uuylmtiuvo .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#uuylmtiuvo .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#uuylmtiuvo .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#uuylmtiuvo .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#uuylmtiuvo .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#uuylmtiuvo .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#uuylmtiuvo .gt_spanner_row {
  border-bottom-style: hidden;
}

#uuylmtiuvo .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#uuylmtiuvo .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#uuylmtiuvo .gt_from_md > :first-child {
  margin-top: 0;
}

#uuylmtiuvo .gt_from_md > :last-child {
  margin-bottom: 0;
}

#uuylmtiuvo .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#uuylmtiuvo .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#uuylmtiuvo .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#uuylmtiuvo .gt_row_group_first td {
  border-top-width: 2px;
}

#uuylmtiuvo .gt_row_group_first th {
  border-top-width: 2px;
}

#uuylmtiuvo .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#uuylmtiuvo .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#uuylmtiuvo .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#uuylmtiuvo .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#uuylmtiuvo .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#uuylmtiuvo .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#uuylmtiuvo .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#uuylmtiuvo .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#uuylmtiuvo .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#uuylmtiuvo .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#uuylmtiuvo .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#uuylmtiuvo .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#uuylmtiuvo .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#uuylmtiuvo .gt_left {
  text-align: left;
}

#uuylmtiuvo .gt_center {
  text-align: center;
}

#uuylmtiuvo .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#uuylmtiuvo .gt_font_normal {
  font-weight: normal;
}

#uuylmtiuvo .gt_font_bold {
  font-weight: bold;
}

#uuylmtiuvo .gt_font_italic {
  font-style: italic;
}

#uuylmtiuvo .gt_super {
  font-size: 65%;
}

#uuylmtiuvo .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#uuylmtiuvo .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#uuylmtiuvo .gt_indent_1 {
  text-indent: 5px;
}

#uuylmtiuvo .gt_indent_2 {
  text-indent: 10px;
}

#uuylmtiuvo .gt_indent_3 {
  text-indent: 15px;
}

#uuylmtiuvo .gt_indent_4 {
  text-indent: 20px;
}

#uuylmtiuvo .gt_indent_5 {
  text-indent: 25px;
}

#uuylmtiuvo .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#uuylmtiuvo div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings gt_spanner_row">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="2" colspan="1" scope="col" id="label"><span class='gt_from_md'><strong>Characteristic</strong></span></th>
      <th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="2" scope="colgroup" id="level 1; stat_1_1">
        <div class="gt_column_spanner"><span class='gt_from_md'><strong>No</strong></span></div>
      </th>
      <th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="2" scope="colgroup" id="level 1; stat_1_2">
        <div class="gt_column_spanner"><span class='gt_from_md'><strong>Yes</strong></span></div>
      </th>
    </tr>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_1_1"><span class='gt_from_md'><strong>Men</strong><br />
N = 112</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_2_1"><span class='gt_from_md'><strong>Women</strong><br />
N = 101</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_1_2"><span class='gt_from_md'><strong>Men</strong><br />
N = 34</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_2_2"><span class='gt_from_md'><strong>Women</strong><br />
N = 53</span><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Age</td>
<td headers="stat_1_1" class="gt_row gt_center">50 (36, 66)</td>
<td headers="stat_2_1" class="gt_row gt_center">48 (34, 65)</td>
<td headers="stat_1_2" class="gt_row gt_center">46 (34, 62)</td>
<td headers="stat_2_2" class="gt_row gt_center">50 (35, 58)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">BMI</td>
<td headers="stat_1_1" class="gt_row gt_center">25.3 (22.5, 27.5)</td>
<td headers="stat_2_1" class="gt_row gt_center">24.5 (21.7, 27.5)</td>
<td headers="stat_1_2" class="gt_row gt_center">25.5 (22.8, 27.3)</td>
<td headers="stat_2_2" class="gt_row gt_center">23.8 (21.0, 25.8)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">SBP</td>
<td headers="stat_1_1" class="gt_row gt_center">120 (109, 130)</td>
<td headers="stat_2_1" class="gt_row gt_center">120 (110, 130)</td>
<td headers="stat_1_2" class="gt_row gt_center">120 (111, 133)</td>
<td headers="stat_2_2" class="gt_row gt_center">120 (113, 130)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">HADS_Anxiety</td>
<td headers="stat_1_1" class="gt_row gt_center">10 (5, 16)</td>
<td headers="stat_2_1" class="gt_row gt_center">11 (5, 16)</td>
<td headers="stat_1_2" class="gt_row gt_center">10 (6, 18)</td>
<td headers="stat_2_2" class="gt_row gt_center">11 (5, 18)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Cholesterol</td>
<td headers="stat_1_1" class="gt_row gt_center">201 (181, 226)</td>
<td headers="stat_2_1" class="gt_row gt_center">201 (185, 218)</td>
<td headers="stat_1_2" class="gt_row gt_center">197 (179, 212)</td>
<td headers="stat_2_2" class="gt_row gt_center">200 (171, 228)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Glucose</td>
<td headers="stat_1_1" class="gt_row gt_center">102 (85, 113)</td>
<td headers="stat_2_1" class="gt_row gt_center">95 (84, 106)</td>
<td headers="stat_1_2" class="gt_row gt_center">97 (82, 109)</td>
<td headers="stat_2_2" class="gt_row gt_center">102 (87, 115)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Variable_Nas</td>
<td headers="stat_1_1" class="gt_row gt_center">200 (181, 203)</td>
<td headers="stat_2_1" class="gt_row gt_center">200 (185, 207)</td>
<td headers="stat_1_2" class="gt_row gt_center">197 (179, 204)</td>
<td headers="stat_2_2" class="gt_row gt_center">200 (171, 206)</td></tr>
  </tbody>
  <tfoot>
    <tr class="gt_footnotes">
      <td class="gt_footnote" colspan="5"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;line-height:0;"><sup>1</sup></span> <span class='gt_from_md'>Median (Q1, Q3)</span></td>
    </tr>
  </tfoot>
</table>
</div>
```

``` r
#~ .x %>% tbl_summary(by = Sex) is a shortcut syntax in R for defining an anonymous (or lambda) function. 


# This is equivalent to writing function(.x) .x %>% tbl_summary(by = Sex).
#In this context, .x represents each subset of the data created by the stratification.
#tbl_summary(by = Sex) is applied to each subset, creating a summary table for each stratified group.
```

### Creating Cross Tables with `tbl_cross()`

You can create cross-tabulations, similar to contingency tables.


``` r
table(data$Sex, data$Family_History_CVD)
```

```
##        
##          No Yes
##   Men   112  34
##   Women 101  53
```

``` r
data %>%
    tbl_cross(
        row = Sex,
        col = Family_History_CVD
    )
```

```{=html}
<div id="mcuiujdfax" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#mcuiujdfax table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#mcuiujdfax thead, #mcuiujdfax tbody, #mcuiujdfax tfoot, #mcuiujdfax tr, #mcuiujdfax td, #mcuiujdfax th {
  border-style: none;
}

#mcuiujdfax p {
  margin: 0;
  padding: 0;
}

#mcuiujdfax .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#mcuiujdfax .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#mcuiujdfax .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#mcuiujdfax .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#mcuiujdfax .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mcuiujdfax .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mcuiujdfax .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mcuiujdfax .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#mcuiujdfax .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#mcuiujdfax .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#mcuiujdfax .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#mcuiujdfax .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#mcuiujdfax .gt_spanner_row {
  border-bottom-style: hidden;
}

#mcuiujdfax .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#mcuiujdfax .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#mcuiujdfax .gt_from_md > :first-child {
  margin-top: 0;
}

#mcuiujdfax .gt_from_md > :last-child {
  margin-bottom: 0;
}

#mcuiujdfax .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#mcuiujdfax .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#mcuiujdfax .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#mcuiujdfax .gt_row_group_first td {
  border-top-width: 2px;
}

#mcuiujdfax .gt_row_group_first th {
  border-top-width: 2px;
}

#mcuiujdfax .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mcuiujdfax .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#mcuiujdfax .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#mcuiujdfax .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mcuiujdfax .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mcuiujdfax .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#mcuiujdfax .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#mcuiujdfax .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#mcuiujdfax .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mcuiujdfax .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mcuiujdfax .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mcuiujdfax .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mcuiujdfax .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mcuiujdfax .gt_left {
  text-align: left;
}

#mcuiujdfax .gt_center {
  text-align: center;
}

#mcuiujdfax .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#mcuiujdfax .gt_font_normal {
  font-weight: normal;
}

#mcuiujdfax .gt_font_bold {
  font-weight: bold;
}

#mcuiujdfax .gt_font_italic {
  font-style: italic;
}

#mcuiujdfax .gt_super {
  font-size: 65%;
}

#mcuiujdfax .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#mcuiujdfax .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#mcuiujdfax .gt_indent_1 {
  text-indent: 5px;
}

#mcuiujdfax .gt_indent_2 {
  text-indent: 10px;
}

#mcuiujdfax .gt_indent_3 {
  text-indent: 15px;
}

#mcuiujdfax .gt_indent_4 {
  text-indent: 20px;
}

#mcuiujdfax .gt_indent_5 {
  text-indent: 25px;
}

#mcuiujdfax .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#mcuiujdfax div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings gt_spanner_row">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="2" colspan="1" scope="col" id="label"></th>
      <th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="2" scope="colgroup" id="level 1; stat_1">
        <div class="gt_column_spanner"><span class='gt_from_md'>Family_History_CVD</span></div>
      </th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="2" colspan="1" scope="col" id="stat_0"><span class='gt_from_md'>Total</span></th>
    </tr>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_1"><span class='gt_from_md'>No</span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="stat_2"><span class='gt_from_md'>Yes</span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Sex</td>
<td headers="stat_1" class="gt_row gt_center"><br /></td>
<td headers="stat_2" class="gt_row gt_center"><br /></td>
<td headers="stat_0" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Men</td>
<td headers="stat_1" class="gt_row gt_center">112</td>
<td headers="stat_2" class="gt_row gt_center">34</td>
<td headers="stat_0" class="gt_row gt_center">146</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Women</td>
<td headers="stat_1" class="gt_row gt_center">101</td>
<td headers="stat_2" class="gt_row gt_center">53</td>
<td headers="stat_0" class="gt_row gt_center">154</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Total</td>
<td headers="stat_1" class="gt_row gt_center">213</td>
<td headers="stat_2" class="gt_row gt_center">87</td>
<td headers="stat_0" class="gt_row gt_center">300</td></tr>
  </tbody>
  
</table>
</div>
```

### Regression table with `tbl_regression()`

If you want to create a regression table, you can fit a model and use `tbl_regression()` to summarize the results. Use `gtsave()` to save the results. 


``` r
model <- lm(Cholesterol ~ Age + Sex + BMI, data = data)
summary(model)
```

```
## 
## Call:
## lm(formula = Cholesterol ~ Age + Sex + BMI, data = data)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -77.394 -20.480  -0.201  22.180  79.460 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 202.86854   12.74511  15.917   <2e-16 ***
## Age           0.01257    0.10073   0.125    0.901    
## SexWomen     -1.11486    3.52738  -0.316    0.752    
## BMI          -0.09220    0.46110  -0.200    0.842    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 30.36 on 296 degrees of freedom
## Multiple R-squared:  0.0004917,	Adjusted R-squared:  -0.009638 
## F-statistic: 0.04854 on 3 and 296 DF,  p-value: 0.9858
```

``` r
tbl_regression(model) %>% as_gt() 
```

```{=html}
<div id="jweprkgrac" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#jweprkgrac table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#jweprkgrac thead, #jweprkgrac tbody, #jweprkgrac tfoot, #jweprkgrac tr, #jweprkgrac td, #jweprkgrac th {
  border-style: none;
}

#jweprkgrac p {
  margin: 0;
  padding: 0;
}

#jweprkgrac .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#jweprkgrac .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#jweprkgrac .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#jweprkgrac .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#jweprkgrac .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#jweprkgrac .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#jweprkgrac .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#jweprkgrac .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#jweprkgrac .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#jweprkgrac .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#jweprkgrac .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#jweprkgrac .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#jweprkgrac .gt_spanner_row {
  border-bottom-style: hidden;
}

#jweprkgrac .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#jweprkgrac .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#jweprkgrac .gt_from_md > :first-child {
  margin-top: 0;
}

#jweprkgrac .gt_from_md > :last-child {
  margin-bottom: 0;
}

#jweprkgrac .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#jweprkgrac .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#jweprkgrac .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#jweprkgrac .gt_row_group_first td {
  border-top-width: 2px;
}

#jweprkgrac .gt_row_group_first th {
  border-top-width: 2px;
}

#jweprkgrac .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#jweprkgrac .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#jweprkgrac .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#jweprkgrac .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#jweprkgrac .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#jweprkgrac .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#jweprkgrac .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#jweprkgrac .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#jweprkgrac .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#jweprkgrac .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#jweprkgrac .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#jweprkgrac .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#jweprkgrac .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#jweprkgrac .gt_left {
  text-align: left;
}

#jweprkgrac .gt_center {
  text-align: center;
}

#jweprkgrac .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#jweprkgrac .gt_font_normal {
  font-weight: normal;
}

#jweprkgrac .gt_font_bold {
  font-weight: bold;
}

#jweprkgrac .gt_font_italic {
  font-style: italic;
}

#jweprkgrac .gt_super {
  font-size: 65%;
}

#jweprkgrac .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#jweprkgrac .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#jweprkgrac .gt_indent_1 {
  text-indent: 5px;
}

#jweprkgrac .gt_indent_2 {
  text-indent: 10px;
}

#jweprkgrac .gt_indent_3 {
  text-indent: 15px;
}

#jweprkgrac .gt_indent_4 {
  text-indent: 20px;
}

#jweprkgrac .gt_indent_5 {
  text-indent: 25px;
}

#jweprkgrac .katex-display {
  display: inline-flex !important;
  margin-bottom: 0.75em !important;
}

#jweprkgrac div.Reactable > div.rt-table > div.rt-thead > div.rt-tr.rt-tr-group-header > div.rt-th-group:after {
  height: 0px !important;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="label"><span class='gt_from_md'><strong>Characteristic</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="estimate"><span class='gt_from_md'><strong>Beta</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="conf.low"><span class='gt_from_md'><strong>95% CI</strong></span></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="p.value"><span class='gt_from_md'><strong>p-value</strong></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Age</td>
<td headers="estimate" class="gt_row gt_center">0.01</td>
<td headers="conf.low" class="gt_row gt_center">-0.19, 0.21</td>
<td headers="p.value" class="gt_row gt_center">>0.9</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Sex</td>
<td headers="estimate" class="gt_row gt_center"><br /></td>
<td headers="conf.low" class="gt_row gt_center"><br /></td>
<td headers="p.value" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Men</td>
<td headers="estimate" class="gt_row gt_center">—</td>
<td headers="conf.low" class="gt_row gt_center">—</td>
<td headers="p.value" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Women</td>
<td headers="estimate" class="gt_row gt_center">-1.1</td>
<td headers="conf.low" class="gt_row gt_center">-8.1, 5.8</td>
<td headers="p.value" class="gt_row gt_center">0.8</td></tr>
    <tr><td headers="label" class="gt_row gt_left">BMI</td>
<td headers="estimate" class="gt_row gt_center">-0.09</td>
<td headers="conf.low" class="gt_row gt_center">-1.0, 0.82</td>
<td headers="p.value" class="gt_row gt_center">0.8</td></tr>
  </tbody>
  <tfoot>
    <tr class="gt_sourcenotes">
      <td class="gt_sourcenote" colspan="4"><span class='gt_from_md'>Abbreviation: CI = Confidence Interval</span></td>
    </tr>
  </tfoot>
</table>
</div>
```

## Exercises

We will work with the `SNPSdataset.txt` (find it in the weekly assignments [submission section](https://aules.uvic.cat/mod/assign/view.php?id=1086918)).

Create a report using rmarkdown. Include in your report:

-   the proportion of cases and controls.

-   the proportion of women and men.

-   the proportion of women and men within cases and controls, respectively.

-   summary table with the information for cases and controls, age and gene_expression values stratifying by sex.

-   summary table with the information for all SNPs genotypes when stratifying by sex

-   summary table with the information for all SNPs genotypes when stratifying by sex and case/control

Alternate the use of tables and plots (basic plots from session 2) to display the information. Work with the functions included in the `dplyr`and `tidyr` packages.

The desired format to submit the report is an html. **It is important to describe the results that you show to answer each question. Is not enough providing the code** (*Example: The dataset is mainly defined by XX % of women and % men. When we stratify by clinical status, we observe a higher percentage of women in controls than in cases*).

### Extra exercises

You can also try to report the same results with rmarkdown in other formats (word, pdf). If you want, you can try to work with other options that we have seen in class (Sweave, Shiny, Beamer). It is not mandatory and it does not have to be submitted. *Only the HTML file of the markdown*. If you want to work with shiny, you can continue the following R code: `ShinySNPSapp.R`. Some indications are explained in the script in order to display the main information.

---

![](logouvic.png)
