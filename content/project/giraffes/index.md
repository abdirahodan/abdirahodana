---
title: "Lost Buzz: Tracking Trends and Factors in Bee Colony Losses"
subtitle: "Raising awareness of threats facing honeybees and their indispensable pollination services through data-driven storytelling."
date: 2020-08-11
author: "Hodan Abdirahman"
draft: false
tags:
  - hugo-site
categories:
  - Theme Features
  - R
  - package
# layout options: single or single-sidebar
layout: single
---
## Key Threats

As a child, I feared bees for their painful stings without appreciating their vital ecological role. While stings are unpleasant, bees only sting to defend themselves and their hive. In fact, bees are prolific pollinators sustaining diverse ecosystems and enabling successful food crop production that our agricultural system relies on.

Declining bee populations from pesticides, habitat loss, climate change, and disease is alarming. My wariness of stings now mixes with awe at bees’ resilience. Though stings briefly hurt, I accept the discomfort knowing it connects me to the expansive pollination systems bees make possible.

My goal is to raise awareness of threats facing honeybees and their indispensable pollination services through data-driven storytelling.

Let’s begin by uploading the data. The data particular comes from U.S Department of Agriculture, but tidytuesday github has made it easier to load and extract.

```toml
colony <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-01-11/colony.csv')
stressor <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-01-11/stressor.csv'
```
We will be implementing two various data sets.

The Colony data set, focuses on quantifying bee colonies directly, provides colony count and change metrics by year, month and state. Overall captures trajectory of total bee colonies and population changes.
```toml
# A tibble: 1,222 × 10
    year months        state     colony_n colony_max colony_lost colony_lost_pct
   <dbl> <chr>         <chr>        <dbl>      <dbl>       <dbl>           <dbl>
 1  2015 January-March Alabama       7000       7000        1800              26
 2  2015 January-March Arizona      35000      35000        4600              13
 3  2015 January-March Arkansas     13000      14000        1500              11
 4  2015 January-March Californ…  1440000    1690000      255000              15
 5  2015 January-March Colorado      3500      12500        1500              12
 6  2015 January-March Connecti…     3900       3900         870              22
 7  2015 January-March Florida     305000     315000       42000              13
 8  2015 January-March Georgia     104000     105000       14500              14
 9  2015 January-March Hawaii       10500      10500         380               4
10  2015 January-March Idaho        81000      88000        3700               4
# ℹ 1,212 more rows
# ℹ 3 more variables: colony_added <dbl>, colony_reno <dbl>,
#   colony_reno_pct <dbl>
```
The stressor data set, focuses on external stress factors affecting be colonies -pesticides, diseases, climate etc. It links stressors to time frames and locations, as well as the impacts in percentage of colonies affect per quarter.
```toml
# A tibble: 7,332 × 5
    year months        state   stressor              stress_pct
   <dbl> <chr>         <chr>   <chr>                      <dbl>
 1  2015 January-March Alabama Varroa mites                10  
 2  2015 January-March Alabama Other pests/parasites        5.4
 3  2015 January-March Alabama Disesases                   NA  
 4  2015 January-March Alabama Pesticides                   2.2
 5  2015 January-March Alabama Other                        9.1
 6  2015 January-March Alabama Unknown                      9.4
 7  2015 January-March Arizona Varroa mites                26.9
 8  2015 January-March Arizona Other pests/parasites       20.5
 9  2015 January-March Arizona Disesases                    0.1
10  2015 January-March Arizona Pesticides                  NA  
# ℹ 7,322 more rows
```
Im interested in understanding the changes in colony numbers over time in various US states. Let’s begin by examining data from six randomly selected states to gain a clearer perspective. To ensure diversity, I’ve chosen states from different regions.

```toml
states_sample<-colony %>%
  filter(state %in% c('Oregon','Iowa','Louisiana','Vermont','California','Pennsylvania'))

ggplot(states_sample, aes(x = year, y = colony_n, color = months)) +
  geom_line() +
  facet_wrap(~ state, ncol = 2, scales = "free_y") +
  labs(title = "Total Colony Numbers Over Time Across Different US States",
  x = "Year", y = "Colony Numbers") +
  theme_minimal()
```
![Formspree Logo](timestate.png)


Most states display a similar pattern of fluctuation in colony numbers across years and months. For example, Pennsylvanian and Vermont display peaks in between fall to winter and fall rather than summer. Meanwhile, California stands out for having a relatively stable colony count over time, without the seasonal highs and lows seen in other states. One commonality is that all states are missing data for April through June of 2018. This gap in the middle of the year makes it difficult to fully assess seasonal trends for that time period. Additional years of data could help determine if 2018 is an anomaly or if spring months regularly lack measurements.


## Overall Trends

This analysis looks at recent 5-year colony trends between 2015-2019 given the available data range. To prepare the data, I filtered the data set to only include complete years within this period for valid comparisons. Examining the states with extreme high or low net changes over this short time frame highlights rapidly improving or worsening areas that may warrant closer investigation.

