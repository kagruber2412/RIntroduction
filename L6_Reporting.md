# The data science process

<img src="./Ressources/DataScienceProcess_Reports.png" width="1225" height="300" style="margin: 0px 50px">

It doesn’t matter how great your analysis is unless you can explain it to others: you need to **communicate** your results!

* Communicating to **decision makers**, who want to focus on the conclusions and not the code behind the analysis.

* Collaborating with **other (data) scientists**, who are interested in both the conclusions, and the code.

# RMarkdown

(See also R for Data Science, Chapter 1 \& 22, or https://r4ds.had.co.nz/r-markdown.html Chapter 5.27, 5.29 \& 5.30)

RMarkdwon is a file format for designing documents that allow to combine code, results, and written text and to store your results in a variety of formats. 

RMarkdown documents rely on three different frameworks:

1. **YAML** for render parameters

2. **knitr** for embedded R code

3. **markdown** for formatted text

> Reports are fully reproducible (although code is not necessarily displayed).

> Reports can be updated automatically with `knitr` (time saving, e.g. when placing figures and tables).

**NOTE!** The necessary add-on packages (`rmarkdown` and `knitr`) are automatically installed in your R package library when installing RStudio. But RStudio does not build PDF and Word documents from scratch. You will need to have Microsoft Word (or a similar program) installed to produce Word Files. For rendering pdf files you will also need a **TeX** distribution (**miktex** for windows https://miktex.org/download, **mactex** for mac https://tug.org/mactex/mactex-download.html).

# Getting started

A template RMarkdown script is provided. 


## 1. YAML for render parameters

The YAML header (at the top of the page in between two lines of three dashes) includes the set up information used by `knitr` during rendering to produce the file:

Selection of available document output formats:

| Format | Document | Presentation |
|--------|----------------|--------------------------|
| HTML | `html_document` | `ioslides_presentation`, `xaringan`, `reveal.js` |
| PDF | `pdf_document` | `beamer_presentation` |
| Word | `word_document` | `powerpoint_presentation` |

* A **table of contents** can be added with the `toc` option. The depth of headers that it applies to is specified with the `toc\_depth` option:

~~~~
---
title: "My first document"
output:
  pdf_document:
    toc: true
    toc_depth: 2
---
~~~~

If the table of contents depth is not explicitly specified, it defaults to 3 (meaning that all level 1, 2, and 3 headers will be included in the table of contents).

* **Section numberings** can be added to the headers eith the `number\_sections` option:

~~~~
---
title: "My first document"
output:
  pdf_document:
    toc: true
    number_sections: true
---
~~~~

* **Width** and **height** of graphical output can be controlled (for example) with the `fig\_width` and `fig\_height` options:

~~~~
---
title: "My first document"
output:
  pdf_document:
    fig_width: 7
    fig_height: 6
---
~~~~

* Enhance the default display of **data frames** (output) with the `df\_print` option:

~~~~
---
title: "My first document"
output:
  pdf_document:
    df_print: kable
---
~~~~

* Change the **syntax highlighting** style:

~~~~
---
title: "My first document"
output:
  pdf_document:
    highlight: tango
---
~~~~

Available highlighting styles are: default, tango, pygments, kate, monochrone, espresso, zenburn, haddock, null (prevents syntax highlighting).

* Further customizations of the template to create the PDF document:

~~~~
---
title: "My first document"
output: 
  pdf_document:
    fontsize: 11pt
---
~~~~

| Setting | Description |
|---------------------------------|--------------------------------------------------|
| `fontsize` | Font size (e.g., 10pt, 11pt, or 12pt) |
| `documentclass` | LaTeX document class (e.g., `article`) |
| `classoption` | Options for documentclass (e.g., `oneside`) |
| `geometry` | Options for geometry class (e.g., `margin=1in`) |
| `linkcolor`, `urlcolor`,  `citecolor` | Color for internal, external, and citation links |


## 2. Markdown formatted text

Markdown is a set of very easy-to-read conventions for formatting plain text:

* bold and italic text

* ordered and unordered lists

* headers (section titles)

* hyperlinks...

<br>

**Markdown reference guide**: 

Toolbar > **Help** > **Markdown Quick Reference** 

<br>

## Mathematical expressions

* **Inline** mathematical expressions can be written in a pair of dollar signs using the LaTeX syntax (see e.g. https://www.overleaf.com/learn/latex/Mathematical_expressions):

~~~~~
$f(k) = {n \choose k} p^{k} (1-p)^{n-k}$
~~~~~

The output looks like: $f(k) = {n \choose k} p^{k} (1-p)^{n-k}$.


* **Display style** mathematical expressions can be written in a pair of double dollar signs:
