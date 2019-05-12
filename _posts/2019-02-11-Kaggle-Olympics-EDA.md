---
layout: technical-post
title: Project: Olympics dataset (Kaggle) 

---

In this mini-project, I performed a quick Exploratory Data Analysis to learn more about Olympics winners & losers since 1896!


<a id='top'></a>
<img src="logo.jpg" width="338" height="128">

# Analysis on the Olympics dataset

## Datasets
The core datasets used for this task were taken from <a href="https://www.kaggle.com/heesoo37/120-years-of-olympic-history-athletes-and-results">Kaggle</a>. 

This is a well-analysed dataset with over 100 kernels pinned against it on Kaggle. To enrich my analysis, and make it original, I also scraped some other datasets: 

>- I used a dataset of capital city Latitude & Longitudes from <a href="http://techslides.com/list-of-countries-and-capitals">Techslides</a>.
- I manually created a new dataset of olympic host city  Altitudes, Latitudes & Longitudes from <a href="http://www.mapcoordinates.net/en">mapcoordinates</a>.

## Running environment
I used a Jupyter Notebook (Python 3) to narrate my analysis. 



## Download dependencies
ON FIRST RUN: execute the code below to download the geopy dependency. 


```python
import sys
!{sys.executable} -m pip install geopy

```

    Requirement already satisfied: geopy in /Users/williamneedham/anaconda/lib/python3.6/site-packages (1.17.0)
    Requirement already satisfied: geographiclib<2,>=1.49 in /Users/williamneedham/anaconda/lib/python3.6/site-packages (from geopy) (1.49)


## Load packages
The following packages were used for the analysis:



```python
#load packages
import pandas as pd
import numpy as np 
import matplotlib.pyplot as  plt
import seaborn as sns
import unittest
#import difflib - not used
from geopy.distance import vincenty, geodesic

%pylab inline
%matplotlib inline

```

    Populating the interactive namespace from numpy and matplotlib


# Contents

My analysis is laid out as follows: 
#### Stage 1: Initial look at the data

><a href='#Initial'>Link to initial look at the data</a> 


#### Stage 2: Feature engineering
><a href='#Feature'>Link to feature engineering section of the report</a> 


#### Stage 3: Research questions
Following a review of the data available and a brainstorming session, I decided to focus my efforts on the following areas:  

><a href='#Summary'>Summary statistics to explore how athletes have changed over the years </a> 

><a href='#countries'>Analysis of some interesting countries</a> 

><a href='#correlation'>Is there a correlation between Distance Travelled to the Games and performance and has that changed since 1980? </a> 


#### Stage 4: Conclusions

><a href='#conclusion'>Conclusions and further research options </a> 

><a href='#jamaica'>...any finally - what about the Jamaican bobsleigh team?  </a> 


#### Contact details
><a href='#contact'>Contact details </a> 


<a id='Initial'></a>
## Initial look at the data



```python
#Loading athlete data
data = pd.read_csv('athlete_events.csv', index_col = 'ID')
data.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Games</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992 Summer</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012 Summer</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Gunnar Nielsen Aaby</td>
      <td>M</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark</td>
      <td>DEN</td>
      <td>1920 Summer</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Football</td>
      <td>Football Men's Football</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900 Summer</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>21.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988 Winter</td>
      <td>1988</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>21.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988 Winter</td>
      <td>1988</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 1,000 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>25.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1992 Winter</td>
      <td>1992</td>
      <td>Winter</td>
      <td>Albertville</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>25.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1992 Winter</td>
      <td>1992</td>
      <td>Winter</td>
      <td>Albertville</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 1,000 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>27.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1994 Winter</td>
      <td>1994</td>
      <td>Winter</td>
      <td>Lillehammer</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>27.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1994 Winter</td>
      <td>1994</td>
      <td>Winter</td>
      <td>Lillehammer</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 1,000 metres</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



A couple of early observations, upon loading the data: 

> - NA in the Medal column have been converted to NaN, needs correcting. 
- Team variable combines > 1 Country, which is interesting, but unhelpful, will look to merge with the NOC table to clean. 
- It looks like Games is just a concat of Year + Season, should remove, once checked. 
- there's some NaN's in the Height & Weight Column , will need to be cleaned if using these columns. 
- There's some interesting sports from way-back-when - including Tug-Of-War, maybe these would be interesting to look at! 

### Fill NaN values in Medal column with 'None'


```python
#Replace NA in Medal column with None
data['Medal'] = data['Medal'].fillna("None")
data['Medal'].head()
```




    ID
    1    None
    2    None
    3    None
    4    Gold
    5    None
    Name: Medal, dtype: object



### Loading the NOC dataset
I want to use the NOC dataset to get a better representation of where an athlete is from  over 'Team' variable. 


```python
#Loading in the NOC dataset
NOC = pd.read_csv('noc_regions.csv')
```

### String replacement: replacing incorrect/ shorterned country names in the NOC dataset
Later on in the analysis, I do fuzzy matching of Country Names to join with the Capital Cities metadata dataset. After doing this, I changed this part of the script to include the fuzzy matches that the algorithm highlighted. i.e. before it is merged in the main data frame. 


```python
NOC['region'].replace(['US', 'USA', 'UK', 'Saint Kitts', 'Trinidad', 'Boliva', 'Virgin Islands, US', 'Curacao'], 
                      ['United States', 'United States', 'United Kingdom', 'Saint Kitts and Nevis', 
                       'Trinidad and Tobago', 'Bolivia', 'US Virgin Islands', 'Cura̤ao'], 
                      inplace = True)

```


```python
data = pd.merge(data, NOC, left_on='NOC', right_on='NOC')
```


```python
#before discarding, check to see if Games is in Year + Season for all values, then discard. 
if (data['Games'].equals(data['Year'].map(str) + " " + data['Season'])):
    data = data.drop(['notes', 'Games'], axis=1)
else: 
    data = data.drop(['notes'], axis=1)
```


```python
data.sample()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>258795</th>
      <td>Miroslav Cerar</td>
      <td>M</td>
      <td>20.0</td>
      <td>172.0</td>
      <td>73.0</td>
      <td>Yugoslavia</td>
      <td>YUG</td>
      <td>1960</td>
      <td>Summer</td>
      <td>Roma</td>
      <td>Gymnastics</td>
      <td>Gymnastics Men's Parallel Bars</td>
      <td>None</td>
      <td>Serbia</td>
    </tr>
  </tbody>
</table>
</div>



We can see above that Games == Year + Season for all values, and so is removed from the dataset. 

### Review of null values in the dataset


```python
# assign a list of (ColumnName, NullCount) tuples to null_list using list comprehension
null_list = [(col, data[col].isnull().sum()) for col in data.columns]
print(null_list)

```

    [('Name', 0), ('Sex', 0), ('Age', 9462), ('Height', 60083), ('Weight', 62785), ('Team', 0), ('NOC', 0), ('Year', 0), ('Season', 0), ('City', 0), ('Sport', 0), ('Event', 0), ('Medal', 0), ('region', 21)]



```python
df_nulls = pd.DataFrame(null_list)
df_nulls
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Name</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sex</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Age</td>
      <td>9462</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Height</td>
      <td>60083</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Weight</td>
      <td>62785</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Team</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NOC</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Year</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Season</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>City</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Sport</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Event</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Medal</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>region</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>



We can see that only the Age, Height, Weight & the newly created region columns contain null values. My instinct tells me that this is because they didn't record age/height/weight measurements before a certain date. We'll have to investigate the region nulls a bit further.

### Investigating the region nulls
We can see below that these are being caused by the Refugee Olympic Athletes Team, Unknowns and Tuvalu. I will remove these rows from the dataset.  


```python
#find out which points haven't merged properly
missing_region_datapoints = data[data.region.isnull()]
print(len(missing_region_datapoints))
missing_region_datapoints.sample(5)
```

    21





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>270681</th>
      <td>A. Laffen</td>
      <td>M</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>UNK</td>
      <td>1912</td>
      <td>Summer</td>
      <td>Stockholm</td>
      <td>Art Competitions</td>
      <td>Art Competitions Mixed Architecture</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>264747</th>
      <td>Yusra Mardini</td>
      <td>F</td>
      <td>18.0</td>
      <td>157.0</td>
      <td>53.0</td>
      <td>Refugee Olympic Athletes</td>
      <td>ROT</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Swimming</td>
      <td>Swimming Women's 100 metres Butterfly</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>270682</th>
      <td>Logona Esau</td>
      <td>M</td>
      <td>21.0</td>
      <td>163.0</td>
      <td>69.0</td>
      <td>Tuvalu</td>
      <td>TUV</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Lightweight</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>264746</th>
      <td>Yusra Mardini</td>
      <td>F</td>
      <td>18.0</td>
      <td>157.0</td>
      <td>53.0</td>
      <td>Refugee Olympic Athletes</td>
      <td>ROT</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Swimming</td>
      <td>Swimming Women's 100 metres Freestyle</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>264743</th>
      <td>Yonas Kinde</td>
      <td>M</td>
      <td>36.0</td>
      <td>172.0</td>
      <td>57.0</td>
      <td>Refugee Olympic Athletes</td>
      <td>ROT</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Athletics</td>
      <td>Athletics Men's Marathon</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# remove rows with NaN in the region column, using prints to check
print(len(data))
data = data[pd.notnull(data['region'])]
print(len(data))
```

    270767
    270746


<a id='Feature'></a>

# Feature engineering

I chose to use external datasets to enrich the analyse and build novel feature sets. This section shows how I went about it. In this section most of the code is wrapped around unit tests to ensure the transformation have been carried out as expected. 

### Mapping host cities to host countries
Firstly, i created a map of Host Cities to Host Country, which wasn't originally in the dataset. This will be when looking at the 'Home-advantage' hypothesis. 



```python
#manually created a dictionary map from city -> country 
city_to_country = { 'Barcelona':'Spain', 'London':'UK', 'Antwerpen':'Belgium', 'Paris':'France', 'Calgary':'Canada',
       'Albertville':'France', 'Lillehammer':'Norway', 'Los Angeles':'USA', 'Salt Lake City':'USA',
       'Helsinki':'Finland', 'Lake Placid':'USA', 'Sydney':'Australia', 'Atlanta':'USA', 'Stockholm':'Sweden',
       'Sochi':'Russia', 'Nagano':'Japan', 'Torino':'Italy', 'Beijing':'China', 'Rio de Janeiro':'Brazil', 'Athina':'Greece',
       'Squaw Valley':'USA', 'Innsbruck':'Austria', 'Sarajevo': 'Bosnia and Herzegovina', 'Mexico City':'Mexico', 'Munich': 'Germany',
       'Seoul': 'South Korea', 'Berlin': 'Germany', 'Oslo': 'Norway', "Cortina d'Ampezzo":'Italy', 'Melbourne': 'Australia', 'Roma': 'Italy',
       'Amsterdam': 'Netherlands', 'Montreal': 'Canada', 'Moskva': 'Russia', 'Tokyo':'Japan', 'Vancouver':'Canada', 'Grenoble':'France',
       'Sapporo':'Japan', 'Chamonix':'France', 'St. Louis':'USA', 'Sankt Moritz':'Switzerland',
       'Garmisch-Partenkirchen':'Germany'}

#simple dictionary map
def map_cities_to_countries(data, dict_city_to_country) : 
    data['Host_Country'] = data['City'].map(dict_city_to_country)
    return data

# unit test the mapping to ensure the merge did not introduce nulls into the dataset. 
class TestMapping(unittest.TestCase):
    # Create the unit test
    def test_mapping(self):
        map_cities_to_countries(data, city_to_country)
        count_na = data['Medal'].isnull().sum()
        
        # Test that the mapping has not introduces NA's
        self.assertEqual(0, count_na)
        
 # Run the test 
unittest.main(argv=['ignored', '-v'], exit=False)        

```

    test_mapping (__main__.TestMapping) ... ok
    
    ----------------------------------------------------------------------
    Ran 1 test in 0.071s
    
    OK





    <unittest.main.TestProgram at 0x1138f12e8>




```python
#have along at the result - we now have a new variable HostCountry
data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
      <th>Host_Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>None</td>
      <td>China</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abudoureheman</td>
      <td>M</td>
      <td>22.0</td>
      <td>182.0</td>
      <td>75.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2000</td>
      <td>Summer</td>
      <td>Sydney</td>
      <td>Boxing</td>
      <td>Boxing Men's Middleweight</td>
      <td>None</td>
      <td>China</td>
      <td>Australia</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ai Linuer</td>
      <td>M</td>
      <td>25.0</td>
      <td>160.0</td>
      <td>62.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2004</td>
      <td>Summer</td>
      <td>Athina</td>
      <td>Wrestling</td>
      <td>Wrestling Men's Lightweight, Greco-Roman</td>
      <td>None</td>
      <td>China</td>
      <td>Greece</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ai Yanhan</td>
      <td>F</td>
      <td>14.0</td>
      <td>168.0</td>
      <td>54.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Swimming</td>
      <td>Swimming Women's 200 metres Freestyle</td>
      <td>None</td>
      <td>China</td>
      <td>Brazil</td>
    </tr>
  </tbody>
</table>
</div>



### Read in and merge the host city metadata - giving the latitude, longitude, altitude of all the host cities


```python
host_cities_metadata = pd.read_csv('hostcities.csv', names=['HostCity', 'HostLatitude','HostLongitude','HostAltitude'])
host_cities_metadata = host_cities_metadata.iloc[1:]
host_cities_metadata.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HostCity</th>
      <th>HostLatitude</th>
      <th>HostLongitude</th>
      <th>HostAltitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>London</td>
      <td>51.4893335</td>
      <td>-0.14405509</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antwerpen</td>
      <td>51.27704655</td>
      <td>4.54451718</td>
      <td>12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Paris</td>
      <td>48.85881005</td>
      <td>2.32003101</td>
      <td>43</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Calgary</td>
      <td>51.02532675</td>
      <td>-114.0498685</td>
      <td>1043</td>
    </tr>
  </tbody>
</table>
</div>




```python
#merge the host cities metadata into the main data frame
data = pd.merge(data, host_cities_metadata, left_on='City', right_on='HostCity')
data.head(5)

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
      <th>Host_Country</th>
      <th>HostCity</th>
      <th>HostLatitude</th>
      <th>HostLongitude</th>
      <th>HostAltitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Boxing</td>
      <td>Boxing Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Rhythmic Gymnastics</td>
      <td>Rhythmic Gymnastics Women's Individual</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Athletics</td>
      <td>Athletics Men's Hammer Throw</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>



### Check to see in the merge  was performed correctly, without introducing NA's


```python
missing_HostLongitude_datapoints = data[data.HostLongitude.isnull()]
missing_HostLongitude_datapoints
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
      <th>Host_Country</th>
      <th>HostCity</th>
      <th>HostLatitude</th>
      <th>HostLongitude</th>
      <th>HostAltitude</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



The empty dataframe above shows the merge was successful. 

### Read in and merge the athletes capital city metadata
This dataset contains the CountryName, CapitalName, CapitalLatitude, CapitalLongitude, CountryCode (not used) and ContinentName. My plan is merge this dataset on the Athletes 'region' variable, that we originally got from the NOC dataset. I'm doing this in preparation for answering the 'distance-travelled' research question; my hypothesis is that athletes that travel the furthest for the games are most disadvantaged due to jet-lag/ acclimatisation etc. 
**I appreciate using the capital city lat/long of an athletes country is not ideal (as they may not live in the capital, or might live abroad etc), but serves as an OK proxy for this analysis.**



```python
#load in the data
names = ['CountryName', 'CapitalName', 'CapitalLatitude','CapitalLongitude', 'CountryCode', 'ContinentName']
capital_cities_metadata = pd.read_csv('country-capitals.csv', names = names)

#remove the first row as it's a duplicate of the column headers
capital_cities_metadata = capital_cities_metadata.iloc[1:]

# describe method to undertsand the shape of the dataset. 
capital_cities_metadata.describe()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CountryName</th>
      <th>CapitalName</th>
      <th>CapitalLatitude</th>
      <th>CapitalLongitude</th>
      <th>CountryCode</th>
      <th>ContinentName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>245</td>
      <td>241</td>
      <td>245</td>
      <td>245</td>
      <td>242</td>
      <td>245</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>245</td>
      <td>238</td>
      <td>235</td>
      <td>239</td>
      <td>242</td>
      <td>8</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Kiribati</td>
      <td>Jerusalem</td>
      <td>0</td>
      <td>0</td>
      <td>NG</td>
      <td>Europe</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>58</td>
    </tr>
  </tbody>
</table>
</div>



### Merge on capital_cities_metadata. 
Before I merge the capital cities metadata dataset into the main 'data' frame, I'm going to perform an outer join to create a new variable called data_new. In doing so, I can then identify Country names are not joining properly. 
This then becomes a fuzzy matching problem between two lists of strings from different sources. 


```python
#df_pitches = pd.read_csv("df_pitches_2016.csv")
#df_salaries = pd.read_csv("df_salaries_2016.csv")
# Creating my merged data frame
data_new = data.merge(capital_cities_metadata, 
                        left_on='region', 
                        right_on='CountryName', 
                        how='outer', 
                       )
```


```python
# Only selecing pitchers with a lot of pitches
#data_new = data_new[data_new > 1000][['CapitalLongitude']]

#Selecing people with missing salaries
missing_latitude = data_new[data_new.CapitalLatitude.isnull()]

#Displaying results
missing_latitude.reset_index(inplace=True,drop='index')
missing_latitude.sample(40)
missing_latitude['region'].unique()
```




    array(['Ivory Coast', 'Saint Vincent', 'Individual Olympic Athletes',
           'Virgin Islands, British', 'Antigua', 'Gambia', 'Brunei',
           'Micronesia'], dtype=object)



### Fuzzy matching of Country Names from the NOC dataset and the country capitals dataset 
To try and facilitate as clean a merge as possible, I used a fuzzy matching algorithm from the fuzzywuzzy package. This highlighted inconsistencies in the string, which I then dealt with earlier on in the analysis (up the page), then re-ran the script. This was because I needed to make the string replacements prior to merge the NOC & main data frames. 



```python
from fuzzywuzzy import fuzz

def match_name(name, list_names, min_score=0):
    # -1 score incase we don't get any matches
    max_score = -1
    # Returning empty name for no match as well
    max_name = ""
    # Iternating over all names in the other
    for name2 in list_names:
        #Finding fuzzy match score
        score = fuzz.ratio(name, name2)
        # Checking if we are above our threshold and have a better score
        if (score > min_score) & (score > max_score):
            max_name = name2
            max_score = score
    return (max_name, max_score)
