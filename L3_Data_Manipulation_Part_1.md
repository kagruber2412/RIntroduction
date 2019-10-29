# The data science process


# Discover

* Real-world data is generally noisy, incomplete and inconsistent. The initial exploration of the raw data set helps you to spot such data quality problems. 

* Scanning your data also helps you to discover first insights into it and provides guidance on applying the right kind of further statistical treatment to it.


# Prepare and Analyse

* Analytical models fed with poor quality data can lead to misleading predictions. The data preparation stage resolves data issues and ensures the dataset used in the modeling stage is acceptable and of improved quality. 

* Data preparation tasks are likely to be performed multiple times during interactive data analysis and model building stages (also called **data wrangling** or **data munging**). That's why data prepartion typically consumes around 80% of overall time of an analytics project.


## Stage 1: Discover

Data inspection constitutes a set of simple tools to answer questions like:

Question 1: What is the size the data set? 

Question 2: What variables are included? 

Question 3: Are there implausible/ missing values?

Question 4: How are values distributed over variables? 

**Example:** _Ben\&Jerry ice-cream_. Subsample of the _Nielson homescan data_,a consumer panel consisting of 70,000 households and all of their purchases. 


**Question 1: What is the size of the data set?**

Check the number of columns and rows (or alternatively the dimension) of the data set:

```{r}
nrow(BenAndJerry)
ncol(BenAndJerry)
dim(BenAndJerry)
```