This is the five states with the biggest growth:
![Formspree Logo](growth.png)

This is the five states with the biggest decline:

![Formspree Logo](decline.png)

The slope charts reveal Texas as a dramatic positive outlier with a net growth of around 115,000 colonies, over double any other state. Florida, Michigan, Minnesota, and Wisconsin also saw substantial gains between 25,000 to 42,000. On the decline side, most states experienced gradual drops of -15,000 or less, but Ohio and Maryland saw steeper decreases nearing -50,000 colonies.

Seeing states like Texas and Florida with large expansions could point to favorable climates, land availability, or agricultural practices supporting beekeeping. Meanwhile, the states with steep declines may reflect urbanization pressure, reduced foraging habitat, or climate stresses negatively impacting colonies. Comparing adjoining states in different directions could help identify regional factors that differentiate success and struggles.

While these net changes highlight growth and declines, they do not account for proportional size differences among states. Larger states can have bigger swings in raw totals. Analyzing changes per existing colony or other metrics could improve normalization. Additionally, linking to stressor and land use data may reveal correlations to explore in future work.

The stressor data set does not directly quantify climate change impacts, it does include variables like disease prevalence that can be linked to weather factors. For example, research shows Varroa mite infestations increase when temperatures rise but decline during heavy rains and high winds (Rowland, 2021).

I want to explore the potential correlation between diseases affected by climate and colony loss rates. Specifically, we can use the data on Varroa mite effects as a proxy for temperature-sensitive diseases. If colonies affected by Varroa mites also exhibit higher loss percentages, it suggests a connection between increasing heat and colony collapses.

Examining relationships between diseases like Varroa mites and colony losses can provide indirect insight into how climate shifts may be impacting bee populations. While not a perfect measure, analyzing these available variables can act as a starting point.

```toml
merged_data <- left_join(colony, stressor, by = c("year", "months", "state"))

mites_data <- merged_data %>%
  filter(stressor == "Varroa mites")

correlation_result <- cor.test(mites_data$stress_pct, mites_data$colony_lost_pct)

(correlation_result)
```
![Formspree Logo](stats.png)

```toml
ggplot(mites_data, aes(x = stress_pct, y = colony_lost_pct)) +
  geom_point(color="brown") +
  labs(title = "Correlation between Varroa mites and Colony Loss Rates",
       x = "Varroa mites",
       y = "Colony Loss Rates (%)")+
  theme_minimal()
```
![Formspree Logo](correlation.png)
As we can see there is a steady slow increase, between the factors of Varroa mites and the percentage of bee colonies lost.

The statistical test to measure the correlation, states there does seem to be a significant connection between these two factors. Specifically, the analysis showed that when a higher percentage of colonies have Varroa mite infestations, a higher percentage of colonies are also lost. And when fewer colonies have mites, fewer colonies are lost.

The correlation test suggested this is a moderate strength positive relationship, so the trends don’t match perfectly, but they seem closely related based on the data.

We can’t definitively prove the mites directly caused the losses from this. But the significant correlation does suggest that these medical issues faced by bee colonies are linked to their survival rates.

## Geographic Patterns
For this analysis we need to load sf package, as we are going to make a choropleth map of the United State. We want to find how colony losses vary across the continental. We might see some region vary in shade compared to other places.

```toml
merged_data <- left_join(states_needed, colony, by = c("NAME" = "state"))

ggplot(merged_data, aes(fill = colony_max)) +
  geom_sf() +
  coord_sf(xlim = c(-125, -65), ylim = c(25, 50))+
  labs(title = "Colony Losses Across the US",
       fill = "Colony Losses") +
  scale_fill_viridis_c(option = 'mako') +  
  theme_minimal()
```
![Formspree Logo](map.png)

Based on the map data, there isn’t a strikingly significant difference in color indicating regional patterns of colony loss rates across the United States. However, certain states do stand out with higher losses, notably California, which recorded over estimated around 900,000 colony losses.

California’s major agricultural industry likely explains its high colony losses. As the top producer of many bee-pollinated crops like almonds, declines in honey bee health could significantly impact crop pollination and food production there. California also has major commercial beekeeping operations that are affected by high mortality rates.

According to an article by Aaron Smith, California produces about 80% of the world’s almonds in over 1 million acres of orchards. In February, 90% of all US honey bees are transported there to pollinate almond blooms. This reliance on imported bees, despite increasing almond acreage, may pressure honey bee health.

States like Texas and Florida saw growth in colonies over the past five years. Texas has gained beekeeping popularity and implemented supportive regulations. Florida produced 8 million pounds of honey in 2021 from over 650,000 colonies( USDA 2022) However, its climate also aids bee pests and diseases that can raise colony los

In summary, the enormous role of agriculture and bee-pollinated crops in states like California, Texas, and Florida make honey bee health especially important. Their large commercial operations and warmer climates also introduce stressors that can negatively impact colonies. Managing bee health in major farm states is key to supporting crop pollination and food supply nationwide.

## Seasonal Analysis

 **It's a joy to use.**


