Bellabeat 
-specializes in creating fitness products specifically aimed at women, such as smart water bottles, fashionable fitness watches and jewelry, and yoga mats. Users can monitor their health data collected by these devices through the Bellabeat app. Bellabeat’s co-founders are interested in examining data from non-Bellabeat fitness devices in order to understand how consumers utilize these products. The company hopes to leverage these insights to shape new marketing strategies.

Ask

Key Stakeholders
•	Urška Sršen - Bellabeat's co-founder and Chief Creative Officer
•	Sando Mu - Mathematician and Bellabeat's co-founder
•	Bellabeat marketing analytics team - a team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.

Bellabeat Products
1.	Bellabeat app - offers users health data concerning their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can assist users in gaining insight into their current habits and making informed, healthy choices. Furthermore, the Bellabeat app is compatible with a range of smart wellness products.
2.	Leaf -  Bellabeat classic wellness tracker is versatile and can be worn as a bracelet, necklace, or clip. It connects to the Bellabeat app to monitor activity, sleep, and stress.
3.	Time - This smartwatch blends the timeless style of a traditional timepiece with advanced technology to monitor user activity, sleep, and stress. The Time watch connects to the Bellabeat app to offer insights into your daily well-being.
4.	Spring - This smart water bottle keeps track of your daily water intake to help you stay properly hydrated. It syncs with the Bellabeat app to monitor your hydration levels.
5.	Bellabeat Membership - Bellabeat provides a subscription-based membership program that grants users round-the-clock access to personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness tailored to their lifestyle and goals.

Business Task
Analyze non-Bellabeat smart device data and compare with one Bellabeat product to discover insight to help guide marketing strategies for the company.

Questions
1.	What are some trends in smart device usage?
2.	How could these trends apply to Bellabeat customers?
3.	How could these trends help influence Bellabeat marketing strategy?
  
Preparation

Data Source:
FitBit Fitness Tracker Data can be found on Kaggle, split into 18 CSV files. The dataset comprises smart health data from personal fitness trackers for thirty Fitbit users. This data was gathered through a survey of personal tracker data, which includes minute-level output for physical activity, heart rate, and sleep monitoring, conducted via Amazon Mechanical Turk between March 12, 2016, and May 12, 2016. The dataset was last updated two years ago as of August 2022 and provides information on daily activity, steps, and heart rate.

Restrictions:
1.	The sample size is limited because only 30 individuals were included in the study.
2.	The data is approximately six years old, and it's probable that FitBit devices have advanced to provide more precise results.
3.	As the data was obtained via a survey, the results may not be entirely accurate due to the potential for participants to provide less than honest responses.
4.	The weight data is only available for eight users, with one field being mostly blank and about two-thirds of the weight entries being manually input.

Process

Choosing Data Files
•	dailyActivity_merged.csv - provide a good summary of steps and calories burned
•	sleepDay_merged.csv - file provides sleep data these are good overall files to   use to analyze participant usage
•	weighLogInfo_merged.csv - containing weight data will also be used

Applications
•	Excel will be used load and take an initial pass for issues
•	R to transform and explore the data

Transform and explore data files
1.	Install and load tidyverse and data files.
2.	Check the data load correctly.
3.	Convert the Id field to character data type.
4.	Change the names of ActivityDate, SleepyDay, and Date in order to convert them to the date data type. 
activity <-activity %>% 
	mutate_at(vars(Id), as.character) %>% 
  mutate_at(vars(ActivityDate), as.Date, format = "%m/%d/%y") %>% 
  rename("Day"="ActivityDate")
![image](https://github.com/EugeneAllan/Bellabeat/assets/171637022/15710c97-af4e-4434-ba40-9c7a07b38db0)

5.	Combine the data frames and add day of the week.
combined_data <-sleep %>%
  right_join(activity, by=c("Id","Day")) %>%
  left_join(weight, by=c("Id", "Day")) %>%
  mutate(Weekday = weekdays(as.Date(Day, "m/%d/%Y")))


6.	Find and remove duplicate rows and count NAs and distinct Ids
combined_data <-combined_data[!duplicated(combined_data), ]
sum(is.na(combined_data))
n_distinct(combined_data$Id)
n_distinct(sleep$Id)
n_distinct(weight$Id)


•	Note the following details: The final data frame consists of 940 variables across 25 different variables. There are 33 unique Id entries in total. The number of distinct users in dailyActivity, sleepDay, and weightLogInfo are 33, 24, and 8, respectively. The combined data contains 6893 NAs. This isn't unexpected, as there is weight data from only eight users and not all users logged sleep information.

Analyze
Select a summary statistics
combined_data %>% 
select(TotalMinutesAsleep, TotalSteps, TotalDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, WeightKg, Fat, BMI, IsManualReport) %>% summary()


•	The typical user has a weight of 72.04 kg, a BMI of 25.19, and engages in light activities for the majority of their time. On average, they sleep for 6.9 hours, take 7,638 steps, and cover a distance of 5.49 km per day.


•	Users were most active on Sundays, taking the most steps, while they were least active on Fridays, taking the fewest steps. Given the consistently high activity levels, the marketing team could infer that the users place a high value on the step-tracking feature of health and fitness devices. This suggests that the feature would be highly beneficial for Bellabeat customers.

 
•	It’s intriguing to observe that physical activity levels decline on Wednesdays and then increase again on Thursdays. This could be because people are returning to work on Monday and feeling fatigued or discouraged by Wednesday. Additionally, Wednesday might be a popular rest day, leading to a resurgence in activity on Thursday.
 

•	The histogram indicates that most people slept between 312 and 563 minutes (5.2 to 9.4 hours). It’s important to note that this does not account for the total time spent in bed resting.

 
•	Aside from a few outliners, calories were primarily burned by individuals who slept between 5 and 7 hours. When focusing on weight loss and calories burned, correlates with a 5.2 to 9.4-hour sleep range, suggesting that those who stay within this range tend to burn more calories.
 

•	The logged feature was rarely used, with many blanks in the data no records available for Thursday and Friday. The highest logged distances were on weekends or during times when people likely had more free time for physical activities.

Act
•	The number of steps users took was lowest on Fridays, possibly due to fatigue at the end of the week. Since this trend isn't unique to FitBit customers, the marketing team could send notifications on Thursday evenings and Friday and Saturday mornings to encourage users to stay physically active throughout the day.
•	Many users did not utilize the Logged Distance feature on FitBit devices, suggesting a preference for automatic data collection. Consequently, the Bellabeat marketing team might consider omitting an activity distance log feature, as it appears to be underused by users.
•	Relative to the data set size, there were very few weight entries. Of those entered, about two-thirds were done manually. Individuals who did not log their weight might not have been focused on weight loss or lacked a device to automatically record this data. Given that many users also did not utilize the logged distance feature, the Bellabeat team could consider marketing smart scales that automatically record weight information.
•	Other data sources, such as the Mi Band fitness tracker data (April 2016 - present), could be valuable for further exploration, as this specific data set tracks an individual over the course of six years.