```

    /Users/williamneedham/anaconda/lib/python3.6/site-packages/fuzzywuzzy/fuzz.py:11: UserWarning: Using slow pure-python SequenceMatcher. Install python-Levenshtein to remove this warning
      warnings.warn('Using slow pure-python SequenceMatcher. Install python-Levenshtein to remove this warning')



```python
# List for dicts for easy dataframe creation
dict_list = []

# iterating over our players without salaries found above
for name in missing_latitude.region.unique():
    # Use our method to find best match, we can set a threshold here
    match = match_name(name, capital_cities_metadata.CountryName, 50)
    
    # New dict for storing data
    dict_ = {}
    dict_.update({"country_name" : name})
    dict_.update({"match_name" : match[0]})
    dict_.update({"score" : match[1]})
    dict_list.append(dict_)
    
merge_table = pd.DataFrame(dict_list)
# Display results
merge_table
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>country_name</th>
      <th>match_name</th>
      <th>score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ivory Coast</td>
      <td></td>
      <td>-1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saint Vincent</td>
      <td>Saint Martin</td>
      <td>64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Individual Olympic Athletes</td>
      <td></td>
      <td>-1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Virgin Islands, British</td>
      <td>US Virgin Islands</td>
      <td>70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Antigua</td>
      <td>Anguilla</td>
      <td>67</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Gambia</td>
      <td>Zambia</td>
      <td>83</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Brunei</td>
      <td>Burundi</td>
      <td>77</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Micronesia</td>
      <td>Indonesia</td>
      <td>63</td>
    </tr>
  </tbody>
</table>
</div>



The above 'remainder' from the fuzzy matching will be discarded from the dataset (given the time constraints). 


```python
#merge the capital cities metadata into the main data frame
data = pd.merge(data, capital_cities_metadata, left_on='region', right_on='CountryName')
data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>...</th>
      <th>HostCity</th>
      <th>HostLatitude</th>
      <th>HostLongitude</th>
      <th>HostAltitude</th>
      <th>CountryName</th>
      <th>CapitalName</th>
      <th>CapitalLatitude</th>
      <th>CapitalLongitude</th>
      <th>CountryCode</th>
      <th>ContinentName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>Barcelona</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>




```python
# remove rows with NaN in the region column, using prints to check
print(len(data))
data = data[pd.notnull(data.region)]
print(len(data))
```

    270147
    270147


### Calculating how far each athlete has travelled to participate in the games 
Now that we have lat/long pairs of 1) where the athlete lives, and 2) where the games is being held, we can calcuate the Vincenty distance (from <a href="https://geopy.readthedocs.io/en/stable/#module-geopy.distance">GeoPy</a>). 

The Vincenty distance is the geodesic distance between two points on the earth, taking into account the Earth's elliptic shape; using Vincenty’s method.





```python
def calculate_vincenty_distance(distances) : 
    '''
    This function first creates (Latitude, Longitude) tuples for both the Athletes Home City and the Olympic Host City. 
    Then, calculates a new column 'vincenty_distance' using np.vectorize (as the vincenty method requires two inputs)
    
    Inputs: distances - dataframe with cols = ['CapitalLatitude', 'CapitalLongitude', 'HostLatitude', 'HostLongitude']
    Outputs: same dataframe with new column ['vincenty_distance']
    '''
        
    distances['AthleteLatLong'] = list(zip(distances.CapitalLatitude, distances.CapitalLongitude))
    distances['HostCityLatLong'] = list(zip(distances.HostLatitude, distances.HostLongitude))
    distances['vincenty_distance'] = np.vectorize(geodesic)(distances['AthleteLatLong'], distances['HostCityLatLong'])
    
    return distances

# create a new dataframe away from the original data, and then concat back into the original dataframe
new_data = data
distances = new_data.ix[:, ['CapitalLongitude', 'CapitalLatitude','HostLongitude','HostLatitude']]

#normally I'd one class for all unit tests, but I've created a new one here for code readability in a notebook. 
class TestCalculations(unittest.TestCase):
    # Create the unit test
    def test_vincenty_calculation(self):
        #create a test tuple of data from the first row of 'data' dataframe
        athleteLatLong0 = (39.91666667, 116.383333) # first row
        
        #create a test tuple of data from the first row of 'data' dataframe
        hostLatLong0 = (41.3828939, 2.1774322) 
        
        #calculate the vincenty distance on the test data
        distanceTest = geodesic(athleteLatLong0, hostLatLong0).km
        
        #calculate vincenty distance using our function calculate_vincenty_distance
        calculate_vincenty_distance(distances)
        
        # get the result of the first row
        function_output = distances.loc[0,'vincenty_distance']
        
        # Test that the result of the two method asserts equal. 
        self.assertAlmostEqual(function_output, distanceTest,5)
        
 # Run the test 
unittest.main(argv=['ignored', '-v'], exit=False)        

```

    test_vincenty_calculation (__main__.TestCalculations) ... ok
    test_mapping (__main__.TestMapping) ... ok
    
    ----------------------------------------------------------------------
    Ran 2 tests in 88.062s
    
    OK





    <unittest.main.TestProgram at 0x103e642e8>



### Merge the new vincenty_distance back into main dataframe


```python
#cast the vincenty distance from Distance ojbect to float 
distances['vincenty_distance'] = distances['vincenty_distance'].astype(str).str[:-3].astype(float)

data = pd.concat([data, distances['vincenty_distance']], axis=1)

#take a look at the results
data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>...</th>
      <th>HostLatitude</th>
      <th>HostLongitude</th>
      <th>HostAltitude</th>
      <th>CountryName</th>
      <th>CapitalName</th>
      <th>CapitalLatitude</th>
      <th>CapitalLongitude</th>
      <th>CountryCode</th>
      <th>ContinentName</th>
      <th>vincenty_distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
      <td>8822.807943</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
      <td>8822.807943</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
      <td>8822.807943</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
      <td>8822.807943</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>...</td>
      <td>41.3828939</td>
      <td>2.1774322</td>
      <td>32</td>
      <td>China</td>
      <td>Beijing</td>
      <td>39.91666667</td>
      <td>116.383333</td>
      <td>CN</td>
      <td>Asia</td>
      <td>8822.807943</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>



### Tidy up the resulting dataframe to remove columns we don't need


