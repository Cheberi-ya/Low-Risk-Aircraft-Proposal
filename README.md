# Low-Risk-Aircraft-Proposal
 by Linda Temoet
## Proposal To Find The Best Low-Risk Aircraft For The Company's New Aviation Division

### 1. Introduction
I was tasked with preparing a proposal to determine which aircraft are the lowest risk for the company to start a new aviation line. 
I obtained a dataset from the Kaggle that includes aviation accident data from 1962 to 2023 about civil aviation accidents and selected incidents in the United States and international waters.
I decided to filter the data and use data from 2000 to 2023, since it is more current and aircraft technology has evolved from 1962 till then.
### 1.1 Objectives
These are the objectives i hope to achieve with this analysis:

main objective: 
- To do analysis determine the lowest risk aircraft

Other objectives:
1. To calculate the injury severity for different types of aircraft engines.
2. To evaluate the severity of accident injuries for different aircraft types.
3. To assess the Passenger safety of the different aircrafts types.

### 2. Data 
The data was analysed by Python pandas run on a jupyter notebook.
There were 4 main processes done for the data:
1. Data filtering (done on python pandas) where the data was filtered and the unnecessary sections were removed.
2. Data cleaning (done on python pandas) where the data was cleaned and then organised so that all the values for analysis were obtained.
3. Data analysis (done on python pandas) was done which I then obtained relevant data that answers all the objectives.
4. Data visulaization was done using matplotlib and tableau which was then presented to the relevant stakehoders.
The columns I remained with were 21 but I used only 8 to achieve the objectives I set out to do.
### 3. Reccommendation
I then made reccommendations of 3 aircraft models to the business stakeholders showing them the data I had obtained and visualizations.


```python
#importing the relevant Libraries
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
#loading the dataset
df = pd.read_csv('AviationData.csv', encoding='ISO-8859-1', dtype='unicode')
```


```python
#overview of the dataset
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 88889 entries, 0 to 88888
    Data columns (total 31 columns):
     #   Column                  Non-Null Count  Dtype 
    ---  ------                  --------------  ----- 
     0   Event.Id                88889 non-null  object
     1   Investigation.Type      88889 non-null  object
     2   Accident.Number         88889 non-null  object
     3   Event.Date              88889 non-null  object
     4   Location                88837 non-null  object
     5   Country                 88663 non-null  object
     6   Latitude                34382 non-null  object
     7   Longitude               34373 non-null  object
     8   Airport.Code            50249 non-null  object
     9   Airport.Name            52790 non-null  object
     10  Injury.Severity         87889 non-null  object
     11  Aircraft.damage         85695 non-null  object
     12  Aircraft.Category       32287 non-null  object
     13  Registration.Number     87572 non-null  object
     14  Make                    88826 non-null  object
     15  Model                   88797 non-null  object
     16  Amateur.Built           88787 non-null  object
     17  Number.of.Engines       82805 non-null  object
     18  Engine.Type             81812 non-null  object
     19  FAR.Description         32023 non-null  object
     20  Schedule                12582 non-null  object
     21  Purpose.of.flight       82697 non-null  object
     22  Air.carrier             16648 non-null  object
     23  Total.Fatal.Injuries    77488 non-null  object
     24  Total.Serious.Injuries  76379 non-null  object
     25  Total.Minor.Injuries    76956 non-null  object
     26  Total.Uninjured         82977 non-null  object
     27  Weather.Condition       84397 non-null  object
     28  Broad.phase.of.flight   61724 non-null  object
     29  Report.Status           82508 non-null  object
     30  Publication.Date        75118 non-null  object
    dtypes: object(31)
    memory usage: 21.0+ MB
    

The data has 30 columns and 88889 rows. We need data that is more up to data with the current technological advancements. Lets draw a time series graph to see changes over time. 
We start by changing the values of the variables to floats and date to datetime


```python
#Changing event date to datetime
df['Event.Date'] = pd.to_datetime(df['Event.Date'])
```


```python
#Tranforming the columns to integers
df['Total.Fatal.Injuries'] = pd.to_numeric(df['Total.Fatal.Injuries'], downcast='integer', errors='coerce')
df['Total.Serious.Injuries'] = pd.to_numeric(df['Total.Serious.Injuries'], downcast='integer', errors='coerce')
df['Total.Minor.Injuries'] = pd.to_numeric(df['Total.Minor.Injuries'], downcast='integer', errors='coerce')
df['Total.Uninjured'] = pd.to_numeric(df['Total.Uninjured'], downcast='integer', errors='coerce')
```


```python
# changes of uninjured over time
fig,ax = plt.subplots(figsize=(15,8))
ax.plot(df['Event.Date'],df['Total.Uninjured'],color='purple')
ax.set_title('Changes of Uninjured cases over time');
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_7_0.png)
    



