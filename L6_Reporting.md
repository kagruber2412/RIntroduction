# The data science process

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
