---
layout: post
title: United Kingdom's Transportation During Covid-19
categories: R
comments: false
---

In this project I look at the United Kingdom's Department for Transport data. The UK government started producing this data at the start of the Covid-19 National Lockdown.



<p style="text-align:center;"><a href="https://www.gov.uk/government/statistics/transport-use-during-the-coronavirus-covid-19-pandemic">Link To Documentation Page</a></p>


--


Data includes:
1. Road traffic in Great Britain
2. Rail passenger journeys in Great Britain
3. Transport for London (TfL) tube and bus routes
4. Bus travel in Great Britain (excluding London)
5. Cycling in England
<br>

---




- Installing the packages and creating the data frame


``` r
library(tidyverse)
library(gganimate)
library(ggplot2)
library(png)
library(gifski)
library(scales)
library(transformr)
library(readr)

df<- read_csv("~/COVID-19-transport-use-statistics v1.csv")
```


---
- Cleaning up the data by making the percentages into numeric values, using the "parse_number" function

``` r
df$small_trucks<-parse_number(df$`Light Commercial Vehicles2`)
df$big_trucks<-parse_number(df$`Heavy Goods Vehicles2`)
df$all_vehicles<-parse_number(df$`All motor vehicles2`)
df$national_rail<-parse_number(df$`National Rail3,4`)
df$tube<-parse_number(df$`Transport for London Tube5`)
df$bikes<-parse_number(df$`Cycling10,11`)
```


---
 - Formating the date column into the date class


``` r
df$col1<-as.Date(df$col1,"%d/%m/%Y")
```





---
- Creating the plot using the ggplot package

``` r

all_plots<-df %>%
  ggplot(aes(x=as.Date(col1)))+
  labs(x="Time",y="Comparison To Feb 2020 1st Week Average")+
  geom_line(aes(y=cars,color="% of Cars"))+
  geom_line(aes(y=small_trucks,color="% of Small Lorries"))+
  geom_line(aes(y=big_trucks,color="% of Big Lorries"))+
  geom_line(aes(y=all_vehicles,color="% of All Motor Vehicles"))+
  geom_line(aes(y=national_rail,color="% of National Rail"))+
  geom_line(aes(y=tube,color="% of London Tube"))+
  geom_line(aes(y=bikes,color="% of Bikes"))
all_plots
```






![GGplot](\images\uk\static_uk_transport_all_types_rplot.png)








---

- Creating the interactive plotly graph





``` r
inter_all_plots<- ggplotly(all_plots,tooltip = F) %>% config(displayModeBar=F)

saveWidget(inter_all_plots,"all_transportation_plots.html")


```
<iframe src="\images\uk\all_transportation_plots.html" height="800px" width="900" ></iframe>

---




- Creating a basic animation of the % of cars




``` r
# Adding moving averages
df<-df %>%
  mutate(cars_3day = rollmean(all_vehicles,k=3,fill=NA),
        cars_7day = rollmean(all_vehicles,k=7,fill=NA),
        cars_15day = rollmean(all_vehicles,k=15,fill=NA),
        cars_21day = rollmean(all_vehicles,k=21,fill=NA))

# Creating the graph

main_plot<-df %>%
  ggplot(aes(x=as.Date(col1)))+
  labs(title ="% of Motor Vehicles on UK Roads",x="Time",
    y="Comparison to Feb 2020 1st Week Average")+
  geom_line(aes(y=all_vehicles,color="% of Cars"))+
  geom_line(aes(y=cars_3day,color="3 Day Moving Average"))+
  geom_line(aes(y=cars_7day,color="7 Day Moving Average"))+
  geom_line(aes(y=cars_15day,color="15 Day Moving Average"))+
  geom_line(aes(y=cars_21day,color="21 Day Moving Average"))


# Creating the animation

main_animation<-main_plot+
  transition_reveal(col1)+
  view_follow(fixed_x = T,fixed_y=T)


# Chnaging up the speed of the animation

animate(main_animation,
  height=750,
  width=750,
  duration=15,
  end_pause=30)
```

![Animation](\images\uk\UK_transportation_animationv2.gif)<!-- -->


<head><script data-ad-client="ca-pub-5313811741920772" async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script></head>
