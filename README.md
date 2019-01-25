# 2016 US Bike Share Activity Exploratory Data Analysis

## Table of Contents
- [Introduction](#intro)
- [Posing Questions](#pose_questions)
- [Data Collection and Wrangling](#wrangling)
  - [Condensing the Trip Data](#condensing)
- [Exploratory Data Analysis](#eda)
  - [Statistics](#statistics)
  - [Visualizations](#visualizations)
- [Performing Your Own Analysis](#eda_continued)
- [Conclusions](#conclusions)


<a id='intro'></a>
## Introduction

Over the past decade, bicycle-sharing systems have been growing in number and popularity in cities across the world. Bicycle-sharing systems allow users to rent bicycles for short trips, typically 30 minutes or less. Thanks to the rise in information technologies, it is easy for a user of the system to access a dock within the system to unlock or return bicycles. These technologies also provide a wealth of data that can be used to explore how these bike-sharing systems are used.

In this project, an exploratory analysis is performed on data provided by [Motivate](https://www.motivateco.com/), a bike-share system provider for many major cities in the United States. The system usage is compared between three large cities: New York City, Chicago, and Washington, DC. Any differences were investigated within each system for those users that are registered, regular users and those users that are short-term, casual users.


<a id='pose_questions'></a>
## Posing Questions

1. In which months of the year, people use bikes most/least, in each of the three cities?
2. What is the porpotion of subscribers/customer in all the three cities?
3. Which city has the highest user density? i.e. users / city area.


<a id='wrangling'></a>
## Data Collection and Wrangling

This project focuses on the record of individual trips taken in 2016 from our selected cities: New York City, Chicago, and Washington, DC. Each of these cities has a page where we can freely download the trip data.:

- New York City (Citi Bike): [Link](https://www.citibikenyc.com/system-data)
- Chicago (Divvy): [Link](https://www.divvybikes.com/system-data)
- Washington, DC (Capital Bikeshare): [Link](https://www.capitalbikeshare.com/system-data)

If you visit these pages, you will notice that each city has a different way of delivering its data. Chicago updates with new data twice a year, Washington DC is quarterly, and New York City is monthly. The data has already been collected, and can be found in the `/Data/` folder. While the original data for 2016 is spread among multiple files for each city, the files in the `/Data/` folder collect all of the trip data for the year into one file per city. Some data wrangling of inconsistencies in timestamp format within each city has already been performed. In addition, a random 2% sample of the original data is taken to make the exploration more manageable. 


<a id='condensing'></a>
### Condensing the Trip Data

It is observable that each city provides different information. Even where the information is the same, the column names and formats are sometimes different. To make things as simple as possible for the actual exploration, data was trimed and cleaned to makes sure that the data formats across the cities are consistent.

New data files were generated with five values of interest for each trip: trip duration, starting month, starting hour, day of the week, and user type. Each of these may require additional wrangling depending on the city:

- **Duration**: This has been given to us in seconds (New York, Chicago) or milliseconds (Washington). A more natural unit of analysis will be if all the trip durations are given in terms of minutes.
- **Month**, **Hour**, **Day of Week**: Ridership volume is likely to change based on the season, time of day, and whether it is a weekday or weekend. Use the start time of the trip to obtain these values. The New York City data includes the seconds in their timestamps, while Washington and Chicago do not. The [`datetime`](https://docs.python.org/3/library/datetime.html) package will be very useful here to make the needed conversions.
- **User Type**: It is possible that users who are subscribed to a bike-share system will have different patterns of use compared to users who only have temporary passes. Washington divides its users into two types: 'Registered' for users with annual, monthly, and other longer-term subscriptions, and 'Casual', for users with 24-hour, 3-day, and other short-term passes. The New York and Chicago data uses 'Subscriber' and 'Customer' for these groups, respectively. For consistency, you will convert the Washington labels to match the other two.


<a id='eda'></a>
## Exploratory Data Analysis

An initial exploration into the data where descriptive statistics are computed. The relative volume of trips made between three U.S. cities and the ratio of trips made by Subscribers and Customers are compared. For one of these cities, differences between Subscribers and Customers in terms of how long a typical trip lasts have been investigated.

<a id='statistics'></a>
### Statistics

- Which city has the highest number of trips? Which city has the highest proportion of trips made by subscribers? Which city has the highest proportion of trips made by short-term customers?

  **NYC** has the highest number of trips (276798).

  **NYC** has the highest proportion of trips made by subscribers (245896).

  **NYC** has the highest proportion of trips made by short-term customers (30902).

  * Washington_number_of_trips
    (51753, 14573, 66326)

  * Chicago_number_of_trips
    (54982, 17149, 72131)

  * NYC_number_of_trips
    (245896, 30902, 276798)

- Bike-share systems are designed for riders to take short trips. Most of the time, users are allowed to take trips of 30 minutes or less with no additional charges, with overage charges made for trips of longer than that duration. What is the average trip length for each city? What proportion of rides made in each city are longer than 30 minutes?

  **Washington**
    * Washington_total_min:  925382
    * Washington_number_of_trips:  66326
    * Washington_avg_trp_leng:  13.952024846968007
    * Washington_30min_prop:  0.10838886711093688

  **Chicago**
    * Chicago_total_min:  992921
    * Chicago_number_of_trips:  72131
    * Chicago_avg_trp_leng:  13.765523838571488
    * Chicago_30min_prop:  0.08332062497400562

  **NYC**
    * NYC_total_min:  3846660
    * NYC_number_of_trips:  276798
    * NYC_avg_trp_leng:  13.896993475386383
    * NYC_30min_prop:  0.07302437156337835
    
- Digging deeper into the question of trip duration based on ridership. Within that city of Washington for example, which type of user takes longer rides on average: Subscribers or Customers?

  **Washington**'s customers take longer rides on average (41.67803139252976) compared to its subscribers (12.528120499294745).
  
  
<a id='visualizations'></a>
### Visualizations

The last set of values computed have pulled an interesting result. While the mean trip time for Subscribers is well under 30 minutes, the mean trip time for Customers is actually _above_ 30 minutes! How the trip times are distributed is plotted.

In the above cell, fifty trip times are collected in a list, and passed this list as the first argument to the `.hist()` function. This function performs the computations and creates plotting objects for generating a histogram, but the plot is actually not rendered until the `.show()` function is executed. The `.title()` and `.xlabel()` functions provide some labeling for plot context.

These functions are used to create a histogram of the trip times for the city previously selected.

The plot is completely unexpected. The plot consists of one extremely tall bar on the left, maybe a very short second bar, and a whole lot of empty space in the center and right. Take a look at the duration values on the x-axis. This suggests that there are some highly infrequent outliers in the data. Instead of reprocessing the data, additional parameters are used with the `.hist()` function to limit the range of data that is plotted. Documentation for the function can be found [[here]](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.hist.html#matplotlib.pyplot.hist).

The parameters of the `.hist()` function are used to plot the distribution of trip times for the Subscribers in the previously selected city. The same thing is done for only the Customers. Limits are added to the plots so that only trips of duration less than 75 minutes are plotted. The plots are setup so that bars are in five-minute wide intervals.

For each group, where is the peak of each distribution? How would you describe the shape of each distribution?

The trip duration distribution of **Washington Subscribers** peaks at 5-10 minutes and the shape of it is skewed to the right. On the other hand, **Washington Customers** trip duration peaks at 15-20 minutes, and the shape of the distribution is closer to a normal distribution, i.e. bell-shape compared to the former.


<a id='eda_continued'></a>
## Performing Your Own Analysis

The following questions are investigated and visualized.
1. In which months of the year, people use bikes most/least, in each city?
   * In Washington, bikes are used the most in July, and the least in January.
   * In Chicago, bikes are used the most in July, and the least in December.
   * In NYC, bikes are used the most in September, and the least in January.
2. What is the porpotion of subscribers/customer in all the three cities?
   * NYC has the highest number of Users (276798), where most of them are Subscribers (245896), and very few are Customers (30902).
   * Chicago comes second after NYC in its number of Users (72131), where most of them are Subscribers (54982), and a few are Customers (17149).
   * Washington has the least number of Users (66326), where most of them are Subscribers 51753, and a few are Customers 14573.

To-be Explored:
- How does ridership differ by month or season? Which month / season has the highest ridership? Does the ratio of Subscriber trips to Customer trips change depending on the month or season?
- Is the pattern of ridership different on the weekends versus weekdays? On what days are Subscribers most likely to use the system? What about Customers? Does the average duration of rides change depending on the day of the week?
- During what time of day is the system used the most? Is there a difference in usage patterns for Subscribers and Customers?


<a id='conclusions'></a>
## Conclusions

Conclusions about the data are drwan by performing a statistical test or fitting the data to a model for making predictions. There are also a lot of potential analyses that could be performed on the data which are not possible with only the data provided. For example, detailed location data has not been investigated.
* Where are the most commonly used docks?
* What are the most common routes?

As another example, weather has potential to have a large impact on daily ridership.
* How much is ridership impacted when there is rain or snow?
* Are subscribers or customers affected more by changes in weather?