```python
#changes of fatalities,serious injuries and minor injuries over time

fig,ax = plt.subplots(figsize=(15,8))
ax.plot(df['Event.Date'],df['Total.Minor.Injuries'],color='purple',label='Total Minor Injuries')
ax.plot(df['Event.Date'],df['Total.Serious.Injuries'],color='Red',label='Total Serious Injuries')
ax.plot(df['Event.Date'],df['Total.Fatal.Injuries'],color='green',label='Total Fatal Injuries')
ax.set_title('Changes in fatalities,Serious and Minor injuries over time')
ax.legend();
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_8_0.png)
    



```python
#filtering the dataset to have only data from 2000-2023
df1 = df.loc[(df['Event.Date'] > '01-01-2000')]
```

We will now proceed to inspect the columns


```python
#inspection of the columns
df1.columns
```




    Index(['Event.Id', 'Investigation.Type', 'Accident.Number', 'Event.Date',
           'Location', 'Country', 'Latitude', 'Longitude', 'Airport.Code',
           'Airport.Name', 'Injury.Severity', 'Aircraft.damage',
           'Aircraft.Category', 'Registration.Number', 'Make', 'Model',
           'Amateur.Built', 'Number.of.Engines', 'Engine.Type', 'FAR.Description',
           'Schedule', 'Purpose.of.flight', 'Air.carrier', 'Total.Fatal.Injuries',
           'Total.Serious.Injuries', 'Total.Minor.Injuries', 'Total.Uninjured',
           'Weather.Condition', 'Broad.phase.of.flight', 'Report.Status',
           'Publication.Date'],
          dtype='object')



Many of the columns in the dataset are irrelevant to our objectives. we will filter them, to remain with the more relevant columns


```python
#removal of the columns we wont need for analysis
df = df1.drop(['Latitude', 'Longitude', 'Airport.Code','Airport.Name','Air.carrier','Publication.Date',
               'Broad.phase.of.flight','FAR.Description','Schedule',],axis=1)