```python
# see what columns we now have 
data.columns
```




    Index(['Name', 'Sex', 'Age', 'Height', 'Weight', 'Team', 'NOC', 'Year',
           'Season', 'City', 'Sport', 'Event', 'Medal', 'region', 'Host_Country',
           'HostCity', 'HostLatitude', 'HostLongitude', 'HostAltitude',
           'CountryName', 'CapitalName', 'CapitalLatitude', 'CapitalLongitude',
           'CountryCode', 'ContinentName', 'vincenty_distance'],
          dtype='object')




```python
cols_to_keep = ['Name', 'Sex', 'Age', 'Team', 'Height', 'Weight', 'Year', 'Season',
       'City', 'Sport', 'Event', 'Medal', 'region', 'Host_Country', 
                'CapitalName', 'ContinentName', 'vincenty_distance', 'HostAltitude']

clean_data = pd.DataFrame(data, columns=cols_to_keep)
clean_data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Team</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>region</th>
      <th>Host_Country</th>
      <th>CapitalName</th>
      <th>ContinentName</th>
      <th>vincenty_distance</th>
      <th>HostAltitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>China</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>China</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Boxing</td>
      <td>Boxing Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>China</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Rhythmic Gymnastics</td>
      <td>Rhythmic Gymnastics Women's Individual</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>China</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Athletics</td>
      <td>Athletics Men's Hammer Throw</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>China</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>



### Re-name some of the columns to make it obvious what they represent


```python
clean_data.columns = ['Name', 'Sex', 'Age', 'Team', 'Height', 'Weight', 'Year', 'Season',
       'City', 'Sport', 'Event', 'Medal', 'AthleteCountry', 'HostCountry', 
                'AthletesHomeCapital', 'AthletesHomeContinent', 'DistanceTravelled', 'HostCityAltitude']

clean_data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Team</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>AthleteCountry</th>
      <th>HostCountry</th>
      <th>AthletesHomeCapital</th>
      <th>AthletesHomeContinent</th>
      <th>DistanceTravelled</th>
      <th>HostCityAltitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>China</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>China</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Boxing</td>
      <td>Boxing Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>China</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Rhythmic Gymnastics</td>
      <td>Rhythmic Gymnastics Women's Individual</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>China</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Athletics</td>
      <td>Athletics Men's Hammer Throw</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>China</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>



### One-hot encode the Medal variable to make later analysis easier


```python
df_medals = pd.get_dummies(clean_data['Medal'])

# Join the dummy variables to the main dataframe
clean_data = pd.concat([clean_data, df_medals], axis=1)
pd.options.display.max_columns = None
clean_data.head()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Team</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>AthleteCountry</th>
      <th>HostCountry</th>
      <th>AthletesHomeCapital</th>
      <th>AthletesHomeContinent</th>
      <th>DistanceTravelled</th>
      <th>HostCityAltitude</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>None</th>
      <th>Silver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>China</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bai Chongguang</td>
      <td>M</td>
      <td>21.0</td>
      <td>China</td>
      <td>184.0</td>
      <td>83.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Boxing</td>
      <td>Boxing Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bai Mei</td>
      <td>F</td>
      <td>17.0</td>
      <td>China</td>
      <td>166.0</td>
      <td>46.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Rhythmic Gymnastics</td>
      <td>Rhythmic Gymnastics Women's Individual</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bi Zhong</td>
      <td>M</td>
      <td>23.0</td>
      <td>China</td>
      <td>188.0</td>
      <td>110.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Athletics</td>
      <td>Athletics Men's Hammer Throw</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cai Yanshu</td>
      <td>M</td>
      <td>28.0</td>
      <td>China</td>
      <td>169.0</td>
      <td>79.0</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Light-Heavyweight</td>
      <td>None</td>
      <td>China</td>
      <td>Spain</td>
      <td>Beijing</td>
      <td>Asia</td>
      <td>8822.807943</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Add binary  AnyMedals? variable


```python
clean_data['TotalMedals'] = clean_data['Gold'] + clean_data['Silver'] + clean_data['Bronze']

