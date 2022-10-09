# Google Data Analytics Professional Certificate Capstone Project(Case Study 1 : Cyclistic)

This project is about a fictional company named Cyclistic, and it is a bike-share company bases in Chicago. I am working for the company as a Junior Analyst, 
and I will meet different characters and team members in the Marketing Analyst Department in the course of my work. In data analytics generally, key business questions
to be answered in order to make data-driven decisions will follow the steps of the data analysis process: Ask, prepare, process, analyze, share, and act. According
to what I learnt in Google Data Analytics Professional Certificate Course, the stages involved in Data Analysis are Asking questions, Preparation of data, Processing 
of data from dirty to clean, Analyze data to answer data-driven questions, Sharing of data, and Act. 

**About the company**
In 2016, Cyclistic Bike Share from California launched a successful bike-share offering. Ever since, the Bike-Share program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked by riders from one station and returned to any other station in the system anytime and get locked. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members. Cyclistic’s stakeholders concluded that annual members are much more profitable than casual riders.  Stakeholders want to design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, our marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. The Director of Marketing(Mr. Moreno) and our Marketing Analyst Team Manager are responsible for the development of campaigns and initiatives to promote bike-share programmes. While our team is interested in analyzing the Cyclistic historical bike trip data to identify trends.

**Scenario:** I am tasked to identify how the casual riders and annual members use our bikes differently and make necessary recommendations to the stakeholders. 

**My Role:** I am a Junior Data Analyst in a Marketing Analyst Team at Cyclistic Bike-Share Company in Chicago. 

**Goal of the Project:** To design strategies of converting casual riders to annual members

## Stage 1: ASK
**Business Question :** I have been assigned to answer this question: How do annual members and casual riders use Cyclistic bikes differently? This question needs to be answered in order to make data-driven decisions in our Bike-Share Company Cyclistic.

## Stage 2: PREPARE
Stage 2: PREPARE
According to improvado blog site, Data preparation is the act of aggregating raw data and transforming it into a format that can be easily analyzed. This can be simply put as the process of collecting and storing data for analysis. Since I know the question that needs to be answered, I will go ahead to briefly explain how I prepared and processed my data. 

On the 25th of September, 2022, I downloaded the last 12 months data. So, I downloaded data within September 2021 - August 2022 from this [site](https://divvy-tripdata.s3.amazonaws.com/index.html). September 2022 data was not available at the time I started the project. All the data were in zip files in csv format. I extracted them, and downloaded the files. Another folder is then created to convert the data into worksheets. After I had extracted the files, all the 12 months data to be analyzed can be found here in udrop folder. All the data were put in their column to better study them.

## Stage 2: PROCESS
During this stage I clean and check the data information to ensure its integrity. After thoroughly studied the downloaded data and went through the business questions, I realized that two additional columns will be needed at this point for all the 12 months data: 
a) ride_length( ended_at column minus started_at column). The started_at column and ended_at column contain the time that each rider started and ended their journeys respectively. 
b) day_of_week is calculated to be the day of the week for each trip.  I have to do this for all the 12 files since it will be too big for excel to merge them together. I saved the calculated data in a separate folder.

I made use of excel extensively by working on the data on worksheets(xlsx). In all the 12 months data, each of them has three sheets in them. One is for the raw data, while the second and the third sheets are working sheets and pivot tables. I created pivot tables for each data. Some of the values that were calculated include: Total number of member-casual base on types of rideable type, Total number of member-casual base on users(heavy, low, and normal users), Average ride length, maximum ride length, number of riders on weekdays base on member-casual. Because of time, I did not go further with creating a dashboard on excel since I will do that eventually for visualization.n. By making my hands dirty, the working sheets and pivot tables for all the twelve months can be seen in this [udrop](https://www.udrop.com/folder/8ab1c577fbb32925cc6e24ec20801e61/Worksheet_for_pivot_tabele-_Cyclistic_Bike_Share_Data).

 I wanted to use SQL at first but since R was taught in the course also, I decided to enhance my R skills with other tasks such as merging of the 12 files, creation of more tables, format and transformation of data. 
 
## Stage 4: ANALYSE
 As stated above, R was used to format and transform and merged data. Our data is sorted, filtered and columns that are not needed are deleted while more columns are added.

My full R code  is provided here on my [github](https://github.com/Gokoyer/Cyclistic-Data-Analytics/blob/master/2_cyclistic_30_9.R). For the sake of clarity and conciseness, I exclude all my mistakes and errors.
Below are all the new columns that were added to the merged data file:
i) ride_length - this is the time each rider travels in a single trip.The difference between the ended_at time and started_at time will result in ride_length. Difftime function is used and the resultant value is converted to minutes. all_12months_data$ride_length <- difftime(cyclistic_df$ended_at, cyclistic_df$started_at, units = "mins")

ii)ride_date:- Started_at date is used. The function that is applied here is 
as.Date. all_12months_data$ride_date <- as.Date(all_12months_data$started_at)

iii) day_of_week :- In this column, it shows days in numerical form(1=Sunday, 2= Monday, 3 = Tuesday, 4= wednesday, 5= Thursday, 6= Friday, 7=Saturday) in which riders start their journey. wday function is used. By default, 1 stands for Sunday. 

iv) Name of day of week :- This data represents the day names(Sunday, Monday and etc) The function that is used here is format
(as.Date(Data)). 
 all_12months_data$day_of_week <- wday(all_12months_data$started_at) .

v) month :- In this column, corresponding months that riders started their journeys are represented by numbers where for example “9” stands for September.
all_12months_data$month <- format(as.Date(all_12months_data$ride_date), "%m") Other codes can be found in [R code](https://github.com/Gokoyer/Cyclistic-Data-Analytics/blob/master/2_cyclistic_30_9.R). 


## Stage 4: SHARE 
 Since Tableau is the visualization tool that we used in Google Data Analytics Professional Certificate, I decided to use it. Some of the visualizations that were created can be seen below:
 ![1_member-casual count](https://user-images.githubusercontent.com/36123056/194763199-b0066347-6875-44d9-8f93-bf9a434d977e.png)
 
 ![2_member-casual by rideable type](https://user-images.githubusercontent.com/36123056/194763267-9ed6a69e-f600-4482-a868-8b7ad30ef292.png)
 
 ![3_Average Ride Length by Weekday](https://user-images.githubusercontent.com/36123056/194763648-6fa218f5-ee95-42e5-87a7-789604e96fb9.png)
 
 ![4_Total Rides Per Weekday](https://user-images.githubusercontent.com/36123056/194763698-9cfb3eac-864a-414a-9811-362744081fe6.png)
 
 ![5_Total Rides Per Season](https://user-images.githubusercontent.com/36123056/194763719-8dc7d679-4d0a-4c19-a370-21fb8a1d1bac.png)
 
 ![6_Total Rides By Months By Seasons](https://user-images.githubusercontent.com/36123056/194763750-f559b8b8-cd6b-46f4-a216-ce18ad2163d3.png)
 
 ![7_Total Rides by Hour](https://user-images.githubusercontent.com/36123056/194763774-50df623f-48db-401a-a6df-5db7836fbd8d.png)
 
 Anyone interested to view individual data in Tableau can do so via all the links below:
[member-casual count](https://public.tableau.com/app/profile/gboyega6535/viz/member-casualcount/member-casual?publish=yes)
[member-casual by Rideable Type](https://public.tableau.com/app/profile/gboyega6535/viz/member-casualbyRideableType/bike_type?publish=yes)
[Average Ride Length by Weekday](https://public.tableau.com/app/profile/gboyega6535/viz/AverageRideLengthbyWeekday/RideLengthperWeekday?publish=yes)
[Total Rides by Weekday](https://public.tableau.com/app/profile/gboyega6535/viz/TotalRidesbyWeekday/TotalRidesPerWeekday?publish=yes)
[Total Rides by Season](https://public.tableau.com/app/profile/gboyega6535/viz/TotalRidesbySeason/TotalRidesPerSeason?publish=yes)
[Total Rides by Month by Season](https://public.tableau.com/app/profile/gboyega6535/viz/TotalRidesbyMonthbySeason/TotalRidesByMonthsBySeasons?publish=yes)
[Total Rides by Hour](https://public.tableau.com/app/profile/gboyega6535/viz/TotalRidesbyHour/TotalRidesPerHour?publish=yes)


The full Tableau dashboard can be assessed via this [link](https://public.tableau.com/app/profile/gboyega6535/viz/ChicagoCyclisticBikeShareAnalysisCaseStudy-GoogleDataAnalyticsCapstoneProject/GDAPCDashboard).


![8_GDAPC Dashboard](https://user-images.githubusercontent.com/36123056/194764478-0f0520be-4b6d-48f7-94d8-083d3e2734f0.png)

 
 




