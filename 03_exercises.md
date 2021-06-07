---
title: 'Weekly Exercises #3'
author: "Rita Liu"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
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
  mutate(weight_lbs =  weight*0.00220462,
         day_week = wday(date, label = TRUE))%>% 
  group_by(vegetable,day_week) %>% 
  summarise(total_weight = sum(weight_lbs)) %>% 
  pivot_wider(names_from = day_week , values_from = total_weight )
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?

```r
garden_planting
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["plot"],"name":[1],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[2],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[3],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[5],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[6],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[7],"type":["chr"],"align":["left"]}],"data":[{"1":"A","2":"peas","3":"Super Sugar Snap","4":"22","5":"2020-04-19","6":"TRUE","7":"NA"},{"1":"B","2":"peas","3":"Magnolia Blossom","4":"24","5":"2020-04-19","6":"TRUE","7":"NA"},{"1":"H","2":"onions","3":"Long Keeping Rainbow","4":"40","5":"2020-04-26","6":"FALSE","7":"NA"},{"1":"P","2":"onions","3":"Delicious Duo","4":"25","5":"2020-04-26","6":"FALSE","7":"NA"},{"1":"C","2":"lettuce","3":"Farmer's Market Blend","4":"60","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"C","2":"radish","3":"Garden Party Mix","4":"20","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"D","2":"potatoes","3":"purple","4":"5","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"D","2":"potatoes","3":"Russet","4":"8","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"I","2":"potatoes","3":"yellow","4":"10","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"P","2":"kale","3":"Heirloom Lacinto","4":"30","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"G","2":"radish","3":"Garden Party Mix","4":"30","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"carrots","3":"Bolero","4":"50","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"carrots","3":"King Midas","4":"50","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"carrots","3":"Dragon","4":"40","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"beets","3":"Gourmet Golden","4":"40","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"beets","3":"Sweet Merlin","4":"40","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"P","2":"lettuce","3":"Tatsoi","4":"25","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"M","2":"Swiss chard","3":"Neon Glow","4":"25","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"H","2":"radish","3":"Garden Party Mix","4":"15","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"O","2":"edamame","3":"edamame","4":"25","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"H","2":"spinach","3":"Catalina","4":"50","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"P","2":"broccoli","3":"Yod Fah","4":"25","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"M","2":"beans","3":"Bush Bush Slender","4":"30","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"L","2":"lettuce","3":"Farmer's Market Blend","4":"60","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"A","2":"squash","3":"Butternut (saved)","4":"8","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"A","2":"squash","3":"Red Kuri","4":"4","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"A","2":"squash","3":"Blue (saved)","4":"4","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"A","2":"squash","3":"Waltham Butternut","4":"4","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"A","2":"pumpkins","3":"Cinderalla's Carraige","4":"3","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"B","2":"squash","3":"Red Kuri","4":"4","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"B","2":"squash","3":"Blue (saved)","4":"8","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"B","2":"pumpkins","3":"Cinderella's Carraige","4":"3","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"B","2":"pumpkins","3":"saved","4":"8","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Mortgage Lifter","4":"1","5":"2020-05-20","6":"TRUE","7":"died"},{"1":"J","2":"tomatoes","3":"Old German","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Better Boy","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Amish Paste","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Cherokee Purple","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Brandywine","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"Bonny Best","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Amish Paste","4":"2","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Big Beef","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Mortgage Lifter","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Black Krim","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Jet Star","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"N","2":"tomatoes","3":"Better Boy","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"O","2":"tomatoes","3":"grape","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"K","2":"peppers","3":"green","4":"12","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"O","2":"peppers","3":"green","4":"5","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potA","2":"peppers","3":"variety","4":"3","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potA","2":"peppers","3":"variety","4":"3","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potB","2":"hot peppers","3":"thai","4":"1","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potC","2":"hot peppers","3":"variety","4":"6","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potD","2":"peppers","3":"variety","4":"1","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"potB","2":"basil","3":"Isle of Naxos","4":"40","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"wagon","2":"dill","3":"Grandma Einck's","4":"40","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"potD","2":"cilantro","3":"cilantro","4":"15","5":"2020-05-16","6":"FALSE","7":"NA"},{"1":"D","2":"beans","3":"Bush Bush Slender","4":"10","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"D","2":"zucchini","3":"Romanesco","4":"3","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"D","2":"brussels sprouts","3":"Long Island","4":"13","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"L","2":"jalapeño","3":"giant","4":"4","5":"2020-05-21","6":"TRUE","7":"NA"},{"1":"D","2":"broccoli","3":"Main Crop Bravado","4":"7","5":"2020-05-22","6":"TRUE","7":"NA"},{"1":"I","2":"broccoli","3":"Main Crop Bravado","4":"7","5":"2020-05-22","6":"TRUE","7":"NA"},{"1":"I","2":"potatoes","3":"yellow","4":"8","5":"2020-05-22","6":"TRUE","7":"NA"},{"1":"I","2":"potatoes","3":"red","4":"3","5":"2020-05-22","6":"FALSE","7":"NA"},{"1":"side","2":"pumpkins","3":"Big Max","4":"6","5":"2020-05-24","6":"TRUE","7":"NA"},{"1":"side","2":"pumpkins","3":"Cinderalla's Carraige","4":"6","5":"2020-05-24","6":"TRUE","7":"NA"},{"1":"side","2":"watermelon","3":"Doll Babies","4":"8","5":"2020-05-24","6":"TRUE","7":"NA"},{"1":"side","2":"melon","3":"honeydew","4":"5","5":"2020-05-24","6":"TRUE","7":"NA"},{"1":"K","2":"squash","3":"Waltham Butternut","4":"6","5":"2020-05-25","6":"TRUE","7":"NA"},{"1":"K","2":"squash","3":"delicata","4":"8","5":"2020-05-25","6":"TRUE","7":"NA"},{"1":"K","2":"pumpkins","3":"New England Sugar","4":"4","5":"2020-05-25","6":"TRUE","7":"NA"},{"1":"K","2":"beans","3":"Chinese Red Noodle","4":"5","5":"2020-05-25","6":"TRUE","7":"NA"},{"1":"L","2":"beans","3":"Chinese Red Noodle","4":"5","5":"2020-05-25","6":"TRUE","7":"NA"},{"1":"L","2":"carrots","3":"Bolero","4":"50","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"L","2":"carrots","3":"King Midas","4":"50","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"L","2":"carrots","3":"Dragon","4":"50","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"E","2":"rudabaga","3":"Improved Helenor","4":"30","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"L","2":"cucumbers","3":"pickling","4":"20","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"F","2":"strawberries","3":"perennial","4":"NA","5":"<NA>","6":"NA","7":"NA"},{"1":"N","2":"tomatoes","3":"volunteers","4":"1","5":"2020-06-03","6":"TRUE","7":"NA"},{"1":"J","2":"tomatoes","3":"volunteers","4":"1","5":"2020-06-03","6":"TRUE","7":"NA"},{"1":"side","2":"squash","3":"Red Kuri","4":"1","5":"2020-05-20","6":"TRUE","7":"NA"},{"1":"front","2":"tomatoes","3":"volunteers","4":"5","5":"2020-06-03","6":"TRUE","7":"NA"},{"1":"O","2":"tomatoes","3":"volunteers","4":"2","5":"2020-06-03","6":"TRUE","7":"NA"},{"1":"A","2":"corn","3":"Dorinny Sweet","4":"20","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"B","2":"corn","3":"Golden Bantam","4":"20","5":"2020-05-25","6":"FALSE","7":"NA"},{"1":"front","2":"kohlrabi","3":"Crispy Colors Duo","4":"10","5":"2020-05-20","6":"FALSE","7":"NA"},{"1":"E","2":"spinach","3":"Catalina","4":"100","5":"2020-06-20","6":"FALSE","7":"NA"},{"1":"E","2":"beans","3":"Classic Slenderette","4":"29","5":"2020-06-20","6":"TRUE","7":"NA"},{"1":"G","2":"lettuce","3":"Lettuce Mixture","4":"200","5":"2020-06-20","6":"FALSE","7":"NA"},{"1":"E","2":"cilantro","3":"cilantro","4":"20","5":"2020-06-20","6":"FALSE","7":"NA"},{"1":"front","2":"kale","3":"Heirloom Lacinto","4":"30","5":"2020-06-20","6":"FALSE","7":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  


```r
garden_harvest %>% 
  mutate(weight_lbs =  weight*0.00220462) %>% 
  group_by(vegetable,variety) %>% 
  summarise(total_weight = sum(weight_lbs)) %>% 
  left_join(garden_planting,
            by =  c('vegetable','variety'))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_weight"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"K","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"O","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

