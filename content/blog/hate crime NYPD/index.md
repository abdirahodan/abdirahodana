---
title: "NYPD Hate Crimes"
excerpt: "Exploring hate crimes in cities is crucial for community well-being and safety, especially in a diverse setting like New York City."
date: 2023-12-15
author: "Hodan Abdirahman"
draft: false
tags:
  - hugo-site
categories:
  - crime
  - R
  - social science
layout: single
---
## Introduction

Exploring hate crimes in cities is crucial for community well-being and safety, especially in a diverse setting like New York City. The NYPD Hate Crimes dataset offers an in-depth look at these incidents in one of the world's most populous cities, capturing a variety of reported cases. This analysis is essential as hate crimes deeply affect community trust and target individuals based on immutable traits like race, religion, or sexuality. The shared goal of community leaders, policymakers, and law enforcement is to understand and address these crimes effectively.

This project focuses on analyzing the NYPD Hate Crimes dataset to uncover trends across different areas, times, and groups. It seeks to answer how hate crime trends in New York City vary by location, time, and demographic factors, and to identify key predictive elements. Using methods like logistic regression and tree-based models, the project aims to offer a clearer picture of hate crimes, aiding in more informed discussions and strategies to tackle this complex issue.

This data set was found in [NYC OpenData](https://data.cityofnewyork.us/Public-Safety/NYPD-Hate-Crimes/bqiq-cu78).

```{r,include=FALSE}
library("tidyverse");theme_set(theme_bw())
library("tidymodels");theme_set(theme_bw())
library("MASS")
library("class")
library(tidyverse)
library(dplyr)
library(lubridate)
library(ggplot2)
library(knitr)
library(tidyr)
library(pROC)
library(caret)
library("rpart")
library("rpart.plot")

#load the data
library(readr)
data <- read_csv("NYPD_Hate_Crimes_20231202.csv")

#clean the data by removing unwanted columns
data<-subset( data, select = -c(`Full Complaint ID`, `Arrest Date`) )

```

+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Column Name**]{.smallcaps}~                   | ~[**Description**]{.smallcaps}~                                |
+:==================================================+================================================================+
| ~[**Full Complaint ID**]{.smallcaps}~             | ~[Identifier for each hate crimes incident]{.smallcaps}~       |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Complaint Year Number**]{.smallcaps}~         | ~[Year in which incident occurred]{.smallcaps}~                |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Month Number**]{.smallcaps}~                  | ~[Month in which incident occurred]{.smallcaps}~               |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Record Create Date**]{.smallcaps}~            | ~[Date report was filed]{.smallcaps}~                          |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Complaint Precinct Code**]{.smallcaps}~       | ~[NYPD Precinct in which incident occurred]{.smallcaps}~       |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Patrol Borough Name**]{.smallcaps}~           | ~[NYPD Patrol Borough in which incident occurred]{.smallcaps}~ |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**County**]{.smallcaps}~                        | ~[County in which incident occurred]{.smallcaps}~              |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Law Code Category Description**]{.smallcaps}~ | ~[Category of offense]{.smallcaps}~                            |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Offense Description**]{.smallcaps}~           | ~[A description of the offense]{.smallcaps}~                   |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**PD Code Description**]{.smallcaps}~           | ~[The NYPD description of the offense]{.smallcaps}~            |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Bias Motive Description**]{.smallcaps}~       | ~[NYPD category of hate crime or bias type]{.smallcaps}~       |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Offense Category**]{.smallcaps}~              | ~[General categorization of hate crime type]{.smallcaps}~      |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Arrest Date**]{.smallcaps}~                   | ~[Date arrest was made (if arrest happened)]{.smallcaps}~      |
+---------------------------------------------------+----------------------------------------------------------------+
| ~[**Arrest Id**]{.smallcaps}~                     | ~[Identifier for arrest (if made)]{.smallcaps}~                |
+---------------------------------------------------+----------------------------------------------------------------+

: Columns in this Data set

## Exploratory Data Analysis (EDA)
