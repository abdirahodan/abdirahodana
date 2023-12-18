---
title: "Lost Buzz: Tracking Trends and Factors in Bee Colony Losses"
subtitle: "Raising awareness of threats facing honeybees and their indispensable pollination services through data-driven storytelling."
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
okay
---
### Key Threats

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
The stressor data set, focuses on external stress factors affecting be colonies -pesticides, diseases, climate etc. It links stressors to time frames and locations, as well as the impacts in percentage of colonies affect per quarter.

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
 **It's a joy to use.**

In using this theme for your next static website project, you won't need to know
anything about Tachyons ... so, don't freak out. Even though I used it to style
the theme, you won't need to change a thing. BUT, if you do want to play around
with it, you can make massive changes very easily. Just familiarize yourself
with the [clear documentation on the design system](http://tachyons.io/docs/).
Once you dive in, you'll recognize all the classes I'm using in the markup.

### BYOTachyons

One of the best features of Tachyons is the exhaustive [component
library](https://www.tachyonstemplates.com/components/?selectedKind=AboutPages&selectedStory=AboutUs&full=0&down=0&left=1&panelRight=0)
contributed by the community. All those components are built to work with the
Tachyons classes, so they will work in this theme too! You can copy/paste
components in order to quickly block out a page, then fill in your content.

### Taste the Rainbow

We've leveraged the [accessible color
combinations](http://tachyons.io/docs/themes/skins/) included with Tachyons to
offer an easy way for you to setup your site using your favorite colors. In the
site configuration file (`config.toml`), there is a full set of color parameters
giving you control over the theme color scheme. For an option like `siteBgColor`
for example, you can just type one of the predefined color names from Tachyons
and save the file. You can totally customize the theme colors within minutes of
installing the theme.

```toml
# basic color options: use only color names as shown in the
# "Color Palette" section of http://tachyons.io/docs/themes/skins/
siteBgColor = "near-white"
sidebarBgColor = "light-gray"
headingColor = "black"
textColor = "dark-gray"
sidebarTextColor = "mid-gray"
bodyLinkColor = "blue"
navLinkColor = "near-black"
sidebarLinkColor = "near-black"
footerTextColor = "silver"
buttonTextColor = "near-white"
buttonBgColor = "black"
buttonHoverTextColor = "white"
buttonHoverBgColor = "blue"
borderColor = "moon-gray"
```

### Dig Deeper

Let's say you have a style guide to follow and `washed-blue` just won't cut the
mustard. We built Blogophonic for you, too. There is a bypass of these
predefined colors built in, you just need to dig a little deeper. In the theme
assets, locate and open the main SCSS file (`/assets/main.scss`). After the
crazy looking variables you probably don't recognize and directly following the
Tachyons import (`@import 'tachyons';`) you'll see a comment that looks just
like this:

```scss
// uncomment the import below to activate custom-colors
// add your own colors at the top of the imported file
// @import 'custom-colors';
```

Once you uncomment the `custom-colors` import, it will look like this:

```scss
// uncomment the import below to activate custom-colors
// add your own colors at the top of the imported file
@import "custom-colors";
```

Save that change, and now the color options in the `config.toml` are no longer
active – they've been bypassed. To customize the colors, locate and open the
`custom-colors` file found in the theme assets (`/assets/custom-colors.scss`).
At the top of that file, you'll find a whole new set of variables for all the
same color options, but this time you get to assign your own HEX codes.

```scss
// set your custom colors here
$siteBgColorCustom: #e3e3da;
$sidebarBgColorCustom: #dbdbd2;
$textColorCustom: #666260;
$sidebarTextColorCustom: #666260;
$headingColorCustom: #103742;
$bodyLinkColorCustom: #c4001a;
$navLinkColorCustom: #c4001a;
$sidebarLinkColorCustom: #c4001a;
$footerTextColorCustom: #918f8d;
$buttonTextColorCustom: #f7f7f4;
$buttonHoverTextColorCustom: #f9f9f8;
$buttonBgColorCustom: #103742;
$buttonHoverBgColorCustom: #c4001a;
$borderColorCustom: #c4beb9;
```
