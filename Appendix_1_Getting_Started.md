---
title: "Getting started with R (and RStudio)"
sidebar: toc
category: getting-started
order: 1
---

# What is R?

* R is a computer **language** (like C or C++).
* The _R Graphical User Interface_ (RGui for Windows) / _R Application_ (R.app for Mac OS X) is a terminal-like window that communicates with your computer to interpret the commands written in the R-language 

<img src="./Ressources/RGui.png" width="425" height="300"> <img src="./Ressources/Rapp.png" width="425" height="300">

**Interesting:** You can also run the code in a UNIX or BASH window by typing the command **`R`**.

## Obtaining R:

[cran.r-project.org](https://cran.r-project.org)

|**Windows**                                 |**Mac OS X**                                        |
|--------------------------------------------|----------------------------------------------------|
|$>$ `Download for Windows`                  |$>$ `Download for Mac OS X`                         |
|$> >$ `base`                                |$> >$ `R-3.6.1.pkg`                                 |           
|$> > >$ `Download R 3.6.1 for Windows`      |                                                    |
|open installer and start installation wizard|download and start installer                        |
|(choose default settings)                   |(choose default settings)                           |

# What is an IDE?

* An _Integrated Development Environment_ is an **application** (like Word or Excel).
* It helps you to organize and maintain your code.
* Free available applications:

    * **RStudio** is most commonly used (Windows, Mac OS X)
    * Eclipse with StatET (Windows, Mac OS X)
    * Emacs / Aquamax with ESS (Linux, Mac OS X)
    * Vi, Vim and GVim (Linux)

**NOTE!** Using Microsoft Word instead of an IDE to write (or save) code is generally a bad idea. Certain keyboard characters, such as quotations “”, are not the same in Word. The difference is largely indistinguishable to the human eye, but will not run in R.

## Obtaining RStudio:

[www.rstudio.com](https://www.rstudio.com/)


|**Windows**                                    |**Mac OS X**                                    |
|-----------------------------------------------|------------------------------------------------|
|$>$ `Download RStudio`                         |$>$ `Download RStudio`                          |
|$>>$ `Download RStudio Desktop`                |$>>$ `Download RStudio Desktop`                 |
|(FREE open source licence)                     |(FREE open source licence)                      |
|$>>>$ `RStudio 1.2.1335 - Windows 7+`          |$>>>$ `RStudio 1.2.1335 - macOS 10.12+`         |


# A Primer on RStudio

RStudio offers...

  * a source code editor that supports direct code execution,
  * workspace management,
  * debugging, syntax-highlighting, intelligent code completion,
  * easy communication with other softwar and plattforms... (and more)
  
![](./Ressources/RStudio_Intro1.png)

**Panel A: R Console**

  * Runs R-code and returns results.
    
**Panel B: Workspace management**

  * Environment (list of all objects in the workspace).
  * History (record of commands executed during the worksession).
    
**Panel C: Other functionality**

  * See files in your directory.
  * View plots.
  * See list of installed and active packages. 
  * Access R help.


## Script (.R)

The sequence of commands necessary for analysis is typically **written down** (_scripted_) in textfiles prior to execution.

Advantages:

* Documentation of tasks (allows you and others to reproduce a task).
* Automation of repetitive tasks (time saving).
* Evaluation of incremental changes.

### Creating a script file

![](./Ressources/RStudio_Script1.png)
![](./Ressources/RStudio_Script2.png)

### Writting a script

![](./Ressources/RStudio_Script3.png)

### Execute single code line

![](./Ressources/RStudio_Script4.png)
![](./Ressources/RStudio_Script5.png)

### Execute code selection

![](./Ressources/RStudio_Script6.png)
![](./Ressources/RStudio_Script7.png)

### Saving a script file

<img src="./Ressources/RStudio_Script9a.png" width="425" height="300"> <img src="./Ressources/RStudio_Script9b.png" width="425" height="300">

<center>
<img src="./Ressources/RStudio_Script9c.png" width="350" height="300"> 
</center>

![](./Ressources/RStudio_Script10.png)

### Open an existing script file from the current directory

![](./Ressources/RStudio_Script10b.png)

### Open an existing script file from another directory

![](./Ressources/RStudio_Script11a.png)




