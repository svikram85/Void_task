# Void_task

#Task 1

###Question 1 
1.Transactional sales data as a JSON response from the Shopsystem API.
Please refer to the jupyternotebook for this task. files are appropriately named.
2. Marketing spends from Google Ads aggregated on campaign and daily level stored in a
Postgres database (connection details below).
There were few problems with the information regarding this. The connection password was not correct space has to be removed and secondly the schema name for the tables is not given needed to do few trial and error by searching
the database to identify the correct schema. 
3. Marketing campaign performance from Google Ads aggregated on daily and product
level stored in a Postgres database (connection details below).
same as above.
4. Product and supply chain data stored in Google Sheets file.
Although I have used the stored json format but I can also be done more safely by using the authenticator 
5. Pick a 5th data source of your choice to further enrich the data. This data source should
be sourced via an API call. Examples are:
I have chosed the weather forecast data which includes min,max, mean temperature and precipitation at daily frequency

Questions
1. Is it possible to apply an ETL approach to some of the sources, already getting some of
the raw data transformation done before loading in the data?
2. Can you find out what all the features stand for and identify the keys?
3. What additional data source did you choose and why?

Answers:
1. Yes, for example in supplychain data we could apply ETL and could use the information in matching with pytrends database to know which which artical is closely fitting to what people are searching. We could also apply ETL approach on sales data so that we dont need to calculates sales for each article per day later on. One thing that we can also do is fill in the zero values in time series when the sales is zero. so that we don't need to match the length of time series later.
2. The keys I used to combine data is date column and variant_id column. Feature Identified for the database for forecasting are impressions,	marketing_spend,	sales_quantity,	avg_price,	avg_dicount,	temp_max,	temp_min,	temp_mean,	precip.
3. I chose weather data for forecasting the demand of sunglasses because buyers will usually do anticipatory buying for sunglasses i.e. they are more likely to buy sunglasses when weather is good or sun is shining.    


#Task 2
Questions
1. After initially analysing the data, what did you find out? Is there anything weird / strange
that could be improved?
2. What cleaning steps did you consider, which did you implement and why?
3. What have you learned about the data and the problem until now, that helped you in
coming up with further valuable features?
4. Are there other advanced feature engineering approaches / methods you would try to
improve accuracy if having more time?

Answers:
1.After initial look at data following thigs are evident 
  a. Time series has missing dates and sales is never zero but low, looks like the missing dates have zero sales for the article hence they are missing as we are using order table to get sales data. But marketing data is also missing on those day which is strange.
  b. article 4 has only two data points 
  c. none of the time series show very clear patterns 

2. For data merging I mostly used outer join so that I donot miss any information available in the databases. article 4 has only two days sales which match with one of the articles missing days hence I replaced the name of the article with main article as sales look convincingly close to the main article (main article id 40570570768535).Secondly I refilled the missing dates with zero values (assuming marketing spend was zero too that day)
3. Fron data it is very clear that the sales of the client recently started picking up after they started spending in online marketing and secondly  there seems to be no easily visible seasonality in the available data. Although it looks like some quarters are more preferred for sales over others. Hence it makes more sense to create some time features i.e. day, month , weekday, quarter.
4. Yes, mainly the model could be fine tuned based on lag of target and the other regressors.

#Task 3

Questions
1. Which deep learning model did you consider and why?
2. Which features are relevant for forecasting? Can some be removed?
3. Does your engineered feature really improve the accuracy of the deep learning model?
4. Is the accuracy going to change with more data and compute resources being available
for training?
5. What other models would you test on this data and why?

Answers:
1. I tried to fit pytorch_forecast, as my thinking was it was giving a lot of options for training i.e. defining unknowns_categorical and unknown_real variables which will make life easier for future forecast as we dont need to give values for the time period we are trying to forecast. secondly we can train one model for all the items which will be real handy if accuracy of the model is good. Unfortunately I couldn't succeed in doing so because pytorch was throwing assertion error related to context length and forecast lenght which i was not able to resolve. I used xgboost regressor to fit three models for each article. which also make sense if we can break the time in all possible seasonalities and use appropriate lags. Although lag analysis could be done more properly but because of lack of time I used 1 lag of the target variable, which improved accuracy of the model drastically.
2. From The feature importance plot we can see max_temp, min_temp, price,day of month and marketing spend are most important feature for the model. As we have seen previously there was no visible seasonaliy hence quarter, weekday were not relavent for the forecast they could have been dropped
3. I think transformation of variables might not be that useful but interaction variables might be use.
4. I would try to feature engineer the appropriate lags for regressors and the target variable which are very important in time series forecasting.

#Task 4 

Questions:
1. Is it possible to find causality in this scenario?
2. How did the demand generation with the new channel Google Ads work so far? Was it a
profitable investment?
3. In a real world scenario, what further information would be required to make a solid
proposal for a new price promotion, marketing allocation and procurement plan across
the product portfolio?

Answers.
Although I couldn't finish this task, following are my answers 
1. yes, we could employ walt test to check the causuality between sales and marketing spend.
2. From the wald test it is very clear that marketing is affecting sales very strongly where for price it seems inconclusive it might be because of very less price fluctuation in our data.
3. WE mainly need more data at different price level to identify the demand fluctuation with respect to price. Secondly we also need more data on types of campaign running so that we can seggregate the affect of each campaign saperately 
