

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


# Scenario
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked
 and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.
 
 The marketing analytics team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. The primary stakeholders for this project include Cyclistic’s director of marketing . The Cyclistic marketing analytics team are secondary stakeholders.



# 1. Ask phase

 1. Business statement: 
*  Analyze the Divvy biker's data to identify user habits and trends when using the bike-sharing program with the intention to use that information to guide its new marketing strategy to convert casual riders to  members.
 
2  Stakeholders:
* Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns
and initiatives to promote the bike-share program. These may include email, social media, and other channels.

* Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and
reporting data that helps guide Cyclistic marketing strategy.

3 . Key Question:
* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?



# 2. prepare phase

I will be using Cyclistic’s historical bike trip data of 2021.In this data i will be perform  6 month  data from jan to june which are  publicly available
[here](https://divvy-tripdata.s3.amazonaws.com/index.html) This data is made available by motivate international inc under this [license](https://ride.divvybikes.com/data-license-agreement)


 In term of  bias and credibility, ROCC process has been used
 
 
 Reliability: Divvy has the rights over the license of this data. Divvy is a program that’s part of the Chicago Department of Transportation (CDOT). The organization owns vehicles, stations, and a wide range of bikes around the city.
 

Original: The data collected is from Motivate International Inc. They manage the City of Chicago’s Divvy bicycle-sharing program.


Comprehensive: The data includes key information such as ride ID, type of bike, time the user started and ended the ride, start and end time, station ID, longitude and latitude, and finally the type of membership.

Current: The data is up to date to apr 2023.


Cited: The data is available under the current license agreement.



Rstudio  has been used for this phase  and these  libraries has been used

```{r}
library(tidyverse)
library(lubridate)
library(skimr)
library(sqldf)
```

import data 
```{r}
data1<- read.csv("c:/users/Sumit/Desktop/divvy_bike_share/Chicago/202101-divvy-tripdata.csv")
data2<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202102-divvy-tripdata.csv")
data3<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202103-divvy-tripdata.csv")
data4<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202104-divvy-tripdata.csv")
data5<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202105-divvy-tripdata.csv")
data6<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202106-divvy-tripdata.csv")

```

# 3. Process phase

 To merge all the files together we need to know that all files has same attributes,same data type in same sequence.

```{r}
str(data1)
str(data2)
str(data3)
str(data4)
str(data5)
str(data6)

```


All files  has same  attributes,same data type in same sequence and Now i have merged these all files together
```{r}
divvy<-bind_rows(data1,data2,data3,data4,data5,data6)
```


 To see the structure of data
```{r}
str(divvy)
```
 Head of the data
```{r}
head(divvy)
```
Tail of data
```{r}
tail(divvy)
```
To check column the column of the data 
```{r}
colnames(divvy)
```
 Now data clean process has been started


There is no  need such type of column (start_lat,start_lng,end_lat,end_lng,start_station_id & end_station_id) in data so i have removed this from the data

```{r}
divvy<-select(divvy,-start_lat,-start_lng,-end_lat,-end_lng,-start_station_id,-end_station_id)
```

 Check the column name again

```{r}
colnames(divvy)
```
 For understanding data clearly i have renamed one column of rideable_type

```{r}
divvy<-rename(divvy,"bike_type"="rideable_type")

```


In our data  there was only one column of bike_type for data aggregation so few of the column has been added.

```{r}
 divvy$month<-format(as_date(divvy$started_at),"%b")
 
 
 divvy$day<-format(as_date(divvy$started_at),"%d")
 

 divvy$weekday<-format(as_date(divvy$started_at),"%a")

```


```{r}
str(divvy)
```
 We have no any numeric data type for which we can summarize our data, so ride length duration  has been extracted from the column of start_at & ended_at 

```{r}
 divvy$ride_length<- difftime(divvy$ended_at,divvy$started_at)
  
  divvy$ride_length<-as.numeric(as.character(divvy$ride_length))
  
```


```{r}
summary(divvy)
```
There is some negative values in ride_length column it means ended time is less  than started time and also  Ride_length column duration time  have in second so it has been converted from second to minutes,and also new data frame has been added to avoid from the data loss 

```{r}

  divvy<- divvy %>% filter(ride_length>0)
  
  divvy<- divvy %>% mutate("ride_length"=ride_length/60)
```

 To check null ,duplicates and empty values in the data 
```{r}
skim_without_charts(divvy)
```


There is a empty values in column of start_station_name and end_station_name ,to remove this
 
```{r}
divvy2<-divvy %>% filter(start_station_name!='') %>% filter(end_station_name!='')

```

 Again check the data

```{r}
skim_without_charts(divvy2)
```
Now Our data has been cleaned and well processed for analysis


# 4. Analysis phase

# AVerage of ride duration by bike type of casual and member
```{r}
 sqldf("select bike_type,  avg(ride_length) as Average_length ,member_casual from divvy2 
               group by bike_type,member_casual ")
```
# Total ride length duration by bike type of member and casual

```{r}
sqldf("select bike_type,  sum(ride_length) as Total_length ,member_casual from divvy2 
               group by bike_type,member_casual ")
```



#  Average ride length duration of  month jan to june of member and casual
```{r}
sqldf("select month,  avg(ride_length) as avg_length ,member_casual from divvy2 
               group by month,member_casual ")
```


#  Average  ride length duration in weekday of member and casual
```{r}
sqldf(" select weekday ,avg(ride_length) as average_length,member_casual
      from divvy2  group by weekday ,member_casual  ")

```


# 5. Share phase
Visulaization of data has been prepared in Tableau public


link are [here](https://public.tableau.com/app/profile/sumit.manhas7726/viz/Divvy_bike_share2021/Dashboard1)
  
  
  ![Dashboard 1](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/c14b4589-5ef9-4cc5-862b-0e28220c9d44)


























