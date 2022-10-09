library(tidyverse)
library(lubridate) #for dates
library(hms) #for time
library(data.table) #activate the package
getwd() # set or get working directory
sep21_data <- read_csv("202109-divvy-tripdata.csv")
oct21_data <- read_csv("202110-divvy-tripdata.csv")
nov21_data <- read_csv("202111-divvy-tripdata.csv")
dec21_data <- read_csv("202112-divvy-tripdata.csv")
jan22_data <- read_csv("202201-divvy-tripdata.csv")
feb22_data <- read_csv("202202-divvy-tripdata.csv")
mar22_data <- read_csv("202203-divvy-tripdata.csv")
apr22_data <- read_csv("202204-divvy-tripdata.csv")
may22_data <- read_csv("202205-divvy-tripdata.csv")
jun22_data <- read_csv("202206-divvy-tripdata.csv")
jul22_data <- read_csv("202207-divvy-tripdata.csv")
aug22_data <- read_csv("202208-divvy-tripdata.csv")


 
#combuine all the 12 months data together
cyclistic_df <- rbind(sep21_data, oct21_data, nov21_data, dec21_data, jan22_data, feb22_data, mar22_data, apr22_data, may22_data, jun22_data, jul22_data, aug22_data)

#this is to create new data frame to contain all new columns to be added
all_12months_data <- cyclistic_df

all_12months_data <- all_12months_data %>%  #remove columns not needed: ride_id, start_station_id, end_station_id, start_lat, start_long, end_lat, end_lng
  select(-c(ride_id, start_station_id, end_station_id,start_lat,start_lng,end_lat,end_lng)) 

#This is to ride length for each trip. This can be done by substracting ended_at time from started_at time.
#The ride_length will then be converted minutes
all_12months_data$ride_length <- difftime(cyclistic_df$ended_at, cyclistic_df$started_at, units = "mins")

#to get strptime format
?strptime

#create column for "day of weeks", "month", "day", "year", "time", "hour"
all_12months_data$ride_date <- as.Date(all_12months_data$started_at) #default format is yyyy-mm-dd, use start date

#calculate the day of the week
all_12months_data$day_of_week <- wday(all_12months_data$started_at) 

#format the day of the week columns into name of day in a week where 1st day of the week= Sunday.
all_12months_data$name_of_day_of_week <- format(as.Date(all_12months_data$ride_date ), "%A")

#create column for month
all_12months_data$month <- format(as.Date(all_12months_data$ride_date), "%m")

#create column for days of rides
all_12months_data$day <- format(as.Date(all_12months_data$ride_date), "%d")

#create column for year of rides
all_12months_data$year <- format(as.Date(all_12months_data$ride_date), "%Y")

#format time in HH:MM:SS format
all_12months_data$time <- format(as.Date(all_12months_data$ride_date), "%H:%M:%S")

#create time of ride in new column
all_12months_data$time <- as_hms(cyclistic_df$started_at)

#remove columns added by mistake
remove_cols <- c('all_12months_data$real_time', 'all_12months_data$rea_time')
#create hour of ride in new column
all_12months_data$hour <- hour(all_12months_data$time)

#create column for different seasons: There are Spring, Summer, Autumn, and Winter.
all_12months_data <-all_12months_data  %>% mutate(season = 
                                                    case_when(month == "03" ~ "Spring",
                                                       month == "04" ~ "Spring",
                                                       month == "05" ~ "Spring",
                                                       month == "06"  ~ "Summer",
                                                       month == "07"  ~ "Summer",
                                                       month == "08"  ~ "Summer",
                                                       month == "09" ~ "Autumn",
                                                       month == "10" ~ "Autumn",
                                                       month == "11" ~ "Autumn",
                                                       month == "12" ~ "Winter",
                                                       month == "01" ~ "Winter", 
                                                       month == "02" ~ "Winter"))

