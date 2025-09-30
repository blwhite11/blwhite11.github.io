# SECTION 1. Errors you may find in R


## Installing R

**Missing R installation** - Before installing Rstudio, we have to install R, as Rstudio is an integrated development environment for R.

**Permission denied while installing packages** -  We must run R/Rstudio (.exe file) as an administrator or install packages in a directory where we have write permissions (Usually packages are saved in the following directory "C:/Program Files/R/R-versionXXXXX")

We need to allow the installer to make changes in our device - the installer will have the necessary administrative privileges to write files to system directories.


**Errors related to missing system dependencies** - We have to install the necessary system dependencies.


**Disk space issues** - We have to remove unnecessary files or applications to free up space in our disk space.


## Loading packages

**“ERROR: this R is version 3.1.2, package 'XXXXXXXXXX' requires R >= 3.2.3."** - We probably need to download the latest version for R (update R).

How to proceed:


1. Download the latest version of R, for Windows, from CRAN at : https://cran.r-project.org/bin/windows/base/

2. From the tab "Packages" in RStudio, update the libraries. 



## Defining directories

**“Warning: cannot open file 'C:/Users/pathtothefile/file.txt': No such file or directory Error in file(file, "rt") : cannot open the connection"**

We have to be careful, is not the same defining the path for windows than for mac.

We can check how our system defines the path to a working directory by typing getwd().

Usually in windows we define a path as “C:/Users/pathtothefile” and in mac as "/Users/pathtothefile".


**Issues with long file paths exceeding system limits** - We have to shorten the file/directory names.

**Error in file(file, "rt") : cannot open the connection - Issues with paths containing spaces** - We can enclose paths with spaces in double quotes.

Example: "C:/XXX/Program Files/R/" -  "C:/XXX/"Program Files"/R/"



## Resources you have

* **Online forums and communities we can check for help**: [Stack Overflow](https://stackoverflow.com/questions/1299871/how-to-join-merge-data-frames-inner-outer-left-right?noredirect=1&lq=1), [R-bloggers](https://www.r-bloggers.com/)

* **Cheatsheets** - [Posit Rstudio Cheatsheet Updates](https://posit.co/blog/cheat-sheet-updates/)

* **Other**  

--- 

![](logouvic.png)


