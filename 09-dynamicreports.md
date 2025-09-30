# SECTION 8. Dynamic Reports 

## What is LATEX?

LATEX is different from other type setting systems in that you just have to tell it the logical and semantic structure of a text. It then derives the typographical form of the text according to the 'rules' given in the document class file and in various style files. LATEX allows users to structure their documents with a variety of hierarchical constructs, including chapters, sections, subsections and paragraphs.

The standard document structure of a LATEX file includes the definition of the document class, where the document starts and ends. Additional information can include title, author(s) and affiliation of the author(s) as well as a table of content and different chapters and sections within the document. Figures, tables and a bibliography are also very common in most LATEX documents. The following code shows a basic LATEX document structure with title, table of contents and a section where the text of the document is entered.


``` r
\documentclass[11pt,a4paper,oneside]{report}
\begin{document}
\title{Basic \LaTeX document structure with table of contents}
\author[1]{Your Name}
\affil[1]{Some Affiliation}
\maketitle
\tableofcontents
\section{Section Header}
Some text.
...
\end{document}
```

More details in: `usrguide_latex.pdf`

## What is Sweave?
* A set of R functions, written by Friedrich Leisch
(http://www.statistik.lmu.de/~leisch), working under one command in utils package

* Processes R code within a LaTex document

* Returns output from such code 

* Creates plots and automatically creates the LaTex code for their inclusion 

* A LaTex package and style (~\R\R-3.0.2\share\texmf\tex\latex\Sweave.sty)


### How to install Sweave

* Assuming LaTex and R are installed, there is no need for installation (in case you need to install LaTex: https://www.latex-project.org/get/) (https://tug.org/texlive/windows.html#install) 

- Sweave is distributed with R (since version 1.5.0)

- It is included in the `utils` package (no need to load it)

#### An easy way is to run Sweave in Rstudio: 

  - open a new file from File, R Sweave

  - just click on compile pdf button after you have edited the .Rnw file


#### How does it work?


##### The Noweb syntax: Code Chunks in Sweave

- To separate code and documentation chunks, the **Noweb** syntax is used

- Noweb is a literate programming tool which allows to combine program source code and the corresponding documentation into a single file

- Different segments are called chunks:
  - < < options > >= denotes the start of code chunk,
  - @ denotes the start of a documentation chunk.

- Two kind of operations:
   - weave: typeset documentation together with code
   - tangle: extract code chunks in a .R file
  

``` r
<<chunkname>>=
a <- 1
b <- 4
print(a+b)
@
```

Each code chunk is translated into LATEX code by Sweave() as follows:


``` r
\begin{Schunk}
\begin{Sinput}
> a <- 1
> b <- 4
> print(a + b)
\end{Sinput}
\begin{Soutput}
[1] 5
\end{Soutput}
\end{Schunk}
```

And finally after creating a pdf from the L ATEXdocument, the result in the pdf file is:


``` r
> a <- 1
> b <- 4
> print(a + b)
[1] 5
```


#### The Sweave R package

The Sweave package is part of the base installation of R (everybody should have it within the standard R installation), for more details see:

``` r
help("Sweave", package = "utils")
```

The two main functions of the Sweave package are:

* `Sweave()`: Produces suitable documentation from the main source files. `Rnw -> tex`

* `Stangle()`: Produces programming code from the main source files. `Rnw -> R`


## Basic options for code chunks

Multiple options can be placed in the construct <<>>= to control how the code in the chunks is executed (bold means default setting):

* `eval` (TRUE, FALSE): Whether the R chunk is run. If FALSE, the code chunk is not evaluated, and hence no text or graphical output produced (default is TRUE).

* `echo` (TRUE, FALSE): Whether the R chunk is shown in the LATEX file (default is FALSE)

* `results` (verbatim, tex, hide): Type of output used to show the printed results produced by the R code. 'hide' will show no output at all (default is verbatim).
 
* `fig`: (TRUE, FALSE): If TRUE it includes the plot created in the code (default is FALSE).

* `width`: numeric, width of figures in inch

* `height`: numeric, height of figures in inch


###  For costly computations: `cacheSweave` package

* Add `cache=TRUE` to computationally expensive chunks

* After the first run, objects from the cached chunks keep stored in a hash map
 
* Subsequent Sweaves do not evaluate these chunks
  
*  Cached chunks must be computations only (NO FIGURES)


- Options can be set globally at the beginning of the file (and changed everywhere else) with


``` r
\SweaveOpts{option1,option2,...}
```


### Example

We created a code chunk named `summaryFUN` with no additional options set. Hence, the R code is executed and displayed as code insight a verbatim environment in LATEX:


``` r
<<summaryFUN>>=
custom.summary <- function(x) {
return(list(Mean = mean(x), Max = max(x), Min = min(x), Summary = summary(x),
Head = head(x), Class = class(x), Length = length(x)))
}
@
```

Now the `custom.summary` function is created in our R environment and can be called in following code chunks. 
For instance, we take the cars data that is included in the R base installation. The cars object includes the data of speed and the distances taken to stop of cars.


``` r
<<chunkresults>>=
result <- custom.summary(cars$speed)
result
@
```

The `custom.summary` function returns a list that is stored into the result object and printed out. Going to more advanced Sweave use cases, we can define our own plotting function or use existing once (for example `plot()`, `hist()`, `boxplot()`, ...) to integrate figures in our document.

For a simple example we use another car dataset that is included in the R base installation which includes a table of 11 aspects of automobile design and performance for 32 cars from 1973-1974. 

A histogram plot of the miles per gallon data for the 32 cars results in:

``` r
<<chunkgraph>>=
hist(mtcars$mpg)
@
```

Sweave and R also allow displaying tables in the document and creating these directly from for example a data frame or matrix. Therefor we have to load the `xtable` package in R, create the LATEX code with `xtable()` and set the results option to tex to directly get LATEX code without interpreting it from Sweave.


``` r
<<chunktable>>=
library(xtable)
texTable <- sapply(mtcars, function(x) summary(x))
xtable(texTable)
@
```



## The Beamer class: LATEX presentations

Beamer is a LATEX class for creating presentations.

### Table of Contents


``` r
\begin{frame}
\frametitle{Outline}
\end{frame}
```

### Sections and Subsections


``` r
\section{Motivation}
\subsection{The Basic Problem That We Studied}
```
Notice that these commands are given outside of frames.

### Frames and structures:

Presentations are divided in frames. Each frame can be generated as follow:


``` r
\begin{frame}
\frametitle{What Are Prime Numbers?}
A prime number is a number that has exactly two divisors.
\end{frame}
```

We can structure frames in block environments:


``` r
\begin{frame}
\frametitle{What's Still To Do?}
\begin{block}{Answered Questions}
How many primes are there?
\end{block}
\begin{block}{Open Questions}
Is every even number the sum of two primes?
\end{block}
\end{frame}
```


An alternative way to structure our frames is the `itemize` statement:

``` r
\begin{frame}
\frametitle{What's Still To Do?}
\begin{itemize}
\item Answered Questions
\begin{itemize}
\item How many primes are there?
\end{itemize}
\item Open Questions
\begin{itemize}
\item Is every even number the sum of two primes?
\end{itemize}
\end{itemize}
\end{frame}
```


* Example: Creating a Beamer Presentation

1. Structure Your Presentation


``` r
\documentclass{beamer}
% This is the file main.tex
\usetheme{Berlin}
\title{Example Presentation Created with the Beamer Package}
\author{Till Tantau}
\date{\today}

\begin{document}
\begin{frame}
\titlepage
\end{frame}

\section*{Outline}
\begin{frame}
\tableofcontents
\end{frame}

\section{Introduction}

\subsection{Overview of the Beamer Class}

\subsection{Overview of Similar Classes}


\section{Usage}

\subsection{...}

\subsection{...}


\section{Examples}

\subsection{...}

\subsection{...}


\begin{frame}
...
\end{frame} % to enforce entries in the table of contents

\end{document}
```


2.  Create Frames: Once the table of contents looks satisfactory, start creating frames for your presentation by adding frame environments:

* The Frame Title: Put a title on each frame. 

* Structuring a Frame: Use block environments like block, theorem, proof, example, and so on

* Writing the Text

* Using Graphs: 


``` r
\includegraphics[scale=1]{file.png}
```

* Choosing Appropriate Themes and colors: http://deic.uab.es/~iblanes/beamer_gallery/




3. Creating a PDF or PostScript File presentation.

More details in: `beameruserguide.pdf`. 

You can also check the code of different beamer templates in overleaf:

- https://www.overleaf.com/project/63542ffd4269fe31b95215c3 

- https://www.overleaf.com/learn/latex/Beamer.



## `Knitr` package

* Prettier out of the box (than Sweave)

* Code re-formating: use option `tidy`.

* Code highlighting (with Sweave too but you need a `xycolor.sty` file in your latex distribution).

* Simple copy-paste enabled.

* Better approach to dealing with plots (for example `fig.keep` option for more than 2 plots in 1 chunk).

### `Knitr` main outputs

* `HTML`: via R Markdown files (`Rmd`).

* `PDF`: mostly through `Rnw` files or via `latex` conversion using in R the `pandoc('namefile', format='latex')` command.



## R Markdown

The rmarkdown package is a next generation implementation of R Markdown based on pandoc. This implementation brings many enhancements to R Markdown, including:

* Create HTML, PDF, and MS Word documents as well as Beamer, ioslides, and Slidy presentations.

* New markdown syntax including expanded support for tables, definition lists, and bibliographies.

* Hooks for customizing HTML and PDF output (include CSS, headers, and footers).

* Include raw LaTeX within markdown for advanced customization of PDF output.

* Compile HTML, PDF, or MS Word notebooks from R scripts.

* Extensibility: easily define new formats for custom publishing requirements.

* Create interactive R Markdown documents using Shiny.



### Installation

If you are working within RStudio then you can simply install the current release of RStudio (both the rmarkdown package and pandoc are included).

If you want to use the rmarkdown package outside of RStudio then you can install the package from CRAN as follows:


``` r
install.packages("rmarkdown")
```

A recent version of pandoc (>= 1.12.3) is also required. See the pandoc installation instructions for details on installing pandoc for your platform.

### Output Formats

R Markdown documents can contain a metadata section that includes both title, author, and date information as well as options for customizing output. For example, this metadata included at the top of an Rmd file adds a table of contents and chooses a different HTML theme:


``` r
---
title: "Sample Document"
output:
  html_document:
    toc: true
    theme: united
---
```

R Markdown has built in support for several output formats (HTML, PDF, and MS Word documents as well as Beamer presentations). These formats can also be specified in metadata, for example:


``` r
---
title: "Sample Document"
output:
  pdf_document:
    toc: true
    highlight: zenburn
---
```

If you aren't specifying format options you can also just use a simple format name:


``` r
---
title: "Sample Document"
output: pdf_document
---
```

Multiple formats can be specified in metadata:


``` r
---
title: "Sample Document"
output:
  html_document:
    toc: true
    theme: united
  pdf_document:
    toc: true
    highlight: zenburn
---
```


### Output Format Functions

Output formats need not be specified in metadata. In fact, metadata is just a convenient way to invoke functions that implement output formats. There are seven built-in output formats each exported as a function from the package:

* html_document

* pdf_document

* word_document

* md_document

* beamer_presentation

* ioslides_presentation

* slidy_presentation


### Chunk Options

Common:

* `eval` (TRUE, FALSE): Whether the R chunk is run. If FALSE, the code chunk is not evaluated, and hence no text or graphical output produced (default is TRUE).

* `echo` (TRUE, FALSE): Whether the R chunk is shown in the LATEX file (default is FALSE).

* `fig.width`, `fig.height`: R control.

Figure: 

* `fig.width`, `fig.height`: R control.

* `out.width`, `out.height`: output control.

* `fig.keep`: for more than 2 plots in 1 chunk.

* `fig.cap`: caption in Rnw only.

More details in: `rmarkdown-reference.pdf`




## Shiny

Shiny is an R package that makes easy to build interactive web applications straight from R. 

First of all, we need to install the Shiny package in R `install.packages("shiny")`

This package has different examples where each one of them is a self-contained Shinny app. 

We will look at first at *Hello Shiny* example, that works with the dataset `faithful`.

`app.R`: to take a look to the main code. 



In the following link we can access different examples: https://shiny.rstudio.com/tutorial/written-tutorial/lesson1/ 


a)  Reactivity

Run example 3 - `runExample("03_reactivity")`


b) Global variables

Run example 4- `runExample("04_mpg")`


In case you want to visualize some applications, you can also access the following link: https://shiny.rstudio.com/gallery/ 
 

Moreover, there is a new resource that allows the user to directly create the structure of the shinny app through an interactive editor: https://rstudio.github.io/shinyuieditor/. 


To use it we should follow the following steps:

1- Download the remotes apckage and upload it: `install.packages("remotes")` and `library(remotes)`.

2- Download the shinyuieditor package from github and upload it: `remotes::install_github("rstudio/shinyuieditor")` and `library(shinyuieditor)`.

3- Create a new app using the editor and see how it creates the code directly to a new R script called app.R: `setwd(path/to/file)` `shinyuieditor::launch_editor(app_loc = "Launch_app/")`



## Quarto

Includes new features and capabilities while at the same time being able to render most existing Rmd files without modification.

Quarto is open source, and it’s as friendly to Python, Julia, Observable JavaScript, and Jupyter notebooks as it is to R. It’s not a language-specific library (as R Markdown), but an external software application.


Different from R Markdown, in Quarto chunk options are typically included in special comments at the top of code chunks rather than within the line that begins the chunk:


``` r
#| echo: false
#| fig-cap: "Air Quality"
#| warning: false
```

Similar to R Markdown, the rendered version is viewed as HTML. We can also choose to render it into other formats like PDF, MS Word, etc.

What can we do with Quarto?

- books

- websites

- blogs

In this section, we’ll show  how to use it in RStudio. We will learn how to edit code just as we would do with any computational document (e.g. R Markdown), and preview the rendered document in the Viewer tab as we work.

Link to download a .qmd file (Quarto document): https://quarto.org/docs/get-started/hello/rstudio.html 

When we render a Quarto document: 

1. Knitr executes all of the code chunks

2. It creates a new markdown (.md) document that includes the code and its output

3. The markdown file is processed by pandoc: it creates the finished format.



## Available templates for each block

You will find them in the moodle, [link](https://aules.uvic.cat/mod/folder/view.php?id=965440)

**- Sweave**: `Sweave.Rnw`contains the code; `Sweave.pdf`contains the output. 

**- Markdown**: 

a) HTML: `Rmarkdown_html.RMD` contains the code; `Rmarkdown_html.hmtl` contains the output.

b) PDF: `Rmarkdown_pdf.RMD` contains the code; `Rmarkdown_pdf.pdf` contains the output.

c) WORD: `Rmarkdown_word.RMD` contains the code; `Rmarkdown_word.docx` contains the output.

**- Beamer**:

a) with Sweave: `Beamer.RNW` contains the code; `Beamer.pdf` contains the output.

b) with Markdown: `Beamer_markdown.RMD` contains the code; `Beamer_markdown.pdf` contains the output.


**- Quarto**: `quarto.QMD` contains the code; `quarto.html` contains the output.

**- Shiny**: `app.R` contains the code; we need to run the app to visualize the output.


![](logouvic.png)
