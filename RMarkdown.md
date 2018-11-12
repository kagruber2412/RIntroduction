---
title: "R Markdown"
sidebar: toc
category: getting-started
order: 1
---

# Markdown and R Markdown

**Markdown**
- A simple (easy-to-write / easy-to-read) text-based markup language using *plaintext* formatting syntax.
- Is quickly becoming a writting standard for academics and scientists (e.g. GitHub uses Markdown). 

**R Markdown**
- An R package, and a set of tools embedded in RStudio, that facilitates the construction of documents that combine formatted text, chunks and embedded R code. R Markdown documents rely on three frameworks:

1. **Markdown** for formatted text
2. **knitr** for embedded R code
3. **YAML** for render parameters

- Allows documentation in a variety of formats for creating dynamic and reproducible documents in R .
 
# 1. Markdown for formatted text

Markdown is a set of conventions for formatting *plaintext* (the regular alphabet, with a few familiar symbols, like asterisks  ( * ) and backticks (` `)). Allows controlling the display of the text:

- bold and italics
- organizing lists
- creating headers
- adding hyperlinks

*Syntax guide*: Toolbar > **Help** > **Markdown Quick Reference**

# 2. knitr for embedded R code

The knitr package extends the basic Markdown syntax to include chunks of executable R code.

*knitr guide*: Toolbar > **Help** > **Markdown Quick Reference**

- When *rendering* the document, knitr will run the code and append the results to the code chunk.
- Provides formatting and syntax highlighting to both the code and its results.

## Code chunk options

- `name` (chunk label, not necessarily requored)
- `echo = TRUE` (sisplay the code chunk or just show the results?)
- `eval = TRUE` (run the code in the code chunk?)
- `warning = TRUE` (display warning messages in the document?)
- `message = TRUE` (display code messages in the document?)
- `results` (specify appearance of the the results)
- `fig.align` (specify figure to appear `right`, `left`, or `center` aligned)
- `fig.height`, `fig.width` (specify height and width of the figure)
- `out.width`, `out.height (width/height to which plots are scaled in the final document)

# 3. YAML for render parameters

The YAML header controls how R Markdown renders the `.Rmd` file.

## Documents

- `output: html_document` (generates html document, default)
- `output: pdf_document` (generates pdf document)
- `output: word_document` (generates word document)
- `output: html_notebook` (generates an html notebook)

**NOTE:**
* Notebooks are **previewed**. (A preview is a rendered html copy of the content displayed in the editor. Consequently, unlike Knit, Preview does not run any R code chunks. Instead, the output of the chunk - when it was last run in the editor - is displayed.)
* For R Markdown documents, all the code is sent to the console at once, but for R Notebooks, only one line at a time is sent. Allows execution to stop if a line raises an error.

### Table of contents 

- `toc: true` (generates the table of contents)
- `toc_depth: 2` (table of contents includes sections and subsections)
- `number_sections: true` (include section numberings)

### Figure options

- `fig_width: 7` (controls the default figure width)
- `fig_height: 6` (controls the default figure height)
- `fig_caption: true` (controls whether figures are rendered with captions)

### Data frame printing

- `df_print: default`
- `df_print: kable`
- `df_print: tibble`

### Syntax highlighting

- `highlight: default`
- `highlight: tango`
- `highlight: pygments`
- `highlight: kate`
- `highlight: monochrone`
- ...
- `highlight: null` (prevents syntax highlighting)

## Presentations

**Built-in formats**

- `output: powerpoint_presentation` (ppt document)
- `output: beamer_presentation` (pdf presentation)
- `output: ioslides_presentation` (html presentation)

**Usage**

- `#`, `##` headers indicate a new slide
- `---` indicates a new slide without header

### Power Point

- `reference_doc: my-styles.pptx` (customizes the appearance of the presentation by passing a custom reference document)

### Beamer

Appearance features:

- `theme: "default"`
- `theme: "AnnArbor"`
- `theme: "Berlin"`
- `theme: "CambridgeUS"`
- `theme: "Copenhagen"`
- `theme: "..."`

Colortheme features:

- `colortheme: "default"`
- `colortheme: "albatross"`
- `colortheme: "beetle"`
- `colortheme: "dolphin"`
- `colortheme: "..."`

