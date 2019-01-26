## Project Luther: Understanding impact of weather on bike commuter traffic across Fremont bridge

Presentation date: Jan 25th, 2019

### Summary of completed work

#### Problem statement

The goal was to quantify how much the daily commute traffic for bikers is influenced by the weather - i.e., how many more bikers set out on an extra nice day or fewer on a yucky one than would have ordinarily?

This came out of a more general premise that Seattle people are reportedly indifferent to the weather - often not bothering with umbrellas when it rains, for example. I was curious to see to what extent that was true!

#### Gathering data

Jupyter notebook: *Project2_code_part1_scrape_weather.ipynb* 

The bike counts were available as a downloadable csv file at https://data.seattle.gov/Transportation/Fremont-Bridge-Hourly-Bicycle-Counts-by-Month-Octo/65db-xm6k

The weather had to be scraped from https://nowdata.rcc-acis.org/sew/ 

I used Selenium exclusively, no Beautiful Soup was necessary. Learnings:

- There is a way to copy Xpath for a specific element from the code browser window
- If the button doesn't respond to the click() action, send in ENTER via Keys
- If I ran all the code at once, it would try to scrape the pages before they loaded and error out; adding a 1-second sleep fixed this issue

#### Data processing

Jupyter notebook: *Project2_code_part2_analysis_final.ipynb* 

I had to convert my scraped weather data from strings to numbers and dates, as needed. I then aggregated the hourly bike count data by day and merged with the daily weather.  One sticky point as I worked with the data was that the Pandas DateTime format and dates defined via Datetime library don't automatically understand each other and so I had to keep making conversions back and forth to apply filters and get the charts to work.

The final list of features used for the winner model was:

- Temp-Avg: average temperature for the day

- Temp HiLo delta: the difference from that day's highest temperature to the lowest

- Precipitation: inches of rain

- Snow_Depth: the existing inches of snow for that day

- sin_month, cos_month: every month (1 through 12) was transformed into 
  $$
  2*pi*month/12
  $$
  and described as two columns, sin(month_transformed) and cos(month_transformed)

- Is_holiday: whether a given date is a holiday; I had to create a manual list of dates for the 2013-2018 years that would be marked as holidays - I started at the listing of Federal Holidays for a given year (e.g. https://www.ca3.uscourts.gov/2016-federal-holidays) and then made a call on which ones to include, since only the biggest holidays are truly shared by everyone with some of the smaller ones (or days after the holiday) are only given off in some companies.  The final list was: New Year's Day, MLK, Memorial Day, Independence Day, Labor Day, Thanksgiving, the day after Thanksgiving, Christmas Night, Christmas Day, New Year's Eve.  I initially also added Columbus Day and Veterans Day, but found that the quality of prediction went down, so I took them back out.

- Weekday: a set of dummy variables for Monday, Tuesday, Wednesday, Thursday, Friday. I originally also had Saturday, but it fell off due to not having predictive power (since the day I originally left out was Sunday, there is apparently not much difference in traffic flow between Sunday and Saturday)

- Weekend_rain: whether it's a weekend and precipitation is > 0

- Precipitation^2: squared number of inches of rain for that day

#### Analysis

1. I initially experimented with transforming my dependent variable via log1p because bike trip counts are non-negative, but ultimately the prediction quality was higher and the model was easier to interpret if I kept the y as is, even at the cost of having some negative predictions in the end
2. Both Lasso and OLS performed equally well in the cross-validation; I chose OLS due to higher interpretability
3. The Weekend_rain and Precipitation^2 variables were added upon review of residuals.  There is still an issue with the residuals for the weekend - there's a clear slope pattern that indicates a likely missing interaction variable
4. To determine the relative impact of weather, I compared the OLS model with weather variables to the one without. Caveat: I recognize that there is some built-in information about the weather in the calendar months (that's also easily visible in the correlation between average temperature and month variables in the heat map) - else the traffic flow would not vary from winter to summer.  So in a way, my weather-agnostic model is not truly completely so, and the weather-inclusive model is more of an indication of how much the weather on any given day varies from the "average expected weather" for that month.
5. Lastly, I really wanted to quantify in an intuitive way how much specific weather influences the typical traffic flow.  The metric I came up with was the average % difference in predicted bike trips between the two models.  For the average, I used the median rather than the mean to reduce the impact of extreme variations (although one might argue that it's specifically the days with extreme weather where the full model would shine 🤷🏻‍♀️).  The resulting 10% means that while on a typical day one might expect to see 1,000 bike trips, having nice weather translates into 1,100 trips instead - or likewise, 900 for bad weather.  While it's a noticeable difference, on the whole I would say the majority of commuters stick to their routine regardless of the elements, so I'll give Seattleites credit where it's due. 😁

#### Future work

- Fix the residuals for the weekends
- Consider adding more advanced considerations into the model - in my research, I learned about the impact of bike share companies entering and exiting Seattle; also, any information about alternative route closures would be useful, too.