# Snowfall-Data-Analysis-and-Visualization
In this project, we aim to do a comparative analysis of the historical patterns of snowfall data using big data technologies and visualize it using ArcGIS. This will help to get meaningful insights from this massive data and predict the snowfall pattern for which we use ML algorithms.

## Overview of the components

<img width="644" alt="Screenshot 2023-06-19 at 11 01 57 AM" src="https://github.com/Shadhrush5/Snowfall-Data-Analysis-and-Visualization/assets/119898772/3f2c2d99-fd91-44b9-9bf3-526b6b5946b6">

## Dataset

### Description

We have used the NOAA Global Surface dataset, it’s an ISH (Integrated Surface Hourly) based data where USAF Climatology Center provides it. This dataset contains data from 9000 stations around the globe. Few main entities available in this dataset are Mean Dew Point, Mean Sea Level, Mean Station Pressure, Mean Visibility, Mean Wind speed, Minimum Temperature, Precipitation Amount, Snow Depth, Elevation, Begin. We have used the data from 2001-2019.
We are using attributes such as temperature, wind speed, mean dew point, precipitation and weather station location for our application.
The step involved in merging the data reported from different weather stations to its location specific to latitude and longitude is using inner merge.

1. https://www.kaggle.com/datasets/noaa/gsod
2. https://www.kaggle.com/code/ircuscus/global-weather-monitoring-dashboard/data

### Pre-processing and cleaning

The Dataset is divided into 2 parts, the first part contains data related to the weather data and the second part contains data related to the mapping. In Mapping the respected station numbers are mapped with their location, i.e there Longitude and Latitude. 
For cleaning the dataset, we completely remove any junk values which we found are not contributing towards our final output; this was done either by dropping the respective row values or in some cases the entire column. We were dealt with this, we worked on finding the correlation between these entities inside our dataset. Upon which we found that few things which had a direct correlation were: Dew Point, Wind Speed, Max and Min Temperature, Temperature. Once we have found the correlation, we have combined these two different parts into one.

### Integration

We are using libraries in python such as NumPy and Pandas that will be used for data processing. Using these libraries, we are trying to do basic preprocessing by replacing junk values with NaN values.
Provided that the data is structured, we are using Spark SQL functions like merge and filter to merge two dataframes, one which contains weather data along with station number and the other dataframe which contains station number along with its location.

### Storage

We are reading our processed csv file as a spark dataframe.

## Prediction and analysis

Once Data Preprocessing and cleaning is done, we have split the data into 70/30 for training and testing purposes. 
We have used the following algorithms for data Modeling purposes. While generating the output of our model or for visualization purposes, we have only used the data for the last 2 years because the ARCGIS Api wouldn’t work with 20 years of data. Even while running the model, we tried to run it for the entire dataset but the learning part itself took roughly 10 hours.

### Random Forest Regression

It’s a Supervised Machine Learning Algorithm which is widely used in classification and regression problems. The decision trees are generated using different samples and at the end a majority vote is taken which helps in decision making. One of the reasons why we chose to go ahead with Random forest is because there are a lot of attributes which might affect our decision. So in order to increase the accuracy we decided to use this algorithm. We have used our random state of 10. One thing that we noticed is that the accuracy of Random Forest is lower when it’s compared with Gradient Boosted Tree, However, data characteristics might affect the accuracy/Performance. However, while working with Random Forest Model we found that the accuracy of this model was very less compared to Gradient boost. Plus the MSE (Mean Square Error) value was high as well.

### Gradient Boost

XGBoost stands for Extreme Gradient Boosting. This algorithm is mainly used to reduce the bias errors when it’s compared with other models. This type of algorithm can be used for both target variables as well as categorical variables. When it’s used as a regressor, the cost function is represented by Mean Square Error (MSE) and when it is used as a classifier it’s Log Loss.

## Evaluation Metric

The Mean Square Error(MSE) or Mean Squared Deviation of an estimator measures the common of all the squares of errors- that is, the average squared distinction between the envisioned values and the real one’s. MSE may additionally talk to the empirical danger, as an estimate of the authentic MSE. 

**Note: We are using spark.ml to run our machine learning models and evaluators. Both our ML regression models are part of this library. We are using function under the stat class to check correlations between each of our features and precipitation.**

## Web Application

In this part, we provide a brief description of the application with respect to the user interface, backend, and frontend implementation. 

### Back-End

Once the csv file is generated, we are using ArcGIS Python API. We upload the file to ArcGIS online and create a feature layer. We apply this feature layer to create a map on ArcGIS. Once the map is completed, we use ArcGIS JavaScript API to display the map on our custom made webpage.

### Front-End

<img width="512" alt="Screenshot 2023-06-19 at 11 14 30 AM" src="https://github.com/Shadhrush5/Snowfall-Data-Analysis-and-Visualization/assets/119898772/b66dbaa7-3277-40b9-be93-a6521b810763">


This front-end UI displays distribution of snowfall for different timeframes. We have developed a custom UI for facilitating this.

### Visualization

We have utilized ArcGIS tool to generate the output map which shows the snowfall intensity over a period of time. We have provided an interactive time lapse for the snowfall prediction representation.

We have used 4 different colors to display the snowfall intensity. The legend is provided in the attached image above. Precipitation here represents either snowfall or rainfall and if the temperature is below 22 Fahrenheit then it represents snowfall.

### Evaluation results

We are using Extreme Gradient Boosting to predict precipitation based on values such as temperature, wind speed, mean dew point, precipitation and weather station location for our application. We have data from 2001-2019 and we are trying to train our model with a random split of 70/30. By the variation of maxIter variable value in the regressor, we are comparing it to the RMSE values to check which is the ideal maxIter value suitable for our prediction. It seems like the max iteration value affects the RMSE inversely but the effect reduces with the increase and seems like it will flatten at a point. The reason we did this analysis is to see if the time trade-off by increasing maxIter to the improvement in RMSE value. But the time increase is not viable if we are looking to scale and train with even more years of data. Our RMSE value is also not ideal to trust the predictions. Even though research has shown that Gradient Boost is good for prediction, it shows that there might be more improvement that can be made by using more fields and attributes.


