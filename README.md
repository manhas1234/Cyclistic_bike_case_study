



# Cyclistic_bike_share_case_study

This case study is one of the case study of google data analytics course on coursera platform

# Summary
Following steps are followed of data analytics process for this case study:

1 Ask 2 prepare 3 process 4 Analyze 5 Share 6 Act

# Scenario
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

The marketing analytics team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. The primary stakeholders for this project include Cyclistic’s director of marketing . The Cyclistic marketing analytics team are secondary stakeholders.

# Ask phase

Business statement:
Analyze the Divvy biker’s data to identify user habits and trends when using the bike-sharing program with the intention to use that information to guide its new marketing strategy to convert casual riders to members.
Stakeholders:
Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.

Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.

3 . Key Question: * How do annual members and casual riders use Cyclistic bikes differently? * How to convert casual riders to annaul members ?

# prepare phase

I will be using Cyclistic’s historical bike trip data of 2021.In this data i will be perform 6 month data from jan to june which are publicly available here This data is made available by motivate international inc under this license

In term of bias and credibility, ROCC process has been used

Reliability: Divvy has the rights over the license of this data. Divvy is a program that’s part of the Chicago Department of Transportation (CDOT). The organization owns vehicles, stations, and a wide range of bikes around the city.

Original: The data collected is from Motivate International Inc. They manage the City of Chicago’s Divvy bicycle-sharing program.

Comprehensive: The data includes key information such as ride ID, type of bike, time the user started and ended the ride, start and end time, station ID, longitude and latitude, and finally the type of membership.

Current: The data is up to date to apr 2023.

Cited: The data is available under the current license agreement.

Rstudio has been used for this phase and these libraries has been used

library(tidyverse)
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.1     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
## ✔ purrr     1.0.1     
## ── Conflicts ──────────────────────────────────
library(lubridate)

library(skimr)

library(ggplot2)

# import data
data1<- read.csv("c:/users/Sumit/Desktop/divvy_bike_share/Chicago/202101-divvy-tripdata.csv")

data2<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202102-divvy-tripdata.csv")

data3<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202103-divvy-tripdata.csv")
data4<- 
read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202104-divvy-tripdata.csv")

data5<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202105-divvy-tripdata.csv")

data6<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202106-divvy-tripdata.csv")
