---
title: 'Weekly Exercises #3'
author: "Xintan Xia"
output: 
  html_document:
    theme: journal
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
```

```
## Warning: replacing previous import 'vctrs::data_frame' by 'tibble::data_frame'
## when loading 'dplyr'
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
## ✓ tibble  3.1.2     ✓ dplyr   1.0.1
## ✓ tidyr   1.1.1     ✓ stringr 1.4.0
## ✓ readr   1.3.1     ✓ forcats 0.5.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## Parsed with column specification:
## cols(
##   state = col_character(),
##   variable = col_character(),
##   year = col_double(),
##   raw = col_double(),
##   inf_adj = col_double(),
##   inf_adj_perchild = col_double()
## )
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>%
  mutate(week_day = wday(date, label = TRUE)) %>%
  group_by(vegetable, week_day) %>%
  summarize(tot_weight_lb = sum(weight) * 0.00220462) %>%
  pivot_wider(id_cols = vegetable,
              names_from = week_day,
              values_from = tot_weight_lb)
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>%
  group_by(vegetable, variety) %>%
  summarize(tot_weight_lb = sum(weight) * 0.00220462) %>%
  left_join(garden_planting %>% select(c("vegetable", "variety", "plot")),
            by = c("vegetable", "variety"))
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["tot_weight_lb"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A"},{"1":"peppers","2":"green","3":"5.69232884","4":"K"},{"1":"peppers","2":"green","3":"5.69232884","4":"O"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  For some vegetable varieties, there are multiple plots recorded for a single variety, and that's why the number of rows increases after using the `left_join()`. This may be quite misleading. Take the Bush Bush Slender beans as an example. People might think that the total harvest of the variety is exactly the same in plot M and D, and that Lisa harvest around 22.13lb of beans at each plot. Yet that is not the case, and the problem is that there is no plot information in the garden harvest data.  
  It's hard to find out a way to fix it.. Maybe the only way would be keeping track of at which plot were the beans harvested in the past.

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.  
  > It'll be helpful to use the `left_join()` function, joining the costs from the `garden_spending`, together with the prices of vegetables in the whole foods market, to the `garden_harvest` dataset. Then we can `mutate` a new variable calculating the "revenue" from vegetables planted by multiplying the price from the whole foods market website and the weight of each vagetable variety. The amount of money "saved" can be finally calculated by subtracting cost from the revenue.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>%
  filter(vegetable == "tomatoes") %>%
  group_by(variety) %>%
  summarize(tot_weight_lb = sum(weight) * 0.00220462,
            first_date = min(date)) %>%
  ggplot(aes(x = tot_weight_lb, y = fct_reorder(variety, first_date))) +
  geom_col() +
  labs(x = "",
       y = "",
       title = "Total harvest (lb) for each tomato variety",
       caption = "From the smallest first harvest date (grape) to the largest (volunteers)")
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>%
  mutate(lower_case_name = str_to_lower(variety),
         variety_name_length = str_length(variety)) %>%
  distinct(variety, .keep_all = TRUE) %>%
  arrange(vegetable, variety_name_length)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["lower_case_name"],"name":[6],"type":["chr"],"align":["left"]},{"label":["variety_name_length"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"2020-09-26","4":"156","5":"grams","6":"unknown","7":"7"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"asparagus","7":"9"},{"1":"basil","2":"Isle of Naxos","3":"2020-06-23","4":"5","5":"grams","6":"isle of naxos","7":"13"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"bush bush slender","7":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-08","4":"108","5":"grams","6":"chinese red noodle","7":"18"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"classic slenderette","7":"19"},{"1":"beets","2":"leaves","3":"2020-06-11","4":"8","5":"grams","6":"leaves","7":"6"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"sweet merlin","7":"12"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-07","4":"62","5":"grams","6":"gourmet golden","7":"14"},{"1":"broccoli","2":"Yod Fah","3":"2020-07-27","4":"372","5":"grams","6":"yod fah","7":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-09","4":"102","5":"grams","6":"main crop bravado","7":"17"},{"1":"carrots","2":"Dragon","3":"2020-07-24","4":"80","5":"grams","6":"dragon","7":"6"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"bolero","7":"6"},{"1":"carrots","2":"greens","3":"2020-08-29","4":"169","5":"grams","6":"greens","7":"6"},{"1":"carrots","2":"King Midas","3":"2020-07-23","4":"56","5":"grams","6":"king midas","7":"10"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"perrenial","7":"9"},{"1":"cilantro","2":"cilantro","3":"2020-06-23","4":"2","5":"grams","6":"cilantro","7":"8"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-11","4":"330","5":"grams","6":"dorinny sweet","7":"13"},{"1":"corn","2":"Golden Bantam","3":"2020-08-15","4":"383","5":"grams","6":"golden bantam","7":"13"},{"1":"cucumbers","2":"pickling","3":"2020-07-08","4":"181","5":"grams","6":"pickling","7":"8"},{"1":"edamame","2":"edamame","3":"2020-08-11","4":"109","5":"grams","6":"edamame","7":"7"},{"1":"hot peppers","2":"thai","3":"2020-07-20","4":"12","5":"grams","6":"thai","7":"4"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"variety","7":"7"},{"1":"jalapeño","2":"giant","3":"2020-07-17","4":"20","5":"grams","6":"giant","7":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-13","4":"10","5":"grams","6":"heirloom lacinto","7":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"2020-09-17","4":"191","5":"grams","6":"crispy colors duo","7":"17"},{"1":"lettuce","2":"reseed","3":"2020-06-06","4":"20","5":"grams","6":"reseed","7":"6"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-20","4":"18","5":"grams","6":"tatsoi","7":"6"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"mustard greens","7":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-22","4":"23","5":"grams","6":"lettuce mixture","7":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"farmer's market blend","7":"21"},{"1":"onions","2":"Delicious Duo","3":"2020-07-16","4":"50","5":"grams","6":"delicious duo","7":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-20","4":"102","5":"grams","6":"long keeping rainbow","7":"20"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-17","4":"8","5":"grams","6":"magnolia blossom","7":"16"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"super sugar snap","7":"16"},{"1":"peppers","2":"green","3":"2020-08-04","4":"81","5":"grams","6":"green","7":"5"},{"1":"potatoes","2":"red","3":"2020-10-15","4":"1718","5":"grams","6":"red","7":"3"},{"1":"potatoes","2":"purple","3":"2020-08-06","4":"317","5":"grams","6":"purple","7":"6"},{"1":"potatoes","2":"yellow","3":"2020-08-06","4":"439","5":"grams","6":"yellow","7":"6"},{"1":"potatoes","2":"Russet","3":"2020-09-16","4":"629","5":"grams","6":"russet","7":"6"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"4758","5":"grams","6":"saved","7":"5"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"new england sugar","7":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"cinderella's carraige","7":"21"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"garden party mix","7":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"883","5":"grams","6":"improved helenor","7":"16"},{"1":"spinach","2":"Catalina","3":"2020-06-11","4":"9","5":"grams","6":"catalina","7":"8"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"307","5":"grams","6":"delicata","7":"8"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1178","5":"grams","6":"red kuri","7":"8"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"3227","5":"grams","6":"blue (saved)","7":"12"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"waltham butternut","7":"17"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"19","5":"grams","6":"neon glow","7":"9"},{"1":"tomatoes","2":"grape","3":"2020-07-11","4":"24","5":"grams","6":"grape","7":"5"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-21","4":"137","5":"grams","6":"big beef","7":"8"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"jet star","7":"8"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-21","4":"339","5":"grams","6":"bonny best","7":"10"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"better boy","7":"10"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"old german","7":"10"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-01","4":"320","5":"grams","6":"brandywine","7":"10"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-01","4":"436","5":"grams","6":"black krim","7":"10"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"volunteers","7":"10"},{"1":"tomatoes","2":"Amish Paste","3":"2020-07-25","4":"463","5":"grams","6":"amish paste","7":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"cherokee purple","7":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"mortgage lifter","7":"15"},{"1":"zucchini","2":"Romanesco","3":"2020-07-06","4":"175","5":"grams","6":"romanesco","7":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>%
  filter(str_detect(variety, "er|ar")) %>%
  distinct(variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"Garden Party Mix"},{"1":"Farmer's Market Blend"},{"1":"Super Sugar Snap"},{"1":"perrenial"},{"1":"asparagus"},{"1":"mustard greens"},{"1":"Bush Bush Slender"},{"1":"Sweet Merlin"},{"1":"variety"},{"1":"Cherokee Purple"},{"1":"Better Boy"},{"1":"Mortgage Lifter"},{"1":"Old German"},{"1":"Jet Star"},{"1":"Bolero"},{"1":"volunteers"},{"1":"Classic Slenderette"},{"1":"Cinderella's Carraige"},{"1":"Waltham Butternut"},{"1":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>



## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## Parsed with column specification:
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>%
  ggplot(aes(x = sdate)) +
  geom_density(fill = "lightblue", color = NA, alpha = .6) +
  labs(x = "",
       y = "",
       title = "Density estimate of bike-renting events' distribution by time")
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
  There is not much information from this graph. In general, the density of events is higher in October than in other months, and decreases sharply starting from late October to late November. This indicates that the frequency of bike-renting events was much higher throughout most of the time in October 2014, and then, by November, people seemed less inclined to rent bikes.  
  The density rebounds in late November and continues increasing until mid-December, which means that bike-renting events happened more and more frequently starting from late November, 2014 until mid-December, 2014, out of unknown reasons. Yet the maximum density here is far less than that in October, and this trend did not last long: People's inclination to rent bikes seemed to decrease again by mid-December and reached its minimum at the end of December.
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2)) %>%
  ggplot(aes(x = time_of_day)) +
  geom_density(fill = "lightblue", color = NA, alpha = 0.6) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  We can observe that the plot is a bit left skewed, with a center at around 12.5 (12:30pm), and is clearly in a bimodal pattern. This means that in late 2014, there were people who rented bikes late at night, and in general, the frequency of renting bikes reaches its highest point either at around 8 (8am) or around 17.5 (17:30), though the events tended to happen more frequently at around 17:30 than at about 8am.  
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>%
  mutate(week_day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(y = fct_rev(fct_infreq(week_day)))) +
  geom_bar(fill = "cadetblue4") +
  labs(x = "",
       y = "",
       title = "Frequency of bike-renting events by weekday")
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  The frequency of the events didn't vary much in different days of the week, according to the plot, but it's still obvious that in the last quarter of 2014, more bike-renting events happened on Friday than all other days, and the total number of times the events happening is the least for Saturday and Sunday.  
  Yet this doesn't mean that people were inclined more to rent a bike on Friday than other weekdays, for each arbitrary week, during that period of time. It's possible that the events of bike-renting happened, with an anomalously high frequency, on one or several Fridays.  
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2),
         week_day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time_of_day)) +
  geom_density(fill = "lightblue", color = NA, alpha = 0.6) +
  facet_wrap(vars(week_day), nrow = 1) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
  Yes, there seems to be a pattern. The density estimates of the distribution of events in weekdays (Monday to Friday) are in a similar bimodal pattern, with one maximum at around 8am and the other at approximately 17:30pm. The density plots for Sunday and Saturday also share a similar pattern: The density (frequency of events) increases until 1am, reaches its lowest level at 5am, then increases drastically and reaches its highest point between 12:30pm and 15:00, and decreases monotonously until 24:00. The pattern shared by weekdays might be attributed to commuting, while it's hard to appropriately elucidate the pattern shared by weekends.
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2),
         week_day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time_of_day, fill = client)) +
  geom_density(alpha = .5, color = NA) +
  facet_wrap(vars(week_day), nrow = 1) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
  
  From this plot, we can see that casual and regular users shared a similar rental behavior pattern during the weekend, but they clearly had different rental behavior patterns during weekdays. In general, registered users followed the bimodal pattern during weekdays, yet casual renters seemed to prefer to rent bikes the most at only around 15:00, except Mondays, when they tended to choose to rent earlier.

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2),
         week_day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time_of_day, fill = client)) +
  geom_density(alpha = .5, 
               color = NA,
               position = position_stack()) +
  facet_wrap(vars(week_day), nrow = 1) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  In terms of displaying rental behavior patterns and comparing them between casual and registered users, this stack position may render it worse in telling stories. Density plots for casual users don't build up from level axes, due to the stack position, and this may lead to the situation that people misinterpret the overall density, which is outlined as the reddish color, as the density for renting events by casual users. For example, it may be the case that people will naturally interpret the renting behaviors, on Monday, of both types of users as fitting in a bimodal pattern. This position visually obscures both the behavior pattern of casual users and the key difference between clients, to some degree.  
  Yet this position makes it easier to have a general concept of proportions. This position allows us to better examine the proportion of each type of users' renting frequency to the total frequency.
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2),
         week_day = wday(sdate),
         weekend = ifelse(near(week_day, 7) | near(week_day, 1), "weekend", "weekday")) %>%
  ggplot(aes(x = time_of_day, fill = client)) +
  geom_density(alpha = .5, 
               color = NA) +
  facet_wrap(vars(weekend), nrow = 1) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day")
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  This graph somehow corroborates interpretations before, showing that casual and registered clients shared a similar renting behavior in weekend and had distinct behaviors in weekdays.
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>%
  mutate(hour = hour(sdate), 
         minute = minute(sdate), 
         time_of_day = round(hour + minute/60, 2),
         week_day = wday(sdate),
         weekend = ifelse(near(week_day, 7) | near(week_day, 1), "weekend", "weekday")) %>%
  ggplot(aes(x = time_of_day, fill = weekend)) +
  geom_density(alpha = .5, 
               color = NA) +
  facet_wrap(vars(client), nrow = 1) +
  labs(x = "",
       y = "",
       title = "Density Estimate of the Events' Distribution by Time of Day") +
  theme(legend.position = "bottom", legend.title = element_blank())
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
  While the previous graph emphasizes the comparison between different kinds of clients' rental behaviors, during weekdays and the weekend, this graph highlights the inner behavioral difference of each type of clients. The story here is that casual users seemed to have similar behaviors regardless of weekday, but regular users had much different behaviors during weekdays from behaviors on the weekend.  
  If we want to focus on the difference between the rental behaviors between two clients' types, than it might be better to consider the previous graph, instead of this one.
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  



**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