#create column for different time_of_day during the ride: Morning, Afternoon, Evening, NIght
all_12months_data <-all_12months_data %>% mutate(time_of_day = 
                                             case_when(hour == "0" ~ "Night",
                                                       hour == "1" ~ "Night",
                                                       hour == "2" ~ "Night",
                                                       hour == "3" ~ "Night",
                                                       hour == "4" ~ "Night",
                                                       hour == "5" ~ "Night",
                                                       hour == "6" ~ "Morning",
                                                       hour == "7" ~ "Morning",
                                                       hour == "8" ~ "Morning",
                                                       hour == "9" ~ "Morning",
                                                       hour == "10" ~ "Morning",
                                                       hour == "11" ~ "Morning",
                                                       hour == "12" ~ "Afternoon",
                                                       hour == "13" ~ "Afternoon",
                                                       hour == "14" ~ "Afternoon",
                                                       hour == "15" ~ "Afternoon",
                                                       hour == "16" ~ "Afternoon",
                                                       hour == "17" ~ "Afternoon",
                                                       hour == "18" ~ "Evening",
                                                       hour == "19" ~ "Evening",
                                                       hour == "20" ~ "Evening",
                                                       hour == "21" ~ "Evening",
                                                       hour == "22" ~ "Evening",
                                                       hour == "23" ~ "Evening"))


#clean the data in general 
#remove rows with NA values
all_12months_data <- na.omit(all_12months_data) 

#remove duplicate rows 
all_12months_data <- distinct(all_12months_data)

#remove where ride_length is 0 or negative
all_12months_data <- all_12months_data[!(all_12months_data$ride_length <=0),] 

 #TYPES OF MEMBERSHIP
all_12months_data %>%
  group_by(member_casual) %>% 
  count(member_casual)

#TYPE OF BIKE

#total bikes by member type 
all_12months_data %>%
  group_by(member_casual, rideable_type) %>% 
  count(rideable_type)

#total number of rides
nrow(all_12months_data)

#total bike rides 
all_12months_data %>%
  group_by(rideable_type) %>% 
  count(rideable_type)


#total rides by member type in the morning
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(time_of_day == "Morning") %>% 
  count(time_of_day)

#total rides in the morning
all_12months_data %>%
  filter(time_of_day == "Morning") %>% 
  count(time_of_day)

#total rides by member type in the afternoon
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(time_of_day == "Afternoon") %>% 
  count(time_of_day)

#total rides in the afternoon
all_12months_data %>%
  filter(time_of_day == "Afternoon") %>% 
  count(time_of_day)

#total rides by member type in the evening
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(time_of_day == "Evening") %>% 
  count(time_of_day)

#total rides in the evening
all_12months_data %>%
  filter(time_of_day == "Evening") %>% 
  count(time_of_day)

#total rides by member type at night
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(time_of_day == "Night") %>% 
  count(time_of_day)

#number of rides at night
all_12months_data %>%
  filter(time_of_day == "Night") %>% 
  count(time_of_day)

#total rides by member type by month 
all_12months_data %>%
  group_by(member_casual) %>% 
  count(month) %>% 
  print(n = 24) #lets you view the entire tibble

#total rides by month
all_12months_data %>%
  count(month) 

#total rides by member type in day of the month
all_12months_data %>%
  group_by(member_casual) %>% 
  count(day) %>% 
  print(n = 62) #lets you view the entire tibble

#total rides
all_12months_data%>%
  count(day) %>% 
  print(n = 31) #lets you view the entire tibble

#total rides by member type in Spring season 
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(season == "Spring") %>% 
  count(season)

#total rides in Spring season
all_12months_data %>%
  filter(season == "Spring") %>% 
  count(season)

#total rides by member type in summer
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(season == "Summer") %>% 
  count(season)

#total rides in summer
all_12months_data %>%
  filter(season == "Summer") %>% 
  count(season)

#total rides by member type in Autumn
all_12months_data %>%
  group_by(member_casual) %>% 
  filter(season == "Autumn") %>% 
  count(season)

#total rides in Autumn
all_12months_data %>%
  filter(season == "Autumn") %>% 
  count(season)

#total rides by member type in Winter
all_12months_data%>%
  group_by(member_casual) %>% 
  filter(season == "Winter") %>% 
  count(season)

#total rides in Winter
all_12months_data %>%
  filter(season == "Winter") %>% 
  count(season)

#total rides by member type in all the 4 seasons
all_12months_data %>%
  group_by(season, member_casual) %>% 
  count(season)