```

The main data we need for analysis remains, we can now check the information left in the dataframe


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Event.Id</th>
      <th>Investigation.Type</th>
      <th>Accident.Number</th>
      <th>Event.Date</th>
      <th>Location</th>
      <th>Country</th>
      <th>Injury.Severity</th>
      <th>Aircraft.damage</th>
      <th>Aircraft.Category</th>
      <th>Registration.Number</th>
      <th>...</th>
      <th>Amateur.Built</th>
      <th>Number.of.Engines</th>
      <th>Engine.Type</th>
      <th>Purpose.of.flight</th>
      <th>Total.Fatal.Injuries</th>
      <th>Total.Serious.Injuries</th>
      <th>Total.Minor.Injuries</th>
      <th>Total.Uninjured</th>
      <th>Weather.Condition</th>
      <th>Report.Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>47677</th>
      <td>20001212X20383</td>
      <td>Accident</td>
      <td>LAX00LA063</td>
      <td>2000-01-02</td>
      <td>VICTORVILLE, CA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>NaN</td>
      <td>N3690L</td>
      <td>...</td>
      <td>No</td>
      <td>1</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>47678</th>
      <td>20001212X20382</td>
      <td>Accident</td>
      <td>LAX00LA062</td>
      <td>2000-01-02</td>
      <td>DOS PALOS, CA</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>NaN</td>
      <td>N7249T</td>
      <td>...</td>
      <td>No</td>
      <td>1</td>
      <td>Reciprocating</td>
      <td>Instructional</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>VMC</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>47679</th>
      <td>20001212X20364</td>
      <td>Accident</td>
      <td>FTW00LA067</td>
      <td>2000-01-02</td>
      <td>CORNING, AR</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>NaN</td>
      <td>N87NF</td>
      <td>...</td>
      <td>No</td>
      <td>1</td>
      <td>Turbo Prop</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>VMC</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>47680</th>
      <td>20001212X20358</td>
      <td>Accident</td>
      <td>FTW00LA057</td>
      <td>2000-01-02</td>
      <td>ODESSA, TX</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>NaN</td>
      <td>N4389W</td>
      <td>...</td>
      <td>No</td>
      <td>1</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>VMC</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>47681</th>
      <td>20001212X20344</td>
      <td>Accident</td>
      <td>DEN00FA037</td>
      <td>2000-01-02</td>
      <td>TELLURIDE, CO</td>
      <td>United States</td>
      <td>Fatal(1)</td>
      <td>Destroyed</td>
      <td>NaN</td>
      <td>N421CF</td>
      <td>...</td>
      <td>No</td>
      <td>2</td>
      <td>Reciprocating</td>
      <td>Personal</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>IMC</td>
      <td>Probable Cause</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>88884</th>
      <td>20221227106491</td>
      <td>Accident</td>
      <td>ERA23LA093</td>
      <td>2022-12-26</td>
      <td>Annapolis, MD</td>
      <td>United States</td>
      <td>Minor</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N1867H</td>
      <td>...</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88885</th>
      <td>20221227106494</td>
      <td>Accident</td>
      <td>ERA23LA095</td>
      <td>2022-12-26</td>
      <td>Hampton, NH</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N2895Z</td>
      <td>...</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88886</th>
      <td>20221227106497</td>
      <td>Accident</td>
      <td>WPR23LA075</td>
      <td>2022-12-26</td>
      <td>Payson, AZ</td>
      <td>United States</td>
      <td>Non-Fatal</td>
      <td>Substantial</td>
      <td>Airplane</td>
      <td>N749PJ</td>
      <td>...</td>
      <td>No</td>
      <td>1</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>VMC</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88887</th>
      <td>20221227106498</td>
      <td>Accident</td>
      <td>WPR23LA076</td>
      <td>2022-12-26</td>
      <td>Morgan, UT</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N210CU</td>
      <td>...</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88888</th>
      <td>20221230106513</td>
      <td>Accident</td>
      <td>ERA23LA097</td>
      <td>2022-12-29</td>
      <td>Athens, GA</td>
      <td>United States</td>
      <td>Minor</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>N9026P</td>
      <td>...</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Personal</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>41212 rows × 22 columns</p>
</div>



We can now go ahead and look at the individual columns. We start by checking the value counts of the Investigation type column


```python
df['Investigation.Type'].value_counts()
```




    Accident    38914
    Incident     2298
    Name: Investigation.Type, dtype: int64



we are not using results for incedents, just for accidents, we can go ahead to remove the incidents.


```python
#Removing rows containing incidents
df = df[df['Investigation.Type'] != 'Incident']
```

We can now start on data cleaning. We start by checking all the values then quantifying the null values.


```python
#checking for null values
df.isna().sum()
```




    Event.Id                      0
    Investigation.Type            0
    Accident.Number               0
    Event.Date                    0
    Location                      9
    Country                      14
    Injury.Severity             327
    Aircraft.damage             884
    Aircraft.Category         12088
    Registration.Number         828
    Make                         28
    Model                        36
    Amateur.Built                56
    Number.of.Engines          3844
    Engine.Type                5828
    Purpose.of.flight          4242
    Total.Fatal.Injuries      10517
    Total.Serious.Injuries    11526
    Total.Minor.Injuries      10966
    Total.Uninjured            5526
    Weather.Condition          3113
    Report.Status              5236
    dtype: int64




```python
#remove the null rows in make, we need these values for analysis, the null values are few so we can drop them
df = df.dropna(subset=['Make'])
```


```python
#remove the null rows in the Injury severity column
df = df.dropna(subset=['Injury.Severity'])
```

Lets look at the Aircraft.category column and remove aircrafts that are not airplanes or helicopters


```python
df['Aircraft.Category'].value_counts()
```




    Airplane             22450
    Helicopter            2970
    Glider                 455
    Balloon                199
    Weight-Shift           161
    Gyrocraft              158
    Powered Parachute       91
    Ultralight              29
    WSFT                     9
    Unknown                  9
    Blimp                    4
    Powered-Lift             3
    ULTR                     1
    Rocket                   1
    Name: Aircraft.Category, dtype: int64




```python
df = df[(df['Aircraft.Category'] == 'Airplane') | (df['Aircraft.Category'] == 'Helicopter' )]
```

We will now look at the numeric columns and fill the missing values to aid in analysis


```python
#looking at the distribution of the numeric columns
fatal_injuries = df['Total.Fatal.Injuries']
fig,ax = plt.subplots()
ax.boxplot(fatal_injuries)
ax.set_ylabel('Distribution');
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_28_0.png)
    



