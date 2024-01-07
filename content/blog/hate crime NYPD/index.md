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

``` toml
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

| **Column Name**                   | **Description**                                |
|:--------------------------------|---------------------------------------|
| **Full Complaint ID**             | Identifier for each hate crimes incident       |
| **Complaint Year Number**         | Year in which incident occurred                |
| **Month Number**                  | Month in which incident occurred               |
| **Record Create Date**            | Date report was filed                          |
| **Complaint Precinct Code**       | NYPD Precinct in which incident occurred       |
| **Patrol Borough Name**           | NYPD Patrol Borough in which incident occurred |
| **County**                        | County in which incident occurred              |
| **Law Code Category Description** | Category of offense                            |
| **Offense Description**           | A description of the offense                   |
| **PD Code Description**           | The NYPD description of the offense            |
| **Bias Motive Description**       | NYPD category of hate crime or bias type       |
| **Offense Category**              | General categorization of hate crime type      |
| **Arrest Date**                   | Date arrest was made (if arrest happened)      |
| **Arrest Id**                     | Identifier for arrest (if made)                |

Columns in this Data set

Columns in this Data set

## Exploratory Data Analysis (EDA)

In our analysis of New York City hate crimes, we shift our focus to whether an arrest was made, as indicated by NYPD records. This study aims to identify key factors influencing arrest outcomes in hate crime cases, such as the type of offense, location, time, and bias motivation. By exploring these elements, we intend to understand better how effectively and consistently hate crimes are addressed by law enforcement, providing insights for policy-making and resource allocation. This project seeks to answer: "Can we predict the likelihood of an arrest in hate crime incidents based on specific incident characteristics?

#### Temporal Patterns

A graph showing the temporal distribution of hate crime rates from 2019 to 2023 serves as the first visual aid for our investigation. This analysis aids in identifying possible seasonal patterns, the effects of particular events, and long-term changes in the number of hate crimes. These kinds of insights are essential for determining the peak times of hate crimes, which facilitates effective resource allocation and preventive strategy development.

``` toml
temp<-data%>%
  group_by(`Complaint Year Number`,`Month Number`)%>%
    summarize(Count = n())

ggplot(temp, aes(x = `Month Number`, y = Count, group = `Complaint Year Number`, color = as.factor(`Complaint Year Number`))) +
  geom_line() +
  labs(title = "Monthly Hate Crime Incidents Over Years",
       x = "Month",
       y = "Count of Incidents",
       color = "Year") +
  scale_x_continuous(breaks = 1:12, labels = month.abb)+
  theme_bw()
```

![Formspree Logo](temperal.png)

The graph presents a five-year trend of monthly hate crime incidents, highlighting fluctuations and certain months with heightened activity, such as February 2022 and May 2021. These variations suggest that specific times may see a higher rate of incidents, although no solid annual trend emerges, pointing to the influence of different factors each year. A mid-year rise in incidents is generally observed, indicating possible seasonal influences. The year begins with moderate incident levels, often dipping in April, except in unusual years like 2020, which likely reflects the unique societal impact of the COVID-19 pandemic. The data eventually levels out, suggesting a baseline that periodically rises or falls. This analysis confirms that hate crime patterns are not uniform year to year, highlighting the complexity of hate crime trends and the significance of external elements.

#### Geographical Distribution

Next, we shift our focus to the geographical distribution of hate crimes, analyzing how they are dispersed across different precincts or boroughs. This graph provides a breakdown by offense category, offering a clearer view of where certain types of hate crimes are more concentrated. Understanding these patterns is crucial as it helps law enforcement and community leaders to target interventions, allocate resources more effectively, and potentially uncover underlying causes linked to specific areas.

``` toml
geo <- data %>%
  group_by(`Offense Category`, `Patrol Borough Name`) %>%
  summarise(counts = n()) # 

ggplot(geo, aes(x = `Patrol Borough Name`, y = counts, fill = `Patrol Borough Name`)) +
  geom_bar(stat = 'identity', position = position_dodge()) +
  scale_fill_brewer(palette="Set2") +
   theme_bw()+
  facet_wrap(~`Offense Category`, scales = 'free_y') + 
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
  labs(title = "Hate Crime Counts by Offense Category across Patrol Boroughs",
       x = "Patrol Borough Name",
       y = "Count of Incidents")
```

![Formspree Logo](geo.png)

The data analysis indicates a disparity in the distribution of hate crimes across various boroughs, with particular offenses such as those motivated by race/color and religion displaying pronounced variations in frequency. Specific boroughs, notably Brooklyn North and Manhattan South, report a higher number of race/color-related incidents. Similarly, hate crimes based on religious practices are predominantly observed in the Brooklyn and Manhattan areas. Despite the diversity of hate crimes across boroughs, there seems to be a recurring pattern of certain areas consistently reporting higher incidents, which should be considered when devising prevention strategies.

These geographical trends suggest that factors such as local societal tensions or historical contexts may contribute to the prevalence of hate crimes in these precincts. On the other hand, the relatively minimal figures for hate crimes related to age and disability could indicate less frequent occurrences or a discrepancy in reporting rates for these types of offenses.

#### Exploring the Link of Offense and Law

When it comes to the relationship between offense categories and legal classifications, it is insightful to examine which offenses fall under which law code categories in the legal system's hierarchy. Since various sorts of crimes are grouped according to their magnitude, this categorization not only tells us about the frequency of particular offenses but also illuminates the perceived seriousness of each incidence.

**Violations**: *These are minor offenses, typically punishable by fines and not resulting in jail time. Examples include speeding tickets and jaywalking.*

**Misdemeanors**: *More serious than violations but less severe than felonies, misdemeanors can lead to up to a year in jail, fines, probation, or community service. Common examples include simple assault and DUI.*

**Felonies:** *The most serious offenses, classified by severity from Class A (most serious) to Class E. They can be violent or non-violent and often result in significant prison time. A Class A felony, like first-degree murder, could lead to life imprisonment.*
