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

To merge all the files together we need to know that all files has same attributes,same data type in same sequence.
str(data1)
str(data2)
str(data3)
str(data4)
str(data5)
str(data6)
All files  has same  attributes,same data type in same sequence 

Now i have merged these all files together
divvy<-bind_rows(data1,data2,data3,data4,data5,data6)

To check that new files
colnames(divvy)

Now data clean process has been started
There is no  need such type of column (start_lat,start_lng,end_lat,end_lng,start_station_id & end_station_id) in data so i have removed this from the data
divvy<-select(divvy,-start_lat,-start_lng,-end_lat,-end_lng,-start_station_id,-end_station_id)

For understanding data clearly i have renamed one column of rideable_type 
divvy<-rename(divvy,"bike_type"="rideable_type")

In our data  there was only one column of bike_type for data aggregation so few of the column has been added.
divvy$month<-format(as_date(divvy$started_at),"%b")
 
 divvy$day<-format(as_date(divvy$started_at),"%d")
 
 divvy$weekday<-format(as_date(divvy$started_at),"%a")

We have no any numeric data type for which we can summarize our data, so ride length duration  has been extracted from the column of start_at & ended_at 

divvy$ride_length<- difftime(divvy$ended_at,divvy$started_at)
  
 divvy$ride_length<-as.numeric(as.character(divvy$ride_length))

 To see null values and empty values
 skim_without_charts(divvy)
 
 There is no null value has been found but there is a empty values in column of start_station_name and end_station_name ,to remove this
 
divvy<- divvy %>%  filter(start_station_name!='') %>% filter(end_station_name!='')

summary(divvy)

There is some negative values in ride_length column it means ended time is less  than started time so to remove this 

 divvy<- divvy %>% filter(ride_length>0)
 
 Ride_length column time have in second so it has been converted from second to minutes,and also new data frame has been added to avoid from the data loss 
   
  divvy2<- divvy %>% mutate("ride_length"=ride_length/60)
  
  Now our data has been cleaned and well processed for analysis.
  
  # Analysis
  Data frame has been converted to csv files for analysis and analysis has been done in tableau public .
  
  
  # Share phase 
  link are here[https://public.tableau.com/app/profile/sumit.manhas7726/viz/Divvy_bike_share2021/Dashboard1]
  
  
  ![Dashboard 1](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/c14b4589-5ef9-4cc5-862b-0e28220c9d44)




  


























 . 










