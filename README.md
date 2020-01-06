# global-wines

Analysis on the data of global production of wine.

## Functions Used in Analysis

Function to process the CSV so it can be used for data analysis:

```python
def process_csv(filename):
    exampleFile = open(filename, encoding="utf-8")
    exampleReader = csv.reader(exampleFile)
    exampleData = list(exampleReader)
    exampleFile.close()
    return exampleData
```

Returning the cell of row index and column name from header:

```python
def cell(row_idx, col_name):
    col_idx = csv_header.index(col_name)
    val = csv_data[row_idx][col_idx]
    if val == "":
        return None
    if col_name == 'points':
        return int(val)
    elif col_name == 'price':
        return float(val)
    else:
        return val
 ```

## Data Analysis

#### List of countries in the dataset:

* United States
* Spain
* France
* Italy
* New Zealand
* Bulgaria
* Argentina
* Australia
* Portugal
* Israel
* South Africa
* Greece
* Chile
* Morocco
* Romania
* Germany
* Canada
* Moldova
* Hungary
* Austria
* Croatia
* Slovenia
* India

#### Finding the average points rating for all wines in the dataset:

```python
total = 0
for i in range(len(csv_data)):
    p = cell(i, 'points')
    if p == '':
        continue
    total = total + p
    avg = total / len(csv_data)
avg
```

The average rating for all wines in the dataset is `89.65489673550967`

#### Wineries in South Africa

```python
wineries_in_south_africa = []
for i in range(len(csv_data)):
    country = csv_data[i][0]
    winery = csv_data[i][4]
    if country.lower() == 'south africa':
        if winery not in wineries_in_south_africa:
            wineries_in_south_africa.append(winery)
wineries_in_south_africa
```
Wineries in South Africa:
  * Waterkloof
  * Bouchard Finlayson
  * Vergelegen
  * Fat Barrel
  * Long Neck
  * Graham Beck
  * Essay
  * Noble Hill
  * Robertson Winery
  * Neil Ellis
  * KWV
  
#### Descriptions of Wine Varieties:

I wanted to find the wine varieties in the dataset that contained the phrases 'carmelized', 'lemon-lime soda', and 'cherry-berry'. This could be done by looping through both the variety and description of each wine and then returning the variety where the description contained the phrase. 
  
  img
  
#### Winery that Produced the Highest Priced Wine in the US

```python
us_price = []
us_highest = []
for i in range(len(csv_data)):
    country = csv_data[i][0]
    price = cell(i, 'price')
    winery = csv_data[i][4]
    if price == None:
        continue
    if country == 'US':
        us_price.append(price)
        m_price = max(us_price)
        
for i in range(len(csv_data)):
    price = cell(i, 'price')
    winery = csv_data[i][4]
    if price == None:
        continue
    if country == 'US':
        if price == m_price:
            us_highest.append(winery)
us_highest
```

The `Hall` winery produced the highest priced wine in the United States based on the dataset

#### Highest Rated Wine Varieties in France

```python
france_points = []
france_highest = []
for i in range(len(csv_data)):
    country = csv_data[i][0]
    points = cell(i, 'points')
    variety = csv_data[i][3] 
    if country == 'France':
        france_points.append(points)
        m_points = max(france_points)
        if points == m_points:
            france_highest.append(variety)
france_highest
```
According to the data, the highest rated wines in France were `Provence red blend`, `Tannat`, and `Malbec`

#### Average Points Per Dollar

The average points per dollar of a wine is a good indicator of the value of the wine. 
This code can be used to find the average points per dollar for a winery:

```python
ppd_ponzi = []
for i in range(len(csv_data)):
    winery = csv_data[i][4]
    points = cell(i, 'points')
    price = cell(i, 'price')
    if winery == 'Ponzi':
        ppd_ponzi.append(points/price)
        total = sum(ppd_ponzi)
        avg = total / len(ppd_ponzi)
avg
```

Thus, the average points per dollar of the Ponzi winery is `1.288074888074888` and the average points per dollar of the Blue Farm winery is `1.3628968253968254`.

In order to find the lowest average points per dollar for a winery in a country, the following code can be implemented:

```python
ppd_nz = []
mi_nz = []

for i in range(len(csv_data)):
    winery = csv_data[i][4]
    points = cell(i, 'points')
    price = cell(i, 'price')
    country = csv_data[i][0]
    if country == 'New Zealand':
        ppd_nz.append(points/price) 
        m_nz = min(ppd_nz)
        
for i in range(len(csv_data)):
    winery = csv_data[i][4]
    points = cell(i, 'points')
    price = cell(i, 'price')
    country = csv_data[i][0]
    if country == 'New Zealand':
        if points / price == m_nz and winery not in mi_nz:
            mi_nz.append(winery)
if len(mi_nz) == 1:
    low = mi_nz[0]
else:
    low = mi_nz
low
```

This reveals that the winery with the lowest average ppd in New Zealand is `Kumeu River`, the lowest average ppd for Australia is `D'Arenberg` and `Dalrymple`, and the lowest average for Canada is `Mission Hill`. 

#### Wine Varieties produced by the Global Wines and Quinta Nova de Nossa Senhora do Carmo wineries

The following code produces a list of wines produced by a winery:

```python
g_wines = []
for i in range(len(csv_data)):
    winery = csv_data[i][4]
    variety = csv_data[i][3]
    if winery == 'Global Wines':
        g_wines.append(variety)
        g_wines = list(set(g_wines))
g_wines
```

Wines Produced by Global Wines:
* Portuguese Red
* Portuguese Sparkling
* Touriga Nacional

Wines Produced by Quinta Nova de Nossa Senhora do Carmo
* Portuguese Rosé
* Portuguese Red
* Portuguese White

We can see that 33% of wines produced by Global Wines are also produced by Quinta Nova de Nossa Senhora do Carmo