```python
#looking at the distribution of the numeric columns
Serious_injuries = df['Total.Serious.Injuries']
fig,ax = plt.subplots()
ax.boxplot(Serious_injuries)
ax.set_title('Seroius Injuries Distribution')
ax.set_ylabel('Distribution');
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_29_0.png)
    



```python
#looking at the distribution of the numeric columns
minor_injuries = df['Total.Minor.Injuries']
fig,ax = plt.subplots()
ax.boxplot(minor_injuries)
ax.set_title('Minor Injuries distribution')
ax.set_ylabel('Distribution');
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_30_0.png)
    



```python
#looking at the distribution of the numeric columns
uninjured = df['Total.Uninjured']
fig,ax = plt.subplots()
ax.boxplot(uninjured)
ax.set_title('Total uninjured distribution')
ax.set_ylabel('Distribution');
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_31_0.png)
    



```python
#filling the null values in the numeric columns; using median because data is skewed
df['Total.Fatal.Injuries'] = df['Total.Fatal.Injuries'].fillna(df['Total.Fatal.Injuries'].median())
df['Total.Serious.Injuries'] = df['Total.Serious.Injuries'].fillna(df['Total.Serious.Injuries'].median())
df['Total.Minor.Injuries'] = df['Total.Minor.Injuries'].fillna(df['Total.Minor.Injuries'].median())
df['Total.Uninjured'] = df['Total.Uninjured'].fillna(df['Total.Uninjured'].median())
```

Let us look at the dataframe again then going into the individual columns, as per the objectives.


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 25420 entries, 47743 to 88886
    Data columns (total 22 columns):
     #   Column                  Non-Null Count  Dtype         
    ---  ------                  --------------  -----         
     0   Event.Id                25420 non-null  object        
     1   Investigation.Type      25420 non-null  object        
     2   Accident.Number         25420 non-null  object        
     3   Event.Date              25420 non-null  datetime64[ns]
     4   Location                25420 non-null  object        
     5   Country                 25419 non-null  object        
     6   Injury.Severity         25420 non-null  object        
     7   Aircraft.damage         24995 non-null  object        
     8   Aircraft.Category       25420 non-null  object        
     9   Registration.Number     25274 non-null  object        
     10  Make                    25420 non-null  object        
     11  Model                   25409 non-null  object        
     12  Amateur.Built           25413 non-null  object        
     13  Number.of.Engines       23209 non-null  object        
     14  Engine.Type             21631 non-null  object        
     15  Purpose.of.flight       22589 non-null  object        
     16  Total.Fatal.Injuries    25420 non-null  float64       
     17  Total.Serious.Injuries  25420 non-null  float64       
     18  Total.Minor.Injuries    25420 non-null  float64       
     19  Total.Uninjured         25420 non-null  float64       
     20  Weather.Condition       23122 non-null  object        
     21  Report.Status           20842 non-null  object        
    dtypes: datetime64[ns](1), float64(4), object(17)
    memory usage: 4.5+ MB
    


```python
#cleaning the make column
df['Make'] = df['Make'].str.lower()
```


```python
#exporting to CSV
df.to_csv('Aviation_data_cleaned.csv')
```

We can now begin data analysis. We begin by analysing the engine types and how they affect injury severity and passanger safety.


```python
#checking for null values
df['Engine.Type'].isna().sum()
df = df.dropna(subset=['Engine.Type'])#dropping the null values
```


```python
#creating a graph of engine type vs uninjured
uninjured = df['Total.Uninjured']

