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
 
 The marketing analytics team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. The primary stakeholders for this project include Cyclistic’s director of marketing . The Cyclistic marketing analytics team are secondary stakeholders.


 

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

# prepare
I will be using Cyclistic’s historical bike trip data of 2021.In this data i will be perform  6 month  data from jan to june which are  publicly available [here](https://divvy-tripdata.s3.amazonaws.com/index.html) This data is made available by motivate international inc under this [license](https://ride.divvybikes.com/data-license-agreement)

 In term of  bias and credibility, ROCC process has been used
 
 
 Reliability: Divvy has the rights over the license of this data. Divvy is a program that’s part of the Chicago Department of Transportation (CDOT). The organization owns vehicles, stations, and a wide range of bikes around the city.

Original: The data collected is from Motivate International Inc. They manage the City of Chicago’s Divvy bicycle-sharing program.

Comprehensive: The data includes key information such as ride ID, type of bike, time the user started and ended the ride, start and end time, station ID, longitude and latitude, and finally the type of membership.

Current: The data is up to date to apr 2023.

Cited: The data is available under the current license agreement.

Rstudio  has been used for this phase

These  libraries has been used

1.library(tidyverse)

2 library(Lubridate)

3 library(skimr)

2.library(sqldf)


For importing data from the file

data1<- read.csv("c:/users/Sumit/Desktop/divvy_bike_share/Chicago/202101-divvy-tripdata.csv")

data2<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202102-divvy-tripdata.csv")

data3<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202103-divvy-tripdata.csv")

data4<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202104-divvy-tripdata.csv")

data5<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202105-divvy-tripdata.csv")

data6<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202106-divvy-tripdata.csv")



# Process

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
  
sqldf("select bike_type,  avg(ride_length)as average_length ,member_casual from divvy2

group by bike_type,member_casual ")

               
     bike_type  average_length  member_casual
1  classic_bike      28.465241        casual
2  classic_bike      14.273375        member
3   docked_bike      90.709745        casual
4   docked_bike       2.633333        member
5 electric_bike      21.813951        casual
6 electric_bike      12.972666        member

               
               
  sqldf("select bike_type,  sum(ride_length)as total_length ,member_casual from divvy2 
  
  group by bike_type,member_casual ")
 
 
 
sqldf(" select month ,avg(ride_length)as average_length,member_casual

from divvy2  group by month,member_casual ")




sqldf(" select weekday ,avg(ride_length)as average_length,member_casual

from divvy2  group by weekday ,member_casual  ")


.
  
  
  # Share phase
  Visulaization of data has been prepared in Tableau public
  
  link are [here](https://public.tableau.com/app/profile/sumit.manhas7726/viz/Divvy_bike_share2021/Dashboard1)
  
  
  ![Dashboard 1](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/c14b4589-5ef9-4cc5-862b-0e28220c9d44)




  


























 . 










