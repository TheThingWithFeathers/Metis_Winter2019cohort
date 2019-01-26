# Project Luther Proposal: Do Seattleites like *[piña coladas and]* getting caught in the rain?

**Natalia Lichagina**

### Scope

In my year+ in Seattle to date, I have anecdotally established that Seattleites not only do not mind the cold and the rain, but can even be gleeful once the gloom descends.  I would like to investigate in a more quantitative manner the impact (or lack thereof?) of inclement weather on commuters and see whether I can predict the number of bicyclists going through the Fremont bridge for the upcoming days based on the forecast.

### Methodology

1. Get bicycle counts for Fremont bridge at a daily level for the past couple of years
2. Get temperature and precipitation levels at a daily level for the same time period
3. Build a linear regression that shows how much temperature and precipitation impact the number of commuters
4. Predict how many commuters will pass in the next couple of days, then check how well my model did

### Data

1. Fremont Bridge hourly bycicle counts (download)

https://data.seattle.gov/Transportation/Fremont-Bridge-Hourly-Bicycle-Counts-by-Month-Octo/65db-xm6k

   

2.  Daily historical temperature and precipitation readings for Seattle (scrape)

 https://w2.weather.gov/climate/xmacis.php?wfo=sew

   

### Prediction

Number of commuters per day for the next 2 days based on the expected daily temperature and precipitation for those days.

### Dependent Variable

- Number of bicycle commuters crossing the Fremont bridge in a day

### Features

- Numeric: Average temperature in F
- Numeric: Min temperature in F
- Numeric: Max temperature in F
- Numeric: Inches of precipitation
- Binary: Whether it rained
- Binary: Whether it snowed
- Categorical -> binary: Day of the week - M,T,W,Th,F,S,Su
- Numeric -> sin+cos: Month of the year
- Binary: Whether it's a holiday

### Things to consider

- Seattle has been the fastest-growing city in the nation, it's possible that the number of bike commuters through the bridge was significantly lower even a couple of years ago
- Any potential historical bridge closings or other traffic disruptions
- Other major shifts like a new big company opening an office close to the bridge - Google opened its Fremont office in 2006, for example
