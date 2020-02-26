# Inside Airbnb Oslo (CRISP-DM)
## by Truls Møller

### Date created
2020-02-26

## Business Questions
Our focus area is the city of Oslo - the capital of Norway - and we have identified the following business questions.

**Question 1**: How do Airbnb prices in Oslo vary throughout the year? Any trends / seasonality observed?

**Question 2**: Which are the most expensive neighbourhoods, when calculated per guest?

**Question 3**: In which neighbourhoods do guests feel like they got good value for their money?

**Question 4**: What are the top predictors of price?

## Data

### Source
The data is gathered from Inside Airbnb (http://insideairbnb.com/), which is a free source, and the listings data is from real listings on the date 2019-11-29. It would have been even better to have access to Airbnb's API, which I assume would contain transactional historic data, but that requires Airbnb to approve your corporation and it comes at a cost. That is not feasible for this project.

### Files:
1. 'neighbourhoods.geojson' (shape: 17, 3)
2. 'listings.csv' (shape: 8604, 106)
3. 'calendar.csv.gz' (shape: 3140460, 7)

### About:
**File 1** is a geopandas 'GeoDataFrame', which makes it possible to plot on a city map divided into neighbourhoods. Otherwise it's like normal pandas.

**Key variables:**
- 'neighbourhood' - self-explanatory
- 'geometry' - multipolygon coordinates that together can map up the city map.

**File 2** is the listings data.

**Key variables:**
- 'id' - listing id
- 'neighbourhood_cleansed' - more accurate than 'neighbourhood'
- 'longitude' & 'latitude' - coordinates, five decimals
- 'amenities' - various amenities in a single text column
- 'room_type' - whether it's a private room, a shared room etc.
- 'property_type' - whether it's an apartment or house or cabin etc.
- 'price' - price for a one night stay
- 'accommodates' - how many people can be accommodated
- 'guests_included' - how many guests are included in 'price'
- 'extra_people' - price per extra person beyond 'guests_included'
- 'price_full' - price when max people are accommodated
- 'price_per_guest' - 'price_full' divided by 'accommodates'
- 'grid_sectors' - categorical labels representing 0.025x0.025 squares on the map based on 'longitude' & 'latitude' coordinates of the lower left corner of each square.

**File 3** calendar data for the upcoming 365 days for each listing.

**Key variables:**
- 'date' - calendar date
- 'adjusted_price' - preferred over 'price' as it seems more accurate


## Findings Regarding Business Questions

**Question 1**: How do Airbnb prices in Oslo vary throughout the year? Any trends / seasonality observed?

1. Weekend peaks. All the small peaks are two-day weekend peaks at Friday and Saturday.
2. Non-winter peak season. All the way from mid-March through November. However, it's possible that this is not the case, and that it rather is some yearly price adjustment happening, for inflation and/or currency effects. If so the peak season trend could be even weaker than this, only with elevated prices from June-Septemeber if 1-2 percent, which is almost nothing.
3. New year peak. There seems to be a peak around new year.

Note that even if these trends are weak on average, they could be significant on single listings. Not all hosts will be equality diligent in raising prices when demand is high.

**Question 2**: Which are the most expensive neighbourhoods, when calculated per guest?

Sentrum (city center) is the most expensive neighbourhood, closely followed by Frogner, which is known as the posh part of town. Not far behind are the popular neighbourhoods of St. Hanshaugen, Gamle Oslo and Grünerløkka.

**Question 3**: In which neighbourhoods do guests feel like they got good value for their money?

Good value would indicate that get something good for less money.

Among the more central neighbourhoods Sagene seems to offer great value. There's also some neighbourhoods in the south-east, Nordstrand, Søndre Nordstrand and Østensjø that offer great value, if you can live with a small commute to the city center.

**Question 4**: What are the top predictors of price?

The top five predictors of price are:
1. grid_sectors_59.9_10.7
2. bathrooms
3. accommodates
4. grid_sectors_59.9_10.725
5. grid_sectors_59.9_10.75

## Model

LinearRegression (scikit-learn) used for our linear regression model used to answer Question 4.

variance_inflation_factor (statsmodels) used to evaluate covariance among some of the variables ('bedrooms', 'bathrooms', 'accommodates').

## Key Insights for the Blog

All the findings regarding business questions make it to the blog.

Except the top predictors of price are boiled down to:

1. Location (represented by three adjacent grid sectors)
2. Roominess (represented by how many people can be accommodated and the number of bathrooms)