See also:

https://hartwork.org/beamer-theme-matrix/

### Ioslides

Features: 

- `incremental: true` (incremental bullets, e.g., `> -` Bullet 1)
- `widescreen: true` (wider form factor)
- `smaller: true` (smaller text)
- `transition: default` (transition speed: `slower`, `faster`, or a numeric value with a number of seconds)
- `|` add subtitle to a slide or section 
- `logo: logo.png` (adding a logo)

**NOTE:** Images and tables will always be placed on new slides (togehter with the slide header and image/table caption). 


# Getting started

1. Open a new R Markdown file: Toolbar > **File** > **New File** > **R Markdown**

![](Ressources/Markdown1.png)

![](Ressources/Markdown2.png)

A template R Markdown script is provided. This includes the set up information at the top of the page in between two lines of three dashes (YAML settings used by knitr during rendering to produce the file). 

2. Render the file.

R Markdown will use the **pandoc** program to transform the file into a new format. R Markdown will preserve the text, code results, and formatting contained in the original `.Rmd` file.

![](Ressources/Markdown4.png)

**NOTE:** 
* The selection you make will override the output!


## Executing Code

* Using the green triangle button on the toolbar of a code chunk: `Run Current Chunk`

![](Ressources/Markdown9.png)

* Using the hotkey combination `Ctrl + Enter` (macOS: `Cmd + Enter`)

* Using the editor toolbar

![](Ressources/Markdown10.png)

After code execution, an indicator will appear in the gutter to show the execution progress. Sent lines are marked in green, lines that have not yet been sent are marked with light green. 

![](Ressources/Markdown12.png)

Output appears beneath the code chunk that produced it.

![](Ressources/Markdown13.png)

Output can be removed by using:

![](Ressources/Markdown14.png)

## Errors 

Execution stops and the remaining lines of that chunk (and any chunks that have not yet been run) are not executed.

The line of code that caused the error havs a red indicator in the editor’s gutter.

![](Ressources/Markdown15.png)


# (Useful) ressources

- https://rmarkdown.rstudio.com
- https://bookdown.org/yihui/rmarkdown/


# Tricks to obtain nicer output

Function `kable()` from package `knitr` takes a matrix or data frame object and turns it into a nicely formatted table for use with R Markdown.

```{r}
# create some random sample data
data <- matrix(rnorm(1000000, mean=0, sd=10), nrow = 100000, ncol = 10)
```

Using the `knitr` package
```{r eval=FALSE}
knitr::kable(summary(data))
```

```{r, eval=FALSE}
# Creating a summary table
df <- rbind(colMeans(data),colSums(data),dim(data)[1])
rownames(df) <- c("Means","Sums","N")
knitr::kable(df)
```

Using the `kableExtra` package
```{r, eval=FALSE}
library(kableExtra)

# HTML table
kable(df, format = "html", caption = "Demo Table") %>%
  kable_styling(bootstrap_options = "striped", full_width = F) %>%
  add_footnote(c("table footnote"))

# LaTeX Table
kable(df, format = "latex", booktabs = T, caption = "Demo Table") %>%
  kable_styling(latex_options = c("striped", "hold_position"), full_width = F) %>%
  add_footnote(c("table footnote"))
```

Using the `stargazer` package (for more settings see `?stargazer`)
```{r, eval=FALSE}
library(stargazer)

# HTML table
stargazer(df, type = "html", digits = 2,
          summary.stat = c("mean","sd","median","min", "max"))
```

Using the `formattable` package
```{r}
# read in the facebook data
data <- read.csv2("facebook_data.csv")
# overriding the colnames
colnames(data) <- c("ID", "Achievement", "Facebookhours", "No.Friends")

library(formattable)

# Print out the first 10 observations
formattable(data[1:10,])
```

The function `formattable()` of the `formattable` add-on package calls `knitr::kable()` internally to translate data frame to HTML code.

```{r}
Adding **orange** color codes for the academic achievements. 
Adding a **green bar** (like a barchart) for the number of facebook hours spent.

formattable(data[1:10,], list(
  Achievement = color_tile("white", "orange"),
  Facebookhours = color_bar("lightgreen"),
  No.Friends = color_bar("lightblue")
))
```