fig,ax = plt.subplots()
ax.bar(x=df['Engine.Type'],height=uninjured, color='purple')
ax.set_title('Engine type Vs uninjured')
ax.set_xlabel('Engine Type')
ax.set_ylabel('Total Uninjured')
ax.tick_params(axis = 'x' , labelrotation = 45);
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_39_0.png)
    



```python
#creating a graph of engine type Vs total fatalities

fig,ax = plt.subplots()


ax.bar(x=df['Engine.Type'],height=df['Total.Fatal.Injuries'], color='green')
ax.set_title('Engine type Vs fatalities')
ax.tick_params(axis = 'x' , labelrotation = 45);
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_40_0.png)
    



```python
#creating a graph of engine type Vs total Serious injuries
fig,ax = plt.subplots()
ax.bar(x=df['Engine.Type'],height=df['Total.Serious.Injuries'], color='red')
ax.set_title('Engine type Vs Serious injuries')
ax.tick_params(axis='x', labelrotation = 45);
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_41_0.png)
    



```python
#creating a graph of engine type Vs total Minor injuries
fig,ax = plt.subplots()

ax.bar(x=df['Engine.Type'],height=df['Total.Minor.Injuries'], color='purple')
ax.set_title('Engine type Vs Minor injuries')
ax.tick_params(axis = 'x' , labelrotation = 45);
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_42_0.png)
    


According to the graphs, we note that the Turbo fan engine type has the most number of uninjured and not the highest number of fatalities. We can go ahead and filter the data by this engine type.


```python
filtered_df = df[df['Engine.Type'] == 'Turbo Fan']
```

The airplanes have been filtered by the engine type to only Turbo fan. We now filter the make column to show only the make of airplanes with value counts greater than 3


```python
make_value_counts = filtered_df['Make'].value_counts()
df2 = filtered_df[filtered_df['Make'].isin(make_value_counts[make_value_counts > 3].index)]
```


```python
df2['Make'].value_counts()
```




    boeing                           150
    cessna                            78
    airbus                            48
    embraer                           40
    mcdonnell douglas                 22
    bombardier inc                    20
    airbus industrie                  14
    learjet                           14
    bombardier                        13
    aero vodochody                     7
    gulfstream aerospace               7
    israel aircraft industries         7
    gulfstream                         7
    gates learjet corp.                7
    learjet inc                        6
    canadair                           6
    bombardier, inc.                   5
    beech                              5
    embraer-empresa brasileira de      5
    gates lear jet                     4
    raytheon                           4
    embraer s a                        4
    mcdonnell douglas aircraft co      4
    raytheon aircraft company          4
    Name: Make, dtype: int64



The make column names are similar companies but different names. We can go ahead and change the makes to represent one company per make


```python
make_dict = {'airbus industrie': 'airbus',
             'bombardier inc' : 'bombardier',
             'gates learjet corp':'learjet',
             'learjet inc':'learjet',
             'gulfstream aerospace' :'gulfstream',
             'bombardier, inc.':'bombardier',
             'mcdonnell douglas aircraft co': 'mcdonnell douglas',
             'gates lear jet':'learjet', 
             'raytheon aircraft company': 'raytheon',
             'embraer-empresa brasileira de' : 'embraer',
             'embraer s a' : 'embraer' }
df2['Make'] = df2['Make'].replace(to_replace = make_dict)
df2['Make'].value_counts()
```

    




    boeing                        150
    cessna                         78
    airbus                         62
    embraer                        49
    bombardier                     38
    mcdonnell douglas              26
    learjet                        24
    gulfstream                     14
    raytheon                        8
    gates learjet corp.             7
    israel aircraft industries      7
    aero vodochody                  7
    canadair                        6
    beech                           5
    Name: Make, dtype: int64



We can now move to the objective 2: calculating the airplane types vs Injury severity


```python
#plotting a visualization of airplane make Vs injuries
fig,ax = plt.subplots(figsize=(15,8))
ax.barh(y=df2['Make'],
        width=df2['Total.Minor.Injuries'],
        color='purple',
        label='Total Minor Injuries',
        alpha=0.5)
ax.set_title('Plot of Total Minor Injuries according to Airplane make')
ax.set_xlabel('Minor Injuries')
ax.legend();
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_51_0.png)
    



