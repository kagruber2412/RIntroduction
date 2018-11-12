---
title: "RMarkdown"
sidebar: toc
category: getting-started
order: 1
---

# Introduction

- Easy-to-write / easy-to-read plain text format for making dynamic documents with R.
- Source code for rich, reproducible documents (contains formatted text and chunks of embedded R code)
- Allows documentation in a variety of formats.

R Markdown reports rely on three frameworks:

1. **markdown** for formatted text
2. **knitr** for embedded R code
3. **YAML** for render parameters

# Markdown formatted text

Markdown is a set of very easy to use conventions for formatting plain text:

- bold and italic text
- lists
- headers
- hyperlinks

See: Toolbar > **Help** > **Markdown Quick Reference**

# knitr for embedded R code

The knitr package extends the basic markdown syntax to include chunks of executable R code.

See: Toolbar > **Help** > **Markdown Quick Reference**

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

# YAML for render parameters

The YAML header controls how rmarkdown renders the `.Rmd` file.

# Dynamic documents

## Document

- `output: html_document` (generates html document, default)
- `output: pdf_document` (generates pdf document)
- `output: word_document` (generates word document)

## Table of contents 

- `toc: true` (generates the table of contents)
- `toc_depth: 2` (table of contents includes sections and subsections)
- `number_sections: true` (include section numberings)

## Figure options

- `fig_width: 7` (controls the default figure width)
- `fig_height: 6` (controls the default figure height)
- `fig_caption: true` (controls whether figures are rendered with captions)

## Data frame printing

- `df_print: default`
- `df_print: kable`
- `df_print: tibble`

## Syntax highlighting

- `highlight: default`
- `highlight: tango`
- `highlight: pygments`
- `highlight: kate`
- `highlight: monochrone`
- ...
- `highlight: null` (prevents syntax highlighting)

# Dynamic presentations

- Generate slides dynamically
- Embed R code into presentations

**Usage**

- `#`, `##` headers indicate a new slide
- `---` indicates a new slide without header

**Built-in formats**

- `output: powerpoint_presentation` (ppt document)
- `output: beamer_presentation` (pdf presentation)
- `output: ioslides_presentation` (html presentation)

## Power Point

- `reference_doc: my-styles.pptx` (customizes the appearance of the presentation by passing a custom reference document)

## Beamer

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

## Ioslides

Features: 

- `incremental: true` (incremental bullets, e.g., `> -` Bullet 1)
- `widescreen: true` (wider form factor)
- `smaller: true` (smaller text)
- `transition: default` (transition speed: `slower`, `faster`, or a numeric value with a number of seconds)
- `|` add subtitle to a slide or section 
- `logo: logo.png (adding a logo)

**NOTE:** Images and tables will always be placed on new slides (togehter with the slide header and image/table caption). 
