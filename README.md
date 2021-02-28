# Analyzing-and-deciding-the-best-property-to-buy-in-and-around-Melbourne
Data Integration and Data Reshaping Integrating multiple files (XML, json, txt, xlsx, pdf, html, dbf, prj, shp, shx) with the appropriate columns mapped to each other, Imputing missing values and cleaning dirty data, Calculating shortest distance from property to nearest train station, shopping centers, hospital and supermarket (using haversine function), Using shape polynomial and point in deciding if a customer resides in that particular suburb, Calculating average time taken from the property to CBD in train, based on all the train trips in GTFS_Melbourne_Train_Information dataset, Direct trains from property to CBD Studying different effects of normalization and transformation methods and comparing the RMSE score of each method. Predicting the price of each property based on how closely it is situated to the nearest hospital, shopping center and CBD, after each columns are: Standardized, Minmax normalized, Log transformed, Power transformed, Box-cox transformed


## Task 1 - Data Integration

#### Introduction:
When someone is looking out to rent/buy a property, he/she looks at different aspects in and around the property to decide if they would want to live there. Collecting all the possible data for a property streamlines the process of narrowing down a property. Few of the aspects one would look into is if the property is located around basic necessities and amenities like:

1. Supermarkets
2. Shopping centers
3. Hospitals
4. Train stations

Now, to extend the information provided for each property, the final result would also include details on which are the nearest supermarkets, shopping centers, hospitals and train stations, whats the average time taken to reach the City (i.e., Flinders Station) through train from the station that is closely situated to each of the properties. If there are direct trains 

Task 1 focuses on integarting data from different sources such as pdfs, txt, xml, json, xlsx, html and shp files into once csv file. Along with this, there are a few logical computations to be done in order to arrive at the desired columns and their values. 

# Supermarket
* To read the pdf I have used the 'pdfminer.six' package. In that, the module named 'extract_text' helps us to read the pdf file into a variable. 
* The read text is then run inside a loop, splitting the entire content on a newline '\n' and the appending it to a list called 'data'. 
* A try and except block is used to check if the data that is split has a number as its first elemnet, if thats true, then append it to the list else do not do anything and go back to the loop

# Hospital id
* To read the data into pandas dataframe, I have used the read_excel function within pandas
* This will read the data into a dataframe format 

# Shopping Center
* To read the 'html' file into a dataframe, pandas.read_html() is used. 
* pd.read_html returns the read dataframe into list of dataframes. 
* Here there is only one element in the list of dataframe and hence 'df_shopping[0]' is used to retrieve the dataframe
* Drop the unwanted column

# Real estate
## There are two types of file here: 'json' and 'xml':
  To read the json file:
      1. Use the pandas.read_json() function which will directly read the json contents to a dataframe

  To read the xml file:

  1. Read it using the 'open()' function in read mode and passing the encoding as 'utf-8'

      - this returns a single string

  2. Eliminate the first and last character of the above string (unwanted characters)
  3. Create a list of list which contains all the column values in it separately as different lists
  4. Converting this lists of list into a dataframe with 10 columns

# GTFS
* Reading just the required files from GTFS to start working on nearest train station ids, travel min to CBD and Transfer flag

# Final dataframe
* Combine json and xml converted dataframe into one dataframe
* Drop any duplicate rows that are present after combing the dataframes
    - 7 rows were dropped

# Haversine function
- Defining a function to calculate the haversine distance between two coordinates

# Distance shopping center
- Converting the latitude and longitude values to float datatype
- Calculate distance of each property location with each of the shopping centers
- Then calculate which is the minimum distance and its indices
- Loop through df_shopping dataframe and use the above mentioned indices to fetch the 'sc_id' of the shopping center
- Update the final dataframe with the new values found

# Station ID
- Converting the latitude and longitude values to float datatype
- Calculate distance of each property location with each of the train centers
- Then calculate which is the minimum distance and its indices
- Loop through df_gtfs_stops dataframe and use the above mentioned indices to fetch the 'Train_station_id'
- Update the final dataframe with the new values found

# Distance to hospital
- Converting the latitude and longitude values to float datatype
- Calculate distance of each property location with each of the hospitals
- Then calculate which is the minimum distance and its indices
- Loop through df_hospitals dataframe and use the above mentioned indices to fetch the 'hospital_id'
- Update the final dataframe with the new values found

# Supermarket
- Converting the latitude and longitude values to float datatype
- Calculate distance of each property location with each of the supermarkets
- Then calculate which is the minimum distance and its indices
- Loop through df_supermarket dataframe and use the above mentioned indices to fetch the 'Supermarket_id'
- Update the final dataframe with the new values found

# Active train service on all weekdays
Check which service id has trains on all the weekdays (Monday-Friday)

# Travel Minutes to CBD
Calculating the average time taken to reach the Flinders Station from each of the station that is nearest to the property.

* Define the time range in which the trains must be monitored 
* For each of the station id nearest to each of the property check:
    - for each station id, filter out all the trip ids that stop in that particular station
    - filter out the dataframe to see the departure times between 7am and 9am
    - on this dataframe, check if all the trips belong to 'T0' service
    - check if each of the trip ids go to Flinders station
        - If they do: take the difference between the times when the trip_id arrived at Flinders station and at what time did the trip_id depart from the nearest station
        - If there is any element in the list, then take the average of that list and make a note of it
        - Make a note of the train station for which the average is being calculated for. 
           * Also, check which all stations are just one stop_sequence before the Flinders station (For Transfer_flag column)
        - If not: continue with the loop

# Suburb
* Read the shapes datafiles into a dataframe
* Create a function to calculate the polygon coordinates and retrieve the suburb name
    - Check if the point object lies within the polygon provided


# Task 2 - Data Reshaping
Observing the effect of different normalization/transformation methods on the “price” ,
“Distance_to_sc”, “travel_min_to_CBD” , and “Distance_to_hospital” attributes. Following are the different methods used:

1. Standardization
2. Minmax normalization 
3. log transformation 
4. power transformation 
5. box-cox transformation