```python
#plotting a visualization of airplane make Vs injuries
fig,ax = plt.subplots(figsize=(15,8))
ax.barh(y=df2['Make'],
        width=df2['Total.Serious.Injuries'],
        color='Red',
        label='Total Serious Injuries',
        alpha=0.5)
ax.set_title('Plot of Total serious Injury according to Airplane make')
ax.set_xlabel('Serious Injuries')
ax.legend();
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_52_0.png)
    



```python
#plotting a visualization of airplane make Vs injuries
fig,ax = plt.subplots(figsize=(15,8))
ax.barh(y=df2['Make'],
        width=df2['Total.Fatal.Injuries'],
        color='green',
        label='Total Fatal Injuries',
        alpha=0.5)
ax.set_title('Plot of Total Fatal Injury according to Airplane make')
ax.set_xlabel('Fatal Injuries')
ax.legend();
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_53_0.png)
    



```python
#plotting a visualization of airplane make Vs uninjured
uninjured = df2['Total.Uninjured']

fig,ax = plt.subplots(figsize=(15,8))
ax.barh(y=df2['Make'],width=uninjured, color='magenta')
ax.set_title('Aircraft Make Vs uninjured')
ax.set_ylabel('Aircraft make')
ax.set_xlabel('Total Uninjured')
ax.tick_params(axis = 'x' , labelrotation = 45);
```


    
![png](https://github.com/Cheberi-ya/Low-Risk-Aircraft-Proposal/blob/main/Images/output_54_0.png)
    



```python
#Looking at the dataframe again
df2.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 481 entries, 47942 to 88158
    Data columns (total 22 columns):
     #   Column                  Non-Null Count  Dtype         
    ---  ------                  --------------  -----         
     0   Event.Id                481 non-null    object        
     1   Investigation.Type      481 non-null    object        
     2   Accident.Number         481 non-null    object        
     3   Event.Date              481 non-null    datetime64[ns]
     4   Location                481 non-null    object        
     5   Country                 481 non-null    object        
     6   Injury.Severity         481 non-null    object        
     7   Aircraft.damage         328 non-null    object        
     8   Aircraft.Category       481 non-null    object        
     9   Registration.Number     480 non-null    object        
     10  Make                    481 non-null    object        
     11  Model                   480 non-null    object        
     12  Amateur.Built           480 non-null    object        
     13  Number.of.Engines       456 non-null    object        
     14  Engine.Type             481 non-null    object        
     15  Purpose.of.flight       146 non-null    object        
     16  Total.Fatal.Injuries    481 non-null    float64       
     17  Total.Serious.Injuries  481 non-null    float64       
     18  Total.Minor.Injuries    481 non-null    float64       
     19  Total.Uninjured         481 non-null    float64       
     20  Weather.Condition       434 non-null    object        
     21  Report.Status           416 non-null    object        
    dtypes: datetime64[ns](1), float64(4), object(17)
    memory usage: 86.4+ KB
    


```python
#exporting the dataframe to then do the Tableau Visualizations
import re

# Define a regex pattern for illegal characters (control characters that Excel doesn't allow)
illegal_chars = re.compile(r'[\x00-\x1F\x7F]')

# Function to clean a string by replacing illegal characters with a space (or any character you prefer)
def replace_illegal_chars(s, replacement=' '):
    if isinstance(s, str):
        return illegal_chars.sub(replacement, s)
    return s

# Apply this function to clean the entire DataFrame
df2_cleaned = df2.applymap(replace_illegal_chars)

# Now export the cleaned DataFrame to Excel
df2_cleaned.to_excel('Aviation_data_cleaned.xlsx')
```

### Reccomendations:
from the data we have done analysis for, here are the three reccomendations for the best aircraft types together with the reasons: 
1. Mc donnell douglas - Has high passanger safety, few injuries and few fatalities.
2. Bombardier - Has high passanger safety and few fatalities and injuries.
3. Embraer - Has high passanger safety and few fatalities and injuries.

The tableau dashboard for this work can be found here https://public.tableau.com/app/profile/linda.temoet/viz/Low-Risk-Aircraft-Proposal-2024-09-LT/LowRiskAircraftProposal?publish=yes

```python

```
