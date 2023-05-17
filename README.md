# Cyclistic_bike_share_case_study
This case study is one of the case study of google data analytics course on coursera platform

# Summary 
Following steps are followed of data analytics process for this case study:
1 Ask
2 prepare
3 process
4 Analyze
5 Share
6 Act


# Overview 
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked
 and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

# Ask 
 1. Business statement: 
*  Analyze the Divvy biker's data to identify user habits and trends when using the bike-sharing program with the intention to use that information to guide its new marketing strategy to convert casual riders to  members.
 
2 Stakeholders:
* Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns
and initiatives to promote the bike-share program. These may include email, social media, and other channels.

* Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and
reporting data that helps guide Cyclistic marketing strategy.

3 . Key Question:
* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

# Process
Rstudio  has been used for process phase
These  libraries has been used:
1.library(tidyverse)
2.library(sqldf)

For importing data from the file
data1<- read.csv("c:/users/Sumit/Desktop/divvy_bike_share/Chicago/202101-divvy-tripdata.csv")
data2<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202102-divvy-tripdata.csv")
data3<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202103-divvy-tripdata.csv")
data4<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202104-divvy-tripdata.csv")
data5<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202105-divvy-tripdata.csv")
data6<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202106-divvy-tripdata.csv")

To merge all the files together we need to know that all files has same attributes in same sequence.To get  this information colnames() fxn has been used.
colnames(data1)
colnames(data2)
colnames(data3)
colnames(data4)
colnames(data5)
colnames(data6)
All files  has same  attributes in same sequence 

Now i have merged these all files together
divvy<-bind_rows(data1,data2,data3,data4,data5,data6)

To check that all files together
colnames(divvy)

































 . 










