# Homework 1 Statistical Learning
 title: "PS1_SL"
author: 'Marco Navarro UTIED: mn27484'
date: "2023-01-24"
output: md_document
---

```{r setup, include=FALSE}
setwd("C:/Users/ACER/Desktop/Stats Learning/PS_1")
knitr::opts_chunk$set(echo = TRUE)
library(haven)
library(tidyverse)
library(kableExtra)
library(broom)
library(ggplot2)
```

# 2) Wrangling the Olympics

## A)

```{r A, include =FALSE}
olympics = read.csv('olympics_top20.csv')

athletics_female = olympics %>%
  filter(sex=="F" & sport=="Athletics") 


result1 = as.data.frame(quantile(athletics_female$height, probs = c(.05,.25, .5, .75, .95)))
colnames(result1) = "Percentiles of heights for female competitors across all Athletics events"
```

```{r A2, echo= FALSE}
result1
```

The 95th percentile of heights for female competitors across all Athletics events 183 centimeters.

## B)

```{r B, include =FALSE}

female_olympics= olympics %>%
  filter(sex=="F")

#heightsd = female_olympics %>%
#group_by(event) %>% 
#summarize(Standard_Deviation = sd(height)) 

heightsd = female_olympics %>%
group_by(event) %>% 
summarize(Standard_Deviation = sd(height)) 


heightsd <- heightsd[with(heightsd,order(-Standard_Deviation)),]

heightsd <- heightsd[1:10,]

```


```{r B2, echo= FALSE}
tibble(heightsd)  
```

If we use standard deviation as a measure of variability, Swimming Women's 100 metres Butterfly is the single women's event that had the greatest variability in competitor's heights across the entire history of the Olympics.

## C)

```{r C1, include =FALSE}

swimmers = olympics %>%
  filter(sport=="Swimming") 

averageage = swimmers  %>% 
group_by(sex, year) %>% 
summarize(Average_Age = mean(age))
```


```{r C2, echo=FALSE}
ggplot(averageage) +
  geom_line(aes(x=year,y=Average_Age, color= sex ),linewidth = 1.5) +
  theme(legend.position="bottom")+
          ggtitle("Average age of swimmers by year and sex") +
  theme(plot.title = element_text(color = "black", size = 16, hjust = 0.5))+
   ylim(10, 32)+
  labs(y= "", x = "Year")
```