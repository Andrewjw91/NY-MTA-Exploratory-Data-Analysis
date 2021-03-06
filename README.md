# Top Subway Stations for Rental Scooter Deployment

Andrew Wong

## Abstract

The goal of my exploratory data analysis project was to use the [NYC MTA data](http://web.mta.info/developers/turnstile.html) to determine the train stations with the highest average exit traffic to help rental scooter companies (Bird, Lime, Lyft etc.) decide the best areas for scooter deployment. I was able to leverage the [Stations By Neighborhood](https://new.mta.info/maps/subway/mta-neighborhood-maps) to further refine the top subway stations by the following boroughs: Bronx, Brooklyn, Queens, and Manhattan. Placing a larger number of scooters against higher foot traffic should help increase the potential for a rider, and the potential for recirculation of the scooters to subsequent riders.

## Design

The design of my analysis is based upon station exit count data. Rental scooter rides provide convenience to riders by providing a flexible mode of transportation to areas that may not have direct subway stations or lines. By determining high traffic areas for deployment, the results of my analysis can benefit scooter companies by increasing potential rental revenue, and increasing brand-name visibility.

## Data

The timeframe of data examined in my project is between the dates of 4/27/2019 to 7/26/2019. The NYC MTA turnstile data is published on a weekly basis, and includes the entry and exit cumulative counts for each turnstile, with categorial data allowing us to sort and aggregate by the Control Area, Unit, Station Name, and Train Lines.

Using the data from [Stations By Neighborhood](https://new.mta.info/maps/subway/mta-neighborhood-maps) I was able to create a separate table listing the borough associated with each station name. This allowed me to filter the station names and determine the top station by borough (Bronx, Brooklyn, Queens, and Manhattan).

The results of my analysis show that the top 10 stations with highest average daily exits are all in Manhattan. Although these stations have the highest exit traffic, these stations may not be particular areas of interest to scooter companies. Manhattan is densely populated with train stations, which means riders are able to choose their exit down to the specific block on a given street. There is likely less need for scooter rentals. Additionally, since these areas have such heavy traffic, the sidewalk/street conditions are not ideal for scooter placement, riding, or safety.

I chose to focus on the aggregate data for the top 5 stations in the Bronx, Brooklyn, and Queens respectively. Stations in these boroughs are less densely situated, thus there may be an increase likelihood that a scooter rental is needed to reach a desination.

The cumulative counts are updated after each 4 hour period of the day, with the last count typically recorded at time 20:00 on a given date. I chose to calculate the net exit counts by finding the max count per date and subtracting the minimum (starting count). Although this method excludes the count data between the hours 20:00 and 23:59, this time period is not as relevant for rental scooter companies. According to public online information, rental scooters are collected to be charged at night and must be redeployed between 04:00 and 07:00.

## Algorithms

In order to create a combined set of data for my time period, I used a python code to download the desired weeks of data as .txt files from the MTA website. I then ran a command line function to combine these files into a single .csv. Then I created a .csv from the Neighboorhood Maps stations list and joined the tables on station name in SQL. This allowed me to identify stations with the same name (e.g. 59 ST Brooklyn vs. 59 ST Manhattan) and alter the name with unique identifiers.

Using 'sqlalchemy' I imported my data into a pandas dataframe. I aggregated the net exits for each SCP turnstile for each day by taking the max-min count. In sorting this dataframe in descending order, I found that the top results had inexplicable jumps in count, with the largest increase around +1.8billion within a 4 hour period. Referencing the standard deviation (7,086,530) these values were more than 2 std from the mean (~51325).

I plotted a seaborn distplot for SCP all net exits less than 51325. The distribution showed the majority of data falling under 10,000 exits per day. I then created a new dataframe showing only rows with daily net exit values less than 51325. Removing outliers greater than 51325 created a new mean for single SCP net exits per day of 9091.

I then created a new dataframe grouped by Station, displaying the SUM of net exits per day for all SCP within each station. I was then able to take the mean to determine the average daily exits per station.

I created separate masks for each borough to filter the data by Bronx, Brooklyn, and Queens respectively. 

## Tools
SQL for data concatenation and filtering<br>
Pandas for data manipulation, analysis, and cleaning<br>
Seaborn for plotting and visualization<br>
Python to download data, and read the data using sqlalchemy engine