clean_data['AnyMedals'] = np.where(clean_data.loc[:,'Medal'] == 'None', 0, 1)
clean_data.sample(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Team</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>AthleteCountry</th>
      <th>HostCountry</th>
      <th>AthletesHomeCapital</th>
      <th>AthletesHomeContinent</th>
      <th>DistanceTravelled</th>
      <th>HostCityAltitude</th>
      <th>Bronze</th>
      <th>Gold</th>
      <th>None</th>
      <th>Silver</th>
      <th>TotalMedals</th>
      <th>AnyMedals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>268023</th>
      <td>Darko Majstorovi</td>
      <td>M</td>
      <td>30.0</td>
      <td>Yugoslavia</td>
      <td>189.0</td>
      <td>85.0</td>
      <td>1976</td>
      <td>Summer</td>
      <td>Montreal</td>
      <td>Rowing</td>
      <td>Rowing Men's Double Sculls</td>
      <td>None</td>
      <td>Serbia</td>
      <td>Canada</td>
      <td>Belgrade</td>
      <td>Europe</td>
      <td>6925.483129</td>
      <td>56</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27524</th>
      <td>George Charles Calnan</td>
      <td>M</td>
      <td>24.0</td>
      <td>United States</td>
      <td>183.0</td>
      <td>NaN</td>
      <td>1924</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Fencing</td>
      <td>Fencing Men's epee, Individual</td>
      <td>None</td>
      <td>United States</td>
      <td>France</td>
      <td>Washington  D.C.</td>
      <td>North America</td>
      <td>6177.680465</td>
      <td>43</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>82598</th>
      <td>Giuseppe Lupi</td>
      <td>M</td>
      <td>33.0</td>
      <td>Italy</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1928</td>
      <td>Summer</td>
      <td>Amsterdam</td>
      <td>Gymnastics</td>
      <td>Gymnastics Men's Team All-Around</td>
      <td>None</td>
      <td>Italy</td>
      <td>Netherlands</td>
      <td>Rome</td>
      <td>Europe</td>
      <td>1297.207864</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>248717</th>
      <td>Jin O-Hyeon</td>
      <td>M</td>
      <td>24.0</td>
      <td>South Korea</td>
      <td>163.0</td>
      <td>67.0</td>
      <td>1960</td>
      <td>Summer</td>
      <td>Roma</td>
      <td>Weightlifting</td>
      <td>Weightlifting Men's Lightweight</td>
      <td>None</td>
      <td>South Korea</td>
      <td>Italy</td>
      <td>Seoul</td>
      <td>Asia</td>
      <td>8990.813118</td>
      <td>21</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>249859</th>
      <td>Chainarong Sophonpong</td>
      <td>M</td>
      <td>20.0</td>
      <td>Thailand</td>
      <td>166.0</td>
      <td>62.0</td>
      <td>1964</td>
      <td>Summer</td>
      <td>Tokyo</td>
      <td>Cycling</td>
      <td>Cycling Men's 100 kilometres Team Time Trial</td>
      <td>None</td>
      <td>Thailand</td>
      <td>Japan</td>
      <td>Bangkok</td>
      <td>Asia</td>
      <td>4608.433126</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>147164</th>
      <td>Makrem Ayed</td>
      <td>M</td>
      <td>22.0</td>
      <td>Tunisia</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>None</td>
      <td>Tunisia</td>
      <td>USA</td>
      <td>Tunis</td>
      <td>Africa</td>
      <td>8219.461425</td>
      <td>311</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>248254</th>
      <td>Park Dong-Ju</td>
      <td>M</td>
      <td>25.0</td>
      <td>South Korea</td>
      <td>173.0</td>
      <td>63.0</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Equestrianism</td>
      <td>Equestrianism Mixed Three-Day Event, Team</td>
      <td>None</td>
      <td>South Korea</td>
      <td>South Korea</td>
      <td>Seoul</td>
      <td>Asia</td>
      <td>1.904024</td>
      <td>40</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>60895</th>
      <td>Alain Bouffard</td>
      <td>M</td>
      <td>25.0</td>
      <td>France</td>
      <td>165.0</td>
      <td>52.0</td>
      <td>1964</td>
      <td>Summer</td>
      <td>Tokyo</td>
      <td>Rowing</td>
      <td>Rowing Men's Coxed Eights</td>
      <td>None</td>
      <td>France</td>
      <td>Japan</td>
      <td>Paris</td>
      <td>Europe</td>
      <td>9738.883220</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>197450</th>
      <td>Alfred Edward Flaxman</td>
      <td>M</td>
      <td>28.0</td>
      <td>Great Britain</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1908</td>
      <td>Summer</td>
      <td>London</td>
      <td>Athletics</td>
      <td>Athletics Men's Standing High Jump</td>
      <td>None</td>
      <td>United Kingdom</td>
      <td>UK</td>
      <td>London</td>
      <td>Europe</td>
      <td>4.380864</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>185309</th>
      <td>Thierry Vatrican</td>
      <td>M</td>
      <td>20.0</td>
      <td>Monaco</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Judo</td>
      <td>Judo Men's Half-Middleweight</td>
      <td>None</td>
      <td>Monaco</td>
      <td>USA</td>
      <td>Monaco</td>
      <td>Europe</td>
      <td>7643.984453</td>
      <td>311</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




<a id='Summary'></a>

# ======================================================
# Summary statistics to explore how athletes have changed over the years 
In this question, i'm looking to explore how the sex, age, and number of competitors has changed in the last 120 years. 

### The changing profile of the Olympic athlete (part 1)



```python
#Proportion 

ProportionMvW = data.groupby(['Year', 'Sex']).agg({'Name': 'count'})

percentages = ProportionMvW.groupby(level=0).apply(lambda x:
                                                 100 * x / float(x.sum()))
percentages = percentages.reset_index()
```


```python
a4_dims = (11.7, 8.27)
fig, ax = pyplot.subplots(figsize=a4_dims)

ax = sns.barplot(x="Year", y="Name", hue="Sex", data=percentages, palette="Set2") 
ax.set_xticklabels(ax.get_xticklabels(), rotation=90, ha="right")
ax.set_title('Proportion of men and women competing in the olympic games')
ax.set_ylabel('Percentage of athletes for each gender')
ax.legend(loc="upper right")
plt.show()
```


![png](output_57_0.png)


*Insights:* 
- Good news! The ratio of male to female competitors is slowly becoming more balanced - could 2020 be the year that we have an equal number of female competitors and male competitors? 

### The changing profile of the Olympic athlete (part 2)
The visualisation below shows how the distribution of men and women's ages have changed since 1896, split by Sex and Summer/Winter Olympics


```python
sns.set(rc={'figure.figsize':(20,20)})

g = sns.FacetGrid(clean_data, col="Season",  row="Sex", size=6)
g = g.map(sns.boxplot, "Year", 'Age')
g.set_xticklabels(ax.get_xticklabels(), rotation=90, ha="right")


show()
```


![png](output_59_0.png)


*Insights:* 
- some 'athletes' in the 20th century were > 80 years when they competed! Worth a quick look back at the data to see what they competed in. 
- As could be expected, variance is greatly reduced in the modern athletes age (with the median Age being in the early 20's. Interestingly the median age in the winter olympics seems to be higher. 
- Higher variance in the Summer Olympics - I would hypothesise this is down to the wider variety of sports on offer at the Summer Olympics. 

<a id='countries'></a>


# Analysis of some interesting countries




```python
#create a table of year,countryname & sex to be merged into a count of each later. 
mini_country_table = clean_data[clean_data['Season'] == 'Summer'].loc[:, ['Year','AthleteCountry', 'Name', 'Sex']].drop_duplicates()
mini_country_table.head()

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>AthleteCountry</th>
      <th>Name</th>
      <th>Sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1992</td>
      <td>China</td>
      <td>A Dijiang</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1992</td>
      <td>China</td>
      <td>Bai Chongguang</td>
      <td>M</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1992</td>
      <td>China</td>
      <td>Bai Mei</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1992</td>
      <td>China</td>
      <td>Bi Zhong</td>
      <td>M</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1992</td>
      <td>China</td>
      <td>Cai Yanshu</td>
      <td>M</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a pivot table to count gender wise representation of each team in each year
CountAthletesByCountry = pd.pivot_table(mini_country_table,
                                        index = ['Year', 'AthleteCountry'],
                                        columns = 'Sex',
                                        aggfunc = 'count').reset_index()

# rename columns as per column names in the 0th level
CountAthletesByCountry.columns = CountAthletesByCountry.columns.get_level_values(0)

# rename the columns appropriately
CountAthletesByCountry.columns = ['Year', 'AthleteCountry', 'Female_Athletes', 'Male_Athletes']

# get total athletes per team-year
CountAthletesByCountry['Total_Athletes'] = CountAthletesByCountry['Female_Athletes'] + \
CountAthletesByCountry['Male_Athletes']

uk_athletes = CountAthletesByCountry[CountAthletesByCountry['AthleteCountry'] == "United Kingdom"]
uk_athletes.fillna(0, inplace = True)
uk_athletes.set_index('Year', inplace = True)

swedish_athletes = CountAthletesByCountry[CountAthletesByCountry['AthleteCountry'] == "Sweden"]
swedish_athletes.set_index('Year', inplace = True)

japanese_athletes = CountAthletesByCountry[CountAthletesByCountry['AthleteCountry'] == "Japan"]
japanese_athletes.set_index('Year', inplace = True)

germany_athletes = CountAthletesByCountry[CountAthletesByCountry['AthleteCountry'] == "Germany"]
germany_athletes.set_index('Year', inplace = True)

# Plot the values of male, female and total athletes using bar charts and the line charts.
fig, ((ax1, ax2), (ax3, ax4)) = subplots(nrows = 2, ncols = 2, figsize = (20, 20), sharey = True)
fig.subplots_adjust(hspace = 0.3)

# Plot team Australia's contingent size
ax1.bar(uk_athletes.index.values, uk_athletes['Male_Athletes'], width = -1, align = 'edge', label = 'Male Athletes')
ax1.bar(uk_athletes.index.values, uk_athletes['Female_Athletes'], width = 1, align = 'edge', label = 'Female Athletes')
ax1.plot(uk_athletes.index.values, uk_athletes['Total_Athletes'], linestyle = ':', color = 'black', label = 'Total Athletes',
        marker = 'o')
ax1.legend(loc="upper right")
ax1.set_title('UK Athletes :\nParticipation since 1896')
ax1.set_ylabel('Count of Athletes')

# Plot German athlete participation
ax2.bar(germany_athletes.index.values, germany_athletes['Male_Athletes'], width = -1, align = 'edge', label = 'Male Athletes')
ax2.bar(germany_athletes.index.values, germany_athletes['Female_Athletes'], width = 1, align = 'edge', label = 'Female Athletes')
ax2.plot(germany_athletes.index.values, germany_athletes['Total_Athletes'], linestyle = ':', color = 'black', label = 'Total Athletes',
        marker = 'o')
ax2.set_title('Germany Athletes :\nParticipation since 1896')
ax2.legend(loc="upper left")

ax2.set_ylabel('Count of Athletes')

# Plot Japan's contingent size
ax3.bar(japanese_athletes.index.values, japanese_athletes['Male_Athletes'], width = -1, align = 'edge', label = 'Male Athletes')
ax3.bar(japanese_athletes.index.values, japanese_athletes['Female_Athletes'], width = 1, align = 'edge', label = 'Female Athletes')
ax3.plot(japanese_athletes.index.values, japanese_athletes['Total_Athletes'], linestyle = ':', color = 'black', label = 'Total Athletes', 
         marker = 'o')
ax3.set_title('Japanese Athletes :\nParticipation since 1896')
ax3.set_ylabel('Count of Athletes')
ax3.legend(loc="upper right")


# Plot team Swedens's contingent size
ax4.bar(swedish_athletes.index.values, swedish_athletes['Male_Athletes'], width = -1, align = 'edge', label = 'Male Athletes')
ax4.bar(swedish_athletes.index.values, swedish_athletes['Female_Athletes'], width = 1, align = 'edge', label = 'Female Athletes')
ax4.plot(swedish_athletes.index.values, swedish_athletes['Total_Athletes'], linestyle = ':', color = 'black', label = 'Total Athletes',
        marker = 'o')
ax4.set_title('Swedish Athletes :\nParticipation since 1896')
ax4.set_ylabel('Count of Athletes')
ax4.legend(loc="upper left")

show()

```

    /Users/williamneedham/anaconda/lib/python3.6/site-packages/pandas/core/frame.py:2842: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      downcast=downcast, **kwargs)



![png](output_63_1.png)


*Insights:* 
- Overall gender balance has been improving slowing since the mid 20th century and it nearly equal for all countries considered. 
- Swedish female participation has risen above male participation in the last 3 Olympic Games. 
- UK particpation peaked in 2012 - when it hosted in London! This peak trend was mirrored for German participation in the 1972 Games, held in West Germany.  
- 1916, 1940 and 1944 games cancelled due to World Wars.  



<a id='correlation'></a>

# Is there a correlation between Distance Travelled to the Games and performance and has that changed since 1980? 
This is where we bring in the engineered feature 'DistanceTravelled' - the Vincenty distance between an athletes home capital and where the games took place. My hypothesis is that distance travelled has an impact of number of medals a Country receives (due to things physiological factors like jet-mind, blood flow etc), I also further hypothesise that the effect of this will be reduced in the modern era (I've chosen Pre/Post 1980). 

To investigate these hypothesese, I've split the dataset (pre and post 1980) and calculate correlation between this distance and medals won. Obviously, whether a country has a high medal count is a multivariate problem and would require a more in-depth analysis in practice. 


```python
# Get just the medalists 
Subset_Medalists = clean_data['AnyMedals'] == 1

#Create a new dataset to be merged with the medal table dataset below 
subset_distancetravelled = clean_data.loc[:, ['Year', 'AthleteCountry', 'DistanceTravelled']].drop_duplicates()

medal_tally = clean_data.groupby(['Year','AthleteCountry'])['TotalMedals'].agg('sum').reset_index()
medal_tally_byDistance = medal_tally.merge(subset_distancetravelled,
                                   left_on = ['Year', 'AthleteCountry'],
                                   right_on = ['Year', 'AthleteCountry'],
                                   how = 'left')

#create two datasets to compare correlations
Pre_1980_MedalTallyByDistance = medal_tally_byDistance[medal_tally_byDistance['Year']<=1980]
Post_1980_MedalTallyByDistance = medal_tally_byDistance[medal_tally_byDistance['Year']>1980]

medal_tally_byDistance.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>AthleteCountry</th>
      <th>TotalMedals</th>
      <th>DistanceTravelled</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1896</td>
      <td>Australia</td>
      <td>3.0</td>
      <td>15201.660597</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1896</td>
      <td>Austria</td>
      <td>5.0</td>
      <td>1282.025503</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1896</td>
      <td>Denmark</td>
      <td>6.0</td>
      <td>2135.520896</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1896</td>
      <td>France</td>
      <td>11.0</td>
      <td>2100.783748</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1896</td>
      <td>Germany</td>
      <td>32.0</td>
      <td>1802.949137</td>
    </tr>
  </tbody>
</table>
</div>



### Pre-1980: impact of distance travelled on country performance


```python
selected_rows = Pre_1980_MedalTallyByDistance['TotalMedals'] > 0

correlation = Pre_1980_MedalTallyByDistance.loc[selected_rows, ['DistanceTravelled', 'TotalMedals']].corr()['TotalMedals'][0]

plot(Pre_1980_MedalTallyByDistance.loc[selected_rows, 'DistanceTravelled'], 
     Pre_1980_MedalTallyByDistance.loc[selected_rows, 'TotalMedals'] , 
     linestyle = 'none', 
     marker = 'o',
     alpha = 0.4)
xlabel('Distance Travelled')

ylabel('Number of Medals')
title('PRE-1980 - Distance travelled versus medal tally')
text(np.nanpercentile(Pre_1980_MedalTallyByDistance['DistanceTravelled'], 99.6), 
     max(Pre_1980_MedalTallyByDistance['TotalMedals']) - 50,
     "Correlation = " + str(correlation))
```




    Text(18557.7,446,'Correlation = -0.146533766948')




![png](output_67_1.png)


### Post-1980: impact of distance travelled on country performance


```python
selected_rows = Post_1980_MedalTallyByDistance['TotalMedals'] > 0

correlation = Post_1980_MedalTallyByDistance.loc[selected_rows, ['DistanceTravelled', 'TotalMedals']].corr()['TotalMedals'][0]

plot(Post_1980_MedalTallyByDistance.loc[selected_rows, 'DistanceTravelled'], 
     Post_1980_MedalTallyByDistance.loc[selected_rows, 'TotalMedals'] , 
     linestyle = 'none', 
     marker = 'o', 
    alpha = 0.4)
xlabel('Distance Travelled')

ylabel('Number of Medals')
title('POST-1960 - Distance travelled versus medal tally')
text(np.nanpercentile(Post_1980_MedalTallyByDistance['DistanceTravelled'], 99.6), 
     max(Post_1980_MedalTallyByDistance['TotalMedals']) - 50,
     "Correlation = " + str(correlation))
```




    Text(18738.3,316,'Correlation = -0.0614926627557')




![png](output_69_1.png)


*Insights:*
- negative correlations recorded for post the Pre-1980 dataset and Post-1980 dataset, meaning a higher distance travelled does lead to lower medal haul. 
- The correlation coefficient for pre-1980 was 'Correlation = -0.146533766948' and for Post-1980 was 'Correlation = -0.0614926627557'. This tells us that my hypothesis was correct and that this Distance Travelled effect has less of an effect in the modern era (where flights are cheap/ more comfortable and athletes probably spend time preparing in the country of the games beforehand). 
- Admittedly though, the coefficients are small and as such the DistanceTravelled-effect is smaller than I thought it would be. 

<a id='conclusion'></a>

# Conclusions and further research options 
This was an enjoyable task! I focused my efforts on data engineering and manfuactures some novel features. The following insights were of most interest to me: 
- good to see gender balance finally becoming a reality (after over 120 years of competition! 
- following high variance in the early years, most athletes are in there early 20's (slightly higher median age for the Winter Olympics). 
- of all interesting countries selected for this study (UK, Germany, Sweden & Japan), Germany send the most athletes (on average each year), 


Given more time, I would like to:
- add more summary statistics, 
- explore covariance, 
- build predictive models (i.e. predicting the medals table at the next Games), 
- conduct a clustering analysis to so what separates the best athletes from OK athletes. 



<a id='jamaica'></a>

# ...any finally - what about the Jamaican bobsleigh team?
It wouldn't be a proper Olympic analysis without a look at how the Jamaican bobsleigh team have done over the years



```python
#df[(df['coverage']  > 50) & (df['reports'] < 4)]
jamaicans = clean_data[(clean_data['AthleteCountry'] == 'Jamaica') & (clean_data['Sport'] == 'Bobsleigh') ]
jamaican_grouped = jamaicans.groupby(['Year','City'])['TotalMedals'].agg('sum').reset_index()
jamaican_grouped
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>City</th>
      <th>TotalMedals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1988</td>
      <td>Calgary</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1992</td>
      <td>Albertville</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1994</td>
      <td>Lillehammer</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1998</td>
      <td>Nagano</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2002</td>
      <td>Salt Lake City</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2014</td>
      <td>Sochi</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_data = {'Year': [2018, 2022], 'City': ["Pyeongchang", "Beijing"], 'TotalMedals': [0, NaN]}
future_results = pd.DataFrame(new_data)
combined = pd.concat([jamaican_grouped, future_results])
combined
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>TotalMedals</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Calgary</td>
      <td>0.0</td>
      <td>1988</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albertville</td>
      <td>0.0</td>
      <td>1992</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lillehammer</td>
      <td>0.0</td>
      <td>1994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nagano</td>
      <td>0.0</td>
      <td>1998</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Salt Lake City</td>
      <td>0.0</td>
      <td>2002</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Sochi</td>
      <td>0.0</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Pyeongchang</td>
      <td>0.0</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Beijing</td>
      <td>NaN</td>
      <td>2022</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.bar(combined['Year'],combined['TotalMedals'] )
fig.suptitle('test title', fontsize=20)
plt.xlabel('Olympic Year', fontsize=18)
plt.ylabel('Number of medals won', fontsize=16)
plt.ylim((0,10))
plt.xticks(combined['Year'])
```




    ([<matplotlib.axis.XTick at 0x111dbfcf8>,
      <matplotlib.axis.XTick at 0x111b19710>,
      <matplotlib.axis.XTick at 0x111b199e8>,
      <matplotlib.axis.XTick at 0x111dfbe48>,
      <matplotlib.axis.XTick at 0x1185083c8>,
      <matplotlib.axis.XTick at 0x118508908>,
      <matplotlib.axis.XTick at 0x118508e48>,
      <matplotlib.axis.XTick at 0x11850e3c8>],
     <a list of 8 Text xticklabel objects>)




![png](output_75_1.png)


## Unfortunately, no medals yet for the Jamaican's in the Bobsleigh, maybe 2022 is their year? 
<img src="jamaica.jpg" width="500">




Hope you enjoyed reading! 


# ------------- > <a href='#top'>Back to Top </a>  < ------------- 