#total rides
all_12months_data %>%
  group_by(season) %>% 
  count(season)


#------------------------------------AVERAGE RIDE LENGTH-----------------------------------

#average ride_length
total_ride_avg <- mean(all_12months_data$ride_length)
print(total_ride_avg)

#------------------MEMBER TYPE--------------------

#average ride_length by membership
all_12months_data %>% group_by( member_casual) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#----------------TYPE OF BIKE---------------------

#total rides by member type 
all_12months_data %>% group_by(member_casual, rideable_type) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride_length
all_12months_data %>% group_by(rideable_type) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#-----------------------HOUR-------------------------

#average ride_length by member type by hour
all_12months_data %>% group_by(hour, member_casual) %>% 
  summarise_at(vars(ride_length),
               list(time = mean)) %>% 
  print(n=48) #lets you view entire tibble

#average ride_length by hour
all_12months_data %>% group_by(hour) %>% 
  summarise_at(vars(ride_length),
               list(time = mean)) %>% 
  print(n=24) #lets you view entire tibble


#average ride length by member type in the morning
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(time_of_day == "Morning") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length in the morning 
all_12months_data %>% 
  filter(time_of_day == "Morning") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type in the afternoon
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(time_of_day == "Afternoon") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length in the afternoon
all_12months_data %>% 
  filter(time_of_day == "Afternoon") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type in the evening
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(time_of_day == "Evening") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length in the evening
all_12months_data %>% 
  filter(time_of_day == "Evening") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type at night
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(time_of_day == "Night") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length at night
all_12months_data %>% 
  filter(time_of_day == "Night") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))


#average ride length by member type by time of the day
all_12months_data %>% 
  group_by(time_of_day, member_casual) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by time of the day
all_12months_data %>% 
  group_by(time_of_day) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))


#average ride_length by member type by day of the month
all_12months_data %>% group_by(day, member_casual) %>% 
  summarise_at(vars(ride_length),
               list(time = mean)) %>% 
  print(n=62)  #lets you view entire tibble

#average ride_length by day of the month
all_12months_data %>% group_by(day) %>% 
  summarise_at(vars(ride_length),
               list(time = mean)) %>% 
  print(n=31)  #lets you view entire tibble

#average ride_length by member type by day of the week
all_12months_data %>% group_by(member_casual, day_of_week) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride_length by day of the week
all_12months_data%>% group_by(day_of_week) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))



#average ride_length by member type by month
all_12months_data %>% group_by(member_casual, month) %>% 
  summarise_at(vars(ride_length),
               list(time = mean)) %>% 
  print(n=24)  #lets you view entire tibble

#average ride_length by month
all_12months_data %>% group_by(month) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type during spring season
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(season == "Spring") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type in all seasons
all_12months_data%>% 
  group_by(season, member_casual) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length in all seasons
all_12months_data%>% 
  group_by(season) %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length during spring season
all_12months_data %>% 
  filter(season == "Spring") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type during autumn season
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(season == "Autumn") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length
all_12months_data %>% 
  filter(season == "Autumn") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))


#average ride length by member type in summer season
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(season == "Summer") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length in summer season 
all_12months_data %>% 
  filter(season == "Summer") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length by member type during winter season
all_12months_data %>% 
  group_by(member_casual) %>% 
  filter(season == "Winter") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#average ride length during winter season
all_12months_data %>% 
  filter(season == "Winter") %>% 
  summarise_at(vars(ride_length),
               list(time = mean))

#This is to remove the "mins" as character attached  to data in column ride_length

all_12months_data$ride_length <- gsub("mins", "", as.character(all_12months_data$ride_length))


#converting ride_length back to numeric
all_12months_data$ride_length <- as.numeric(all_12months_data$ride_length)

#Rounding ride_length to 1 decimal places for tableau visaulization
dataframe_for_tableau <- all_12months_data %>%
  mutate_if(is.numeric,
            round,
            digits = 1)

#exporting of files
write.csv(cyclistic_df, file = "cyclistic_df.csv")

write.csv(all_12months_data, file = "all_12months_data.csv")

write.csv(dataframe_for_tableau, file = "dataframe_for_tableau.csv")
                             