> The problem is that the same vegetable with the same variety was planted in different plots. 


  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.

> We could, first of all, calculate the average price of each variety of each vegetable from all stores. Then, we could use left join, joining on vegetable and variety, creating a new variable called money_saved by using the weight time the average price calculated before. 

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(variety = fct_reorder(variety, date),
         weight_lbs =  weight*0.00220462) %>% 
  group_by(variety) %>% 
  summarise(total_weight = sum(weight_lbs)) %>% 
  ggplot(aes(x = total_weight, y = variety)) + 
  geom_col() + 
  labs(x = 'total weight of tomatoes (lbs)')
```

![](03_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(lower_variety = str_to_lower(variety),
         length_variety = str_length(variety)) %>% 
  arrange(lower_variety,length_variety) %>% 
  distinct(vegetable,variety,.keep_all = TRUE)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["lower_variety"],"name":[6],"type":["chr"],"align":["left"]},{"label":["length_variety"],"name":[7],"type":["int"],"align":["right"]}],"data":[{"1":"tomatoes","2":"Amish Paste","3":"2020-07-25","4":"463","5":"grams","6":"amish paste","7":"11"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"asparagus","7":"9"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"better boy","7":"10"},{"1":"tomatoes","2":"Big Beef","3":"2020-07-21","4":"137","5":"grams","6":"big beef","7":"8"},{"1":"tomatoes","2":"Black Krim","3":"2020-08-01","4":"436","5":"grams","6":"black krim","7":"10"},{"1":"squash","2":"Blue (saved)","3":"2020-09-01","4":"3227","5":"grams","6":"blue (saved)","7":"12"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"bolero","7":"6"},{"1":"tomatoes","2":"Bonny Best","3":"2020-07-21","4":"339","5":"grams","6":"bonny best","7":"10"},{"1":"tomatoes","2":"Brandywine","3":"2020-08-01","4":"320","5":"grams","6":"brandywine","7":"10"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"bush bush slender","7":"17"},{"1":"spinach","2":"Catalina","3":"2020-06-11","4":"9","5":"grams","6":"catalina","7":"8"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"cherokee purple","7":"15"},{"1":"beans","2":"Chinese Red Noodle","3":"2020-08-08","4":"108","5":"grams","6":"chinese red noodle","7":"18"},{"1":"cilantro","2":"cilantro","3":"2020-06-23","4":"2","5":"grams","6":"cilantro","7":"8"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"cinderella's carraige","7":"21"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"classic slenderette","7":"19"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"2020-09-17","4":"191","5":"grams","6":"crispy colors duo","7":"17"},{"1":"squash","2":"delicata","3":"2020-09-19","4":"307","5":"grams","6":"delicata","7":"8"},{"1":"onions","2":"Delicious Duo","3":"2020-07-16","4":"50","5":"grams","6":"delicious duo","7":"13"},{"1":"corn","2":"Dorinny Sweet","3":"2020-08-11","4":"330","5":"grams","6":"dorinny sweet","7":"13"},{"1":"carrots","2":"Dragon","3":"2020-07-24","4":"80","5":"grams","6":"dragon","7":"6"},{"1":"edamame","2":"edamame","3":"2020-08-11","4":"109","5":"grams","6":"edamame","7":"7"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"farmer's market blend","7":"21"},{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"garden party mix","7":"16"},{"1":"jalapeño","2":"giant","3":"2020-07-17","4":"20","5":"grams","6":"giant","7":"5"},{"1":"corn","2":"Golden Bantam","3":"2020-08-15","4":"383","5":"grams","6":"golden bantam","7":"13"},{"1":"beets","2":"Gourmet Golden","3":"2020-07-07","4":"62","5":"grams","6":"gourmet golden","7":"14"},{"1":"tomatoes","2":"grape","3":"2020-07-11","4":"24","5":"grams","6":"grape","7":"5"},{"1":"peppers","2":"green","3":"2020-08-04","4":"81","5":"grams","6":"green","7":"5"},{"1":"carrots","2":"greens","3":"2020-08-29","4":"169","5":"grams","6":"greens","7":"6"},{"1":"kale","2":"Heirloom Lacinto","3":"2020-06-13","4":"10","5":"grams","6":"heirloom lacinto","7":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"2020-10-16","4":"883","5":"grams","6":"improved helenor","7":"16"},{"1":"basil","2":"Isle of Naxos","3":"2020-06-23","4":"5","5":"grams","6":"isle of naxos","7":"13"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"jet star","7":"8"},{"1":"carrots","2":"King Midas","3":"2020-07-23","4":"56","5":"grams","6":"king midas","7":"10"},{"1":"beets","2":"leaves","3":"2020-06-11","4":"8","5":"grams","6":"leaves","7":"6"},{"1":"lettuce","2":"Lettuce Mixture","3":"2020-07-22","4":"23","5":"grams","6":"lettuce mixture","7":"15"},{"1":"onions","2":"Long Keeping Rainbow","3":"2020-07-20","4":"102","5":"grams","6":"long keeping rainbow","7":"20"},{"1":"peas","2":"Magnolia Blossom","3":"2020-06-17","4":"8","5":"grams","6":"magnolia blossom","7":"16"},{"1":"broccoli","2":"Main Crop Bravado","3":"2020-09-09","4":"102","5":"grams","6":"main crop bravado","7":"17"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"mortgage lifter","7":"15"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"mustard greens","7":"14"},{"1":"Swiss chard","2":"Neon Glow","3":"2020-06-21","4":"19","5":"grams","6":"neon glow","7":"9"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"new england sugar","7":"17"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"old german","7":"10"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"perrenial","7":"9"},{"1":"strawberries","2":"perrenial","3":"2020-06-18","4":"40","5":"grams","6":"perrenial","7":"9"},{"1":"raspberries","2":"perrenial","3":"2020-06-29","4":"30","5":"grams","6":"perrenial","7":"9"},{"1":"cucumbers","2":"pickling","3":"2020-07-08","4":"181","5":"grams","6":"pickling","7":"8"},{"1":"potatoes","2":"purple","3":"2020-08-06","4":"317","5":"grams","6":"purple","7":"6"},{"1":"potatoes","2":"red","3":"2020-10-15","4":"1718","5":"grams","6":"red","7":"3"},{"1":"squash","2":"Red Kuri","3":"2020-09-19","4":"1178","5":"grams","6":"red kuri","7":"8"},{"1":"lettuce","2":"reseed","3":"2020-06-06","4":"20","5":"grams","6":"reseed","7":"6"},{"1":"zucchini","2":"Romanesco","3":"2020-07-06","4":"175","5":"grams","6":"romanesco","7":"9"},{"1":"potatoes","2":"Russet","3":"2020-09-16","4":"629","5":"grams","6":"russet","7":"6"},{"1":"pumpkins","2":"saved","3":"2020-09-01","4":"4758","5":"grams","6":"saved","7":"5"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"super sugar snap","7":"16"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"sweet merlin","7":"12"},{"1":"lettuce","2":"Tatsoi","3":"2020-06-20","4":"18","5":"grams","6":"tatsoi","7":"6"},{"1":"hot peppers","2":"thai","3":"2020-07-20","4":"12","5":"grams","6":"thai","7":"4"},{"1":"apple","2":"unknown","3":"2020-09-26","4":"156","5":"grams","6":"unknown","7":"7"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"variety","7":"7"},{"1":"peppers","2":"variety","3":"2020-07-24","4":"68","5":"grams","6":"variety","7":"7"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"volunteers","7":"10"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"waltham butternut","7":"17"},{"1":"potatoes","2":"yellow","3":"2020-08-06","4":"439","5":"grams","6":"yellow","7":"6"},{"1":"broccoli","2":"Yod Fah","3":"2020-07-27","4":"372","5":"grams","6":"yod fah","7":"7"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(special_char = (str_detect(variety, 'er') | str_detect(variety, 'ar'))) %>% 
  filter(special_char == TRUE) %>% 
  distinct(vegetable,variety,.keep_all = TRUE)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["date"],"align":["right"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]},{"label":["special_char"],"name":[6],"type":["lgl"],"align":["right"]}],"data":[{"1":"radish","2":"Garden Party Mix","3":"2020-06-06","4":"36","5":"grams","6":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"2020-06-11","4":"12","5":"grams","6":"TRUE"},{"1":"peas","2":"Super Sugar Snap","3":"2020-06-17","4":"121","5":"grams","6":"TRUE"},{"1":"chives","2":"perrenial","3":"2020-06-17","4":"8","5":"grams","6":"TRUE"},{"1":"strawberries","2":"perrenial","3":"2020-06-18","4":"40","5":"grams","6":"TRUE"},{"1":"asparagus","2":"asparagus","3":"2020-06-20","4":"20","5":"grams","6":"TRUE"},{"1":"lettuce","2":"mustard greens","3":"2020-06-29","4":"23","5":"grams","6":"TRUE"},{"1":"raspberries","2":"perrenial","3":"2020-06-29","4":"30","5":"grams","6":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"2020-07-06","4":"235","5":"grams","6":"TRUE"},{"1":"beets","2":"Sweet Merlin","3":"2020-07-07","4":"10","5":"grams","6":"TRUE"},{"1":"hot peppers","2":"variety","3":"2020-07-20","4":"559","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Cherokee Purple","3":"2020-07-24","4":"247","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"2020-07-24","4":"220","5":"grams","6":"TRUE"},{"1":"peppers","2":"variety","3":"2020-07-24","4":"68","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"2020-07-27","4":"801","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Old German","3":"2020-07-28","4":"611","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"2020-07-28","4":"315","5":"grams","6":"TRUE"},{"1":"carrots","2":"Bolero","3":"2020-07-30","4":"116","5":"grams","6":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"2020-08-04","4":"73","5":"grams","6":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"2020-08-05","4":"41","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"2020-09-01","4":"7350","5":"grams","6":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"2020-09-19","4":"1834","5":"grams","6":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"2020-09-19","4":"1109","5":"grams","6":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
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

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(sdate)) + 
  geom_density() + 
  labs(x = 'start date')
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>%
  mutate(sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60) %>% 
  ggplot(aes(sd_time)) + 
  geom_density() + 
  labs(x = 'Time of start date')
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE)) %>% 
  ggplot(aes(y = sd_week)) +
  geom_bar() + 
  labs(y =  'Week', x = 'Counts of trips')
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE),
         sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60) %>% 
  ggplot(aes(sd_time)) + 
  geom_density() +
  facet_wrap(vars(sd_week))
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
  > Yes. Except for weekends, there are two peaks for renting bikes, one in teh morning and the other in the afternoon.   
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE),
         sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60) %>% 
  ggplot(aes(x = sd_time,fill = client)) + 
  geom_density(alpha = 0.5, color = NA) +
  facet_wrap(vars(sd_week)) + 
  labs(x = 'time of start date')
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE),
         sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60) %>% 
  ggplot(aes(x = sd_time,fill = client)) + 
  geom_density(alpha = 0.5, color = NA,position = position_stack()) +
  facet_wrap(vars(sd_week))
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  > I think it's better to tell a story. The advantages of using `position = position_stack()` is that we can see each density curve more clearly and the two curves are comparable(??)   
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE),
         sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60,
         weekend = ifelse(sd_week %in% c('Sun','Sat'),1,0)) %>% 
  ggplot(aes(x = sd_time,fill = client)) + 
  geom_density(alpha = 0.5, color = NA,position = position_stack()) +
  facet_wrap(vars(weekend))
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>%
  mutate(sd_week = wday(sdate,label = TRUE),
         sd_hour = hour(sdate),
         sd_minute = minute(sdate),
         sd_time = sd_hour + sd_minute/60,
         weekend = ifelse(sd_week %in% c('Sun','Sat'),1,0)) %>% 
  ggplot(aes(x = sd_time,fill = factor(weekend))) + 
  geom_density(alpha = 0.5, color = NA,position = position_stack()) +
  facet_wrap(vars(client))
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

> Whether it is weekend seems to affect the bike renting pattern of registered users. 
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!

  
  

```r
Trips %>% 
  left_join(Stations,by = c('sstation' = 'name'), keep = TRUE) %>% 
  group_by(sstation) %>% 
  summarise(total_dep = n(),
            lat = mean(lat),
            long = mean(long)) %>% 
  ggplot(aes(x = lat, y = long,size = total_dep)) + 
  geom_point()
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Trips %>% 
  group_by(sstation) %>% 
  mutate(all = n()) %>% 
  filter(client == 'Casual') %>% 
  summarise(casual_sum = n(),
            all = mean(all)) %>% 
  mutate(per_casual = casual_sum/all) %>% 
  ggplot(aes(x = per_casual, y = fct_reorder(sstation,per_casual))) +
  geom_col()
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->
  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

```r
stat_date_comb <- Trips %>% 
  mutate(sdate = as_date(sdate)) %>% 
  group_by(sstation,sdate) %>% 
  summarise(total_dep = n()) %>% 
  arrange(desc(total_dep)) %>% 
  head(10)
stat_date_comb
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sstation"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sdate"],"name":[2],"type":["date"],"align":["right"]},{"label":["total_dep"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7"},{"1":"14th & V St NW","2":"2014-11-07","3":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

```r
date_trip <- Trips %>% 
  mutate(sdate = as_date(sdate))
stat_date_comb %>% 
  left_join(date_trip,by = c('sstation','sdate'))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sstation"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sdate"],"name":[2],"type":["date"],"align":["right"]},{"label":["total_dep"],"name":[3],"type":["int"],"align":["right"]},{"label":["duration"],"name":[4],"type":["chr"],"align":["left"]},{"label":["edate"],"name":[5],"type":["dttm"],"align":["right"]},{"label":["estation"],"name":[6],"type":["chr"],"align":["left"]},{"label":["bikeno"],"name":[7],"type":["chr"],"align":["left"]},{"label":["client"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 13m 0s","5":"2014-11-12 06:21:00","6":"Maryland & Independence Ave SW","7":"W00733","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 11m 48s","5":"2014-11-12 07:54:00","6":"L'Enfant Plaza / 7th & C St SW","7":"W20307","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 9m 45s","5":"2014-11-12 08:28:00","6":"Potomac Ave & 8th St SE","7":"W01369","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 10m 17s","5":"2014-11-12 10:12:00","6":"11th & F St NW","7":"W00766","8":"Casual"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 9m 45s","5":"2014-11-12 20:17:00","6":"13th & H St NE","7":"W20481","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 6m 28s","5":"2014-11-12 15:08:00","6":"11th & H St NE","7":"W01357","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 11m 33s","5":"2014-11-12 18:18:00","6":"3rd & G St SE","7":"W21695","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 6m 54s","5":"2014-11-12 17:42:00","6":"11th & H St NE","7":"W00229","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 23m 6s","5":"2014-11-12 14:58:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W21407","8":"Casual"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 2m 3s","5":"2014-11-12 18:22:00","6":"3rd & H St NE","7":"W20792","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-11-12","3":"11","4":"0h 10m 43s","5":"2014-11-12 14:47:00","6":"Eastern Market Metro / Pennsylvania Ave & 7th St SE","7":"W01158","8":"Registered"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 13m 36s","5":"2014-12-27 13:56:00","6":"Jefferson Memorial","7":"W00924","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 57m 4s","5":"2014-12-27 10:44:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W01059","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 53m 48s","5":"2014-12-27 10:44:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W00653","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 19m 21s","5":"2014-12-27 11:35:00","6":"Maryland & Independence Ave SW","7":"W20232","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 14m 5s","5":"2014-12-27 16:06:00","6":"Maryland & Independence Ave SW","7":"W00022","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 38m 22s","5":"2014-12-27 16:28:00","6":"Jefferson Memorial","7":"W21946","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 9m 21s","5":"2014-12-27 16:13:00","6":"Washington & Independence Ave SW/HHS","7":"W20584","8":"Registered"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"1h 22m 31s","5":"2014-12-27 14:20:00","6":"New York Ave & 15th St NW","7":"W21494","8":"Casual"},{"1":"Jefferson Dr & 14th St SW","2":"2014-12-27","3":"9","4":"0h 48m 48s","5":"2014-12-27 14:40:00","6":"19th St & Constitution Ave NW","7":"W21453","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"2h 10m 20s","5":"2014-10-05 14:45:00","6":"Ohio Dr & West Basin Dr SW / MLK & FDR Memorials","7":"W01221","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 30m 6s","5":"2014-10-05 12:28:00","6":"Jefferson Memorial","7":"W20256","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 20m 5s","5":"2014-10-05 18:50:00","6":"14th St & New York Ave NW","7":"W20890","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 36m 38s","5":"2014-10-05 11:48:00","6":"Maryland & Independence Ave SW","7":"W21644","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 14m 35s","5":"2014-10-05 20:04:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W20974","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 25m 37s","5":"2014-10-05 12:19:00","6":"Iwo Jima Memorial/N Meade & 14th St N","7":"W21089","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 21m 16s","5":"2014-10-05 12:23:00","6":"Lincoln Memorial","7":"W01177","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 41m 18s","5":"2014-10-05 21:35:00","6":"New York Ave & 15th St NW","7":"W20605","8":"Registered"},{"1":"Lincoln Memorial","2":"2014-10-05","3":"9","4":"0h 49m 1s","5":"2014-10-05 14:51:00","6":"Maryland & Independence Ave SW","7":"W20927","8":"Registered"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 7m 57s","5":"2014-10-09 11:42:00","6":"21st St & Constitution Ave NW","7":"W20032","8":"Registered"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 13m 38s","5":"2014-10-09 22:21:00","6":"Jefferson Memorial","7":"W20283","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 17m 45s","5":"2014-10-09 14:01:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W01464","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 11m 54s","5":"2014-10-09 13:59:00","6":"Smithsonian / Jefferson Dr & 12th St SW","7":"W01384","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"1h 32m 53s","5":"2014-10-09 18:24:00","6":"14th & D St NW / Ronald Reagan Building","7":"W20284","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 27m 4s","5":"2014-10-09 08:15:00","6":"8th & Eye St SE / Barracks Row","7":"W21619","8":"Registered"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"0h 23m 8s","5":"2014-10-09 12:40:00","6":"Jefferson Dr & 14th St SW","7":"W00851","8":"Casual"},{"1":"Lincoln Memorial","2":"2014-10-09","3":"8","4":"1h 20m 27s","5":"2014-10-09 16:37:00","6":"Lincoln Memorial","7":"W00006","8":"Casual"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 3m 41s","5":"2014-10-06 12:49:00","6":"Massachusetts Ave & Dupont Circle NW","7":"W01470","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 11m 5s","5":"2014-10-06 21:44:00","6":"10th & U St NW","7":"W20675","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 12m 26s","5":"2014-10-06 19:25:00","6":"New Jersey Ave & N St NW/Dunbar HS","7":"W20069","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 16m 10s","5":"2014-10-06 09:03:00","6":"25th St & Pennsylvania Ave NW","7":"W20141","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 6m 41s","5":"2014-10-06 08:06:00","6":"14th & R St NW","7":"W00684","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 3m 9s","5":"2014-10-06 19:26:00","6":"15th & P St NW","7":"W00409","8":"Registered"},{"1":"17th St & Massachusetts Ave NW","2":"2014-10-06","3":"7","4":"0h 3m 57s","5":"2014-10-06 18:34:00","6":"15th & P St NW","7":"W21041","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 12m 43s","5":"2014-10-02 08:36:00","6":"14th St & New York Ave NW","7":"W20228","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 23m 24s","5":"2014-10-02 09:42:00","6":"17th & K St NW / Farragut Square","7":"W00281","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 8m 8s","5":"2014-10-02 06:16:00","6":"8th & D St NW","7":"W21071","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 3m 33s","5":"2014-10-02 09:22:00","6":"Constitution Ave & 2nd St NW/DOL","7":"W00490","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 18m 14s","5":"2014-10-02 14:27:00","6":"New York Ave & 15th St NW","7":"W00759","8":"Casual"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 8m 49s","5":"2014-10-02 17:32:00","6":"3rd St & Pennsylvania Ave SE","7":"W21214","8":"Registered"},{"1":"Columbus Circle / Union Station","2":"2014-10-02","3":"7","4":"0h 13m 19s","5":"2014-10-02 17:46:00","6":"14th & D St SE","7":"W21573","8":"Registered"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 24m 26s","5":"2014-10-25 18:26:00","6":"Georgetown Harbor / 30th St NW","7":"W21957","8":"Casual"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 17m 49s","5":"2014-10-25 16:36:00","6":"Harvard St & Adams Mill Rd NW","7":"W00842","8":"Registered"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 17m 0s","5":"2014-10-25 16:36:00","6":"Harvard St & Adams Mill Rd NW","7":"W20600","8":"Casual"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 28m 33s","5":"2014-10-25 17:44:00","6":"Lincoln Memorial","7":"W21439","8":"Casual"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 25m 15s","5":"2014-10-25 14:08:00","6":"Constitution Ave & 2nd St NW/DOL","7":"W21629","8":"Casual"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 5m 33s","5":"2014-10-25 11:17:00","6":"34th & Water St NW","7":"W20311","8":"Casual"},{"1":"Georgetown Harbor / 30th St NW","2":"2014-10-25","3":"7","4":"0h 5m 53s","5":"2014-10-25 18:19:00","6":"New Hampshire Ave & 24th St NW","7":"W21709","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 7m 41s","5":"2014-10-01 22:09:00","6":"14th & Belmont St NW","7":"W20200","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 6m 31s","5":"2014-10-01 08:56:00","6":"New Hampshire Ave & 24th St NW","7":"W21010","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 20m 47s","5":"2014-10-01 23:07:00","6":"North Capitol St & F St NW","7":"W21122","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 7m 51s","5":"2014-10-01 18:29:00","6":"Calvert St & Woodley Pl NW","7":"W21466","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 1m 48s","5":"2014-10-01 19:29:00","6":"21st & M St NW","7":"W00928","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 19m 16s","5":"2014-10-01 21:02:00","6":"Calvert St & Woodley Pl NW","7":"W20884","8":"Registered"},{"1":"Massachusetts Ave & Dupont Circle NW","2":"2014-10-01","3":"7","4":"0h 11m 31s","5":"2014-10-01 13:25:00","6":"37th & O St NW / Georgetown University","7":"W21080","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 20m 32s","5":"2014-10-16 07:25:00","6":"North Capitol St & G Pl NE","7":"W21470","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 11m 15s","5":"2014-10-16 09:16:00","6":"17th & G St NW","7":"W20852","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 8m 44s","5":"2014-10-16 08:28:00","6":"19th & K St NW","7":"W20787","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 9m 10s","5":"2014-10-16 11:55:00","6":"19th St & Pennsylvania Ave NW","7":"W01078","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 3m 58s","5":"2014-10-16 08:35:00","6":"Massachusetts Ave & Dupont Circle NW","7":"W21089","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 8m 8s","5":"2014-10-16 16:37:00","6":"14th & Harvard St NW","7":"W21623","8":"Registered"},{"1":"New Hampshire Ave & T St NW","2":"2014-10-16","3":"7","4":"0h 2m 49s","5":"2014-10-16 09:19:00","6":"Massachusetts Ave & Dupont Circle NW","7":"W00066","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 9m 23s","5":"2014-11-07 20:03:00","6":"18th & M St NW","7":"W21318","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 8m 52s","5":"2014-11-07 17:50:00","6":"17th St & Massachusetts Ave NW","7":"W00595","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 10m 52s","5":"2014-11-07 07:49:00","6":"New York Ave & 15th St NW","7":"W01452","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 7m 41s","5":"2014-11-07 16:49:00","6":"Massachusetts Ave & Dupont Circle NW","7":"W20094","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 14m 48s","5":"2014-11-07 08:14:00","6":"17th & G St NW","7":"W01215","8":"Registered"},{"1":"14th & V St NW","2":"2014-11-07","3":"6","4":"0h 10m 33s","5":"2014-11-07 14:32:00","6":"8th & H St NW","7":"W21189","8":"Registered"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

```r
stat_date_comb %>% 
  left_join(date_trip,by = c('sstation','sdate')) %>% 
  mutate(week = wday(sdate,label = TRUE)) %>% 
  group_by(client,week) %>% 
  mutate(week_dep = n()) %>% 
  group_by(client) %>% 
  mutate(client_dep = n()) %>% 
  mutate(client_prop = week_dep/client_dep) %>% 
  group_by(week,client) %>% 
  summarise(client_prop = mean(client_prop)) %>% 
  pivot_wider(names_from = client,values_from = client_prop)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["week"],"name":[1],"type":["ord"],"align":["right"]},{"label":["Casual"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Registered"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"Sun","2":"0.24137931","3":"0.04081633"},{"1":"Mon","2":"NA","3":"0.14285714"},{"1":"Wed","2":"0.06896552","3":"0.32653061"},{"1":"Thu","2":"0.24137931","3":"0.30612245"},{"1":"Fri","2":"NA","3":"0.12244898"},{"1":"Sat","2":"0.44827586","3":"0.06122449"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.
 http://github.com/RitaLiu6/Assignment3  
 
## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
