
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
# Four years of Blue Bikes

Blue Bikes (formerly Hubway) is owned by the municipalities of Boston, Brookline, Cambridge and Somerville, and operated by Motivate International, Inc in Boston, MA.





## About Data Source

The datasets used in this project are provided by Blue Bikes (Motivate International, Inc.)

It includes:

- Comprehensive set of trip histories which is updated each annual quarter
- Real time system data, published in open General Bikeshare Feed Specification (GBFS) format - a format recommended by the North American Bike Share Association (NABSA).

Home: [[link]](https://www.bluebikes.com/system-data)\
Dataset Bucket: [[link]](https://s3.amazonaws.com/hubway-data/index.html)

## About the Analysis

### Objectives behind

- Locationwise station details and most and least travelled stations.
- Heatmap of trips for the last four years. 
- Details about trips. How does the usual commute look like? Subscription wise, Gender wise.
- What are the peak hours and peak weekdays? What are the most travelled routes?
- Age groups. Subscription status by age groups.
- Trips taken at late hours, Agewise. 
- Trends of the trips so far. Forecast for the next year.
- Weather factors that might influence the number of trips taken.

## Tableau Dashboard [[link]](https://bit.ly/3VCEDWf)

The Dashboards in this project is built using Tableu Public.

### Screenshots

**Stations**

![Dashboard1](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/stations.png)

**Trips**

![Dashboard2](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/trips.png)

**Trends**

![Dashboard3](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/trends.png)

**By Age**
![Dashboard4](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/by_age.png)

**Forecast for 2023**

![Dashboard5](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/forecast_2023.png)

**Weather influence**

![Dashboard6](https://raw.githubusercontent.com/sarbhanub/blue-bikes-dataanalysis/master/images/dashboards/weatherwise_2022.png)

## Pre-processing

Below are the steps performed using Pandas and NumPy on this notebook file in order to convert the collected data to a managable format which can be used for visualization.

### Analyzing available features

Motivate International, Inc publishes their data each month, so in order to get a time series for 4 years, from Jan, 2019 to Nov, 22 which gives
47 seperate files. [As of 11/06/2022, Nov-22 is the last data published.]

* Merging Data in seperate dataframes **by Year** (2019, 2020, 2021, 2022)
* Extracting the Features from each set.
Note: Since all of the years don't contain same features and sometimes aren't of same data type, processing them seperately was the best way of dealing with features since those features are important.

### Available features:

| Name | Type | Description | Availability |
| :------ | :------- | :----- | :----- |
| `tripduration` | `int` | Duration in seconds | 19, 20, 21, 22 |
| `starttime` | `string` | Date Time in YYYY-MM-DD HH:MM:SS | 19, 20, 21, 22 |
| `stoptime` | `string` | Date Time in YYYY-MM-DD HH:MM:SS | 19, 20, 21, 22 |
| `start station id` | `int` | Start staion terminal id | 19, 20, 21, 22 |
| `start station name` | `string` | Start station name | 19, 20, 21, 22 |
| `start station latitude` | `float` | Latitude | 19, 20, 21, 22 |
| `start station longitude` | `float` | Longitude | 19, 20, 21, 22 |
| `end station id` | `int` | End station terminal ID | 19, 20, 21, 22 |
| `end station name` | `string` | End station name | 19, 20, 21, 22 |
| `end station latitude` | `float` | Latitude | 19, 20, 21, 22 |
| `end station longitude` | `float` | Longitude | 19, 20, 21, 22 |
| `bikeid` | `int` | Bike Number | 19, 20, 21, 22 |
| `usertype` | `int` | Subscriber or Casual | 19, 20 |
| `birth year` | `int` | Year of Birth | 19, 20 |
| `gender` | `int` | Male or Female | 19, 20 |
| `postal code` | `int` | Incomplete information | 20, 21, 22 |


### Cleaning data:

#### Counts

| Year  | Shape |
| :---- | :---- |
| 2019 | `(2522771, 15)` |
| 2020 | `(2073448, 16)` |
| 2021 | `(2934378, 14)` |
| 2022 | `(3614369, 14)` |
| Total | `(11144966, 15)` |

#### Data after Cleaning

###
**Trip Data:**

NB: Files are not included with the repository. Source data files can be downloaded from here. [[link]](https://s3.amazonaws.com/hubway-data/index.html) 
| Name | Type | Description |
| :------ | :------- | :----- |
| `tripduration` | `int` | Duration in seconds |
| `starttime` | `string` | Date Time in YYYY-MM-DD HH:MM:SS |
| `stoptime` | `string` | Date Time in YYYY-MM-DD HH:MM:SS |
| `start station id` | `int` | Start staion terminal id |
| `end station id` | `int` | End station terminal ID |
| `bikeid` | `int` | Bike Number |
| `usertype` | `int` | Subscriber or Casual, [0, 1] |
| `gender` | `int` | NaN, Male, Female [0, 1, 2] |
| `age` | `int` | (birth year - starttime year) |

#### Data Added

After reverse geocoding, station data:

**Station Data:** [[link]](https://github.com/sarbhanub/blue-bikes-dataanalysis/tree/master/tableau/bluebikes_stations.csv)
| Name | Type | Description |
| :------ | :------- | :----- |
| `id` | `int` | Terminal id // Not unique |
| `name` | `string` | name |
| `lat` | `float` | Latitude |
| `long` | `float` | Longitude |
| `street` | `string` | Street name |
| `city` | `string` | Neighbourhood |
| `county` | `string` | County name |
| `state` | `string` | State name (MA) |
| `zip` | `int` | zip code |

#### API Implementation

Here we used Geocodeio's [[link]](https://www.geocod.io/docs/?python#reverse-geocoding) API for python to reverse geocode some of the referenced latitudes and longitudes to fetch geographical information and categorical features about the stations and terminal locations.  
More on the implementation here at **In [88]** of this notebook. [[link]](https://github.com/sarbhanub/blue-bikes-dataanalysis/blob/master/Blue%20Bikes%20Data%20Preprocessing.ipynb)

Unofficial package for Geocodeio Python API from bennylope/pygeocodio by bennylope. [[link]](https://github.com/bennylope/pygeocodio)
```py
  $ pip install pygeocodio
```
Calling the client.

```py
  $ from geocodio import GeocodioClient
```
Adding the Key
```py
  $ client = GeocodioClient('KEY_HERE', timeout=300) #timeout: API conn. timeout
```

| Parameter          | Type     | Description                                      |
| :----------------- | :------- | :----------------------------------------------- |
| `client.reverse()` | `List`   | Python **List** containing Lat Long information. |

Also, added the Weather data for each day from Jan, 2019 to Nov, 2022
Used Visualcrossing's [[link]](https://www.visualcrossing.com/) weather data here.

**Weather Data:** [[link]](https://github.com/sarbhanub/blue-bikes-dataanalysis/tree/master/tableau/weather_history.csv)

| Name | Type | Description |
| :------ | :------- | :----- |
| `datetime` | `string` | Each day from Jan, 2019 to Nov, 2022 |
| `temp` | `float` | Temprature |
| `dew` | `float` | Dew |
| `humidity` | `float` | Humidity |
| `precip` | `float` | Precipitation |
| `precipcover` | `float` | Precipitation cover |
| `preciptype` | `string` | Precipitation type |
| `uvindex` | `int` | UV index |

#### Data removed
- Since Postal code information was incomplete, we were unable to use that information. Instead later we collected the postcode of each station using Geocodeio API.
- Removed the redundant information containing 'start station name', 'start station latitude', 'start station longitude', 'end station name', 'end station latitude', 'end station longitude'.

#### Reduction in Data Size

| Before | After | Reduction |
| :------ | :------- | :------ |
| 2.27 GB | 1.23 GB | ~ 54.2% |