G


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

I will be using Cyclistic’s historical bike trip data of 2021.In this data i will be perform 6 month data from jan to june which are publicly available [here](https://divvy-tripdata.s3.amazonaws.com/index.html) data is made available by motivate international inc under this [license](https://ride.divvybikes.com/data-license-agreement)

In term of bias and credibility, ROCC process has been used

Reliability: Divvy has the rights over the license of this data. Divvy is a program that’s part of the Chicago Department of Transportation (CDOT). The organization owns vehicles, stations, and a wide range of bikes around the city.

Original: The data collected is from Motivate International Inc. They manage the City of Chicago’s Divvy bicycle-sharing program.

Comprehensive: The data includes key information such as ride ID, type of bike, time the user started and ended the ride, start and end time, station ID, longitude and latitude, and finally the type of membership.

Current: The data is up to date to apr 2023.

Cited: The data is available under the current license agreement.




* Rstudio has been used for this phase and these libraries has been used
![IMG_20230809_124850](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/e06350e9-05f2-4510-b183-103c0e2f5946)


# import data
data1<- read.csv("c:/users/Sumit/Desktop/divvy_bike_share/Chicago/202101-divvy-tripdata.csv")

data2<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202102-divvy-tripdata.csv")

data3<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202103-divvy-tripdata.csv")

data4<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202104-divvy-tripdata.csv")

data5<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202105-divvy-tripdata.csv")

data6<- read.csv("C:/Users/Sumit/Desktop/divvy_bike_share/Chicago/202106-divvy-tripdata.csv")

# Process phase

* To merge all the files together we need to know that all files has same attributes,same data type in same sequence.

str(data1)

str(data2)

str(data3)

str(data4)

str(data5)

str(data6)

* All files has same attributes,same data type in same sequence and Now i have merged these all files together

divvy<-bind_rows(data1,data2,data3,data4,data5,data6)


* To see the structure of merged  data 
str(divvy)


* Head of the data
  
  head(divvy)


* Tail of data

 tail(divvy)

* To check the column in the data
 
colnames(divvy)
![IMG_20230809_173721](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/2561dd7c-f2b6-4c0f-adc9-d7177e93c1e7)





# Now data clean process has been started

* There is no need such type of column (start_lat,start_lng,end_lat,end_lng,start_station_id & end_station_id) in data so i have removed this from the data
 
divvy<-select(divvy,-start_lat,-start_lng,-end_lat,-end_lng,-start_station_id,-end_station_id)

* Again check the column
  
  colnames(divvy)
  ![IMG_20230809_173031](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/eb35deb5-ed76-41a1-89d8-c850305e4967)


  
* For understanding data clearly i have renamed one column of rideable_type
 
divvy<-rename(divvy,"bike_type"="rideable_type")

* In our data there was only one column of bike_type for data aggregation so few of the column has been added.

 divvy$month<-format(as_date(divvy$started_at),"%b")
 
 divvy$day<-format(as_date(divvy$started_at),"%d")
 

 divvy$weekday<-format(as_date(divvy$started_at),"%a")

 str(divvy)
 ![IMG_20230809_174040](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/47612473-4200-4031-8d40-f81417c28b36)

 
* We have no any numeric data type for which we can summarize our data, so ride length duration has been extracted from the column of start_at & ended_at

 divvy$ride_length<- difftime(divvy$ended_at,divvy$started_at)
 
 divvy$ride_length<-as.numeric(as.character(divvy$ride_length)) 

 summary(divvy)
 
![IMG_20230809_180137](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/98a0f69c-4445-466c-b9bd-941df24dd86c)

 * There is some negative values in ride_length column it means ended time is less than started time and also Ride_length column duration time have in second so it has been converted from second to minutes
 
  divvy<- divvy %>% filter(ride_length>0)
  

  divvy<- divvy %>%  mutate("ride_length"=ride_length/60)

  
skim_without_charts(divvy)
![IMG_20230809_174606](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/46242551-91e1-43d9-b048-192bc024d131)



* There is a empty values in column of start_station_name and end_station_name so i have to remove this and also new data frame has been added to avoid from the data loss

divvy2<-divvy %>% filter(start_station_name!='') %>% filter(end_station_name!='')


* Now Our data has been cleaned and well processed for analysis


# Analysis phase

* AVerage of ride duration by bike type of casual and member

 divvy2 %>% group_by(member_casual,bike_type) %>% summarize(Average_length=mean(ride_length))
 ![IMG_20230809_174947](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/c1049aa0-a840-4dfe-ab74-b5d8af61db74)

 
* Total ride length duration of member and casual
 
divvy2 %>% group_by(member_casual) %>%
  summarize(Total_ride=n())
  ![IMG_20230809_175304](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/80e9d416-c73c-4c7d-9ed1-56dcf06848ff)

  
* Average ride length duration in the month from jan to june of member and casual
 
 divvy2 %>% group_by(member_casual,month) %>% summarize(Average_length=mean(ride_length)) 
![IMG_20230809_175238](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/17551851-b826-4d9e-9a42-667601932741)


* Average ride length duration in weekday of member and casual

 divvy2 %>% group_by(member_casual,weekday) %>% summarize(Average_length=mean(ride_length))
 ![IMG_20230809_175208](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/a96daf6e-dcdb-4a2c-a9a4-70b6e21aaaa1)

#  Share phase

 divvy2 %>% group_by(member_casual,bike_type) %>% summarize(Average_length=mean(ride_length)) %>%
ggplot(aes(x=bike_type,y=Average_length,fill=member_casual)) + geom_col(position="dodge") +
  labs(title="Average duration of members & casual by bike type ")
![IMG_20230809_180359](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/0a32e17c-1a32-4251-8fdb-f3b8a51438df)


divvy2 %>% group_by(member_casual) %>% summarize(Total_ride=n()) %>% ggplot(aes(x=member_casual,y=Total_ride,fill=member_casual)) + geom_col()






divvy2 %>% group_by(member_casual,month) %>% summarize(Average_length=mean(ride_length)) %>%
ggplot(aes(x=month,y=Average_length,fill=member_casual)) +geom_col(position="dodge") +
  labs(title = "Average ride length of member & casual by month ")
  ![IMG_20230809_180758](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/efea2882-3676-44cc-be16-d5e2957c4892)

  
divvy2 %>% group_by(member_casual,weekday) %>% summarize(Average_length=mean(ride_length)) %>%
ggplot(aes(x=weekday,y=Average_length,fill= member_casual)) + geom_col(position = "dodge") +
  labs(title = "Average ride length of member & casual in weekday")
![IMG_20230809_180719](https://github.com/manhas1234/Cyclistic_bike_case_study/assets/130725137/82f37644-397e-41e1-bc6a-dbc61c373ba5)


# Act phase
Docked biked has been more used by casual riders as compared to member users .so encourage casual rider by giving them various coupon so that they can get a membership

Host fun biking competitions with prizes at sunday becuase lot of casual rider rides bike on this days


