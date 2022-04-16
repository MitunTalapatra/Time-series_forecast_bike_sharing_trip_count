# Forecasting Bike Sharing Trip count through time-series analysis
The goal of the project was to predict hourly trip count for bike sharing events. Prediction was derived from the ~300M bike sharing data reported since October 2010 published by Capital Bike Sharing. The results was compared in local (single station) and global (multiple stations) using Neural Prophet.

There are 1225 stations of 8 regions around Washing DC are being operated by Capital Bike Share. To reduce the computation time within project time, 10 station with most popular were selected for the project. The id of stations are:
['31248.0', '31214.0', '31241.0', '31101.0', '31229.0', '31247.0', '31201.0', '31258.0', '31200.0', '31623.0']

## Notebooks: 
data_downloader is to extract data from AWS S3 bucket
data_processing is to merge all data, process, join with region data and separate them by region
Local_model includes identifying 10 stations and run Neural Prophet for each individually
Global_model includes test to apply global model
data_viz is to prepare map for stations

## Model Description:
Neural Prohet is developed by open source community which is heavily inspired by Facebook Prophet. It has additional features like AR lags, adding hidden layers, special event, global modelling etc. In this project, hourly prediction of 10 stations from Washington DC region has carried out. Results derived in local ( by each station individualy) and in global ( all 10 stations counted as 10 region) approach.

### - Local approach:
Model paramteres: 
  growth='linear' (as a linear growth was observed during initial test)
  yearly_seasonality=True, weekly_seasonality=True, daily_seasonality=True (intentionally applied to avoid model's own decision)
  n_lags=3*24 and learning_rate=0.01 (lags were considered per hour for 3 days)
  additiona feature: add_country_holidays('US') (to see whether weights from holidays were influencing the model)
  
  	              SmoothL1Loss	    MAE	         RMSE	  
        Train	        0.014861	  3.069174	  4.427516	
        Validation	   0.006771	  2.387943	  3.012018
        
  ### - Global approach:
  Global model converts each region in a dictionary and the model individually for each region. The model gives higher accuracy than previous approach. 
  There is a possibility of the influence of total count of all region as of the model's documentation. In addition to that computation requires longer and 
  more memory than CPU for more iteration and lags
  Model parameter:
    a simple model with 24 lags and 5 epoch was applies which derived following score:
    
      	              SmoothL1Loss	    MAE	         RMSE	  
        Train	        0.026165	      0.150889	  0.227946
 
 ## Summary/Way Forward:
 - From the result, it is observed that global model has opportunity to develop with remove ifluence of total count. Also some spatial features could be   added like region's weights.
 
 - From the yearly seasonality trend, it is observed that trend is downward, specially after 2019. The reason could be due to pandemic and increasing number electric scooter.
 
 - From the monthly seasonality, the prediction also followed the same trend
