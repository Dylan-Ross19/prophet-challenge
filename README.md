# prophet-challenge

I have tasked with being a growth analyst at MercadoLibre, the most popular e-commerce site in Latin America. I need to analyze the company's finicial and user data to make the company grow. 

## Step 1: Analyzing Hourly Google Search Traffic

 
  ### 1. Reading and Slicing Data
   
   We start the analysis by importing the necessary libraries and dependencies, including Prophet for time series forecasting. We then load the hourly Google search trends data into a Pandas DataFrame, focusing specifically on the month of May 2020.
  
  ### 2. Exploratory Data Analysis (EDA)
   
   After loading the data, we conduct a exploratory data analysis to gain insights into search traffic trends during May 2020. This includes displaying the first and last five rows of the DataFrame, checking data types, and plotting the search traffic for May 2020. Visualizations and summary statistics reveal distinct patterns, such as high search traffic during midnight hours and notable spikes on May 4th and May 5th. The total search traffic for May 2020 was 38181
 
  ### 3. Comparing Median to May 2020
   
   We compare the total search traffic for May 2020 with the median monthly search traffic across all months. This comparison provides information into whether there were significant deviations in search behavior, particularly around key events such as the release of MercadoLibre's quarterly financial results. We found the median search traffic acrros all months was 35172.5. The comparison is a difference of 1% increase of search traffic, or just around 3000 more searches.

## Step 2: Mine the search traffic data for seasonality
  
  ### 1. Grouping by Hour of Day
   
   Grouping the hourly search data to plot the average traffic by the hour of the day reveals peak search traffic is around midnight.
  
  ### 2. Grouping by Day of the Week
   
   Grouping the hourly search data to plot the average traffic by the day of the week reveals the busiest day is on Tuesdays.
  
  ### 3. Grouping by Week of the Year
   
   Grouping the hourly search data to plot the average traffic by the week of the year reveals increased search activity during the winter months, particularly in February, as well as a resaurgence of traffic each quarterly release.

## Step 3: Relate the search traffic to stock price patterns
  
  During a meeting with the finance group, the possibility of a relationship between search data and the company's stock price is raised. To investigate this, we complete the following steps:
   
   ### 1. Read in and plot the stock price data, then concatenate it with the search data in a single DataFrame.
   
    Upload the "mercado_stock_price.csv" file into Colab and store it in a Pandas DataFrame. Set the "date" column as the Datetime Index. Plot the closing price of the stock to observe its trends over time. Stock price data is loaded from the CSV file and concatenated with the search trends data to create a single DataFrame for analysis.
   
   ### 2. Analyze market events in 2020, focusing on the first half of the year to identify common trends consistent with narrative.
   
    Given the unique market events of 2020, particularly the surge in e-commerce activity following the initial shock to global financial markets, focus on the first half of 2020 (January to June). Slice the data to this period and plot both the stock price and search traffic data to identify any common trends consistent with the narrative.

    Time series plot is used to visualize the relationship between search activity and stock price movements. The code utilizes Matplotlib's 'subplot' functionality to create separate axes (plots) within a single figure. Each subplot represents a different aspect of the data, allowing for a comprehensive analysis of various components simultaneously.
    
    Our analysis suggests that while there are some correlations between search activity and stock price movements, the evidence for a consistent and direct relationship is limited. Notably, we observed a correlation between lower search activity and the low point in Mercado Libre's stock price in late March to early April. However, as the stock price recovered, search activity also increased, albeit not at the same pace.
    An interesting observation is the spike in both search traffic and stock price around May 5th, 2020, coinciding with the company's earnings release.
   
   ### 3. Create additional columns for lagged search trends, stock volatility, and hourly stock return to explore potential correlations.
   
    Create a new column named "Lagged Search Trends" that shifts the search traffic data by one hour. Additionally, calculate two additional columns:
    
    "Stock Volatility": representing an exponentially weighted four-hour rolling average of the company's stock volatility. There is a slight negative correlation between searches for the firm and subsequent stock volatility. Increased search activity tends to indicate less near-term hourly stock risk for the firm. 

    mercado_stock_trends['close'].pct_change(): This part of the code calculates the percentage change in the closing stock price ('close' column) using the pct_change() method. This gives us the returns of the stock price
    .rolling(window=4): This specifies a rolling window of 4 periods (in this case, hours) over which the standard deviation will be calculated. Rolling window operations allow us to compute statistics over a moving window of data.
    .std(): This calculates the standard deviation of the returns within each rolling window. Standard deviation is a measure of the dispersion or volatility of a set of values. Here, it indicates how much the stock price returns fluctuate over the specified window. Similar is repeated for below
    
    "Hourly Stock Return": representing the percent change of the company's stock price on an hourly basis. On the other hand, there is a positive correlation between search activity in one hour and stock returns in the next. However, this effect is weak, with the correlation being close to zero. This suggests that while there may be some relationship between search activity and stock price movements in the short term, it is not strong enough to reliably predict stock market behavior.

   ### 4. Review time series correlation to determine if a predictable relationship exists between search traffic and stock price patterns.
    
    We analyze the correlation between the lagged search traffic and both the stock volatility and the stock price returns. This step helps determine if any predictable relationship exists between search traffic patterns and stock market behavior.
    .corr(): This method computes the pairwise correlation of columns, generating a correlation matrix. Each cell in the matrix represents the correlation coefficient between two variables.
    corr_table: This variable stores the correlation matrix, which provides insights into the relationships between the selected variables.

## Step 4: Create a time series model with Prophet.
   
   ### 1. Set up the Google search data for a Prophet forecasting model.
   
    A. Data Preparation: We reset the index of the DataFrame and labeled the columns as 'ds' for dates and 'y' for search traffic volume. Any NaN values were dropped to ensure data integrity.

    B. Prophet Model Setup: We initialized a Prophet model object and fitted it to the prepared dataset.

    C. Future Prediction: A future dataframe was created to hold predictions, extending the forecast out to approximately 80 days (2000 hours).

    D. Forecast Generation: Using the Prophet model, we generated predictions for the trend data and stored them in the forecast_mercado_trends DataFrame.

   ### 2.  After estimating the model, plot the forecast. How's the near-term forecast for the popularity of MercadoLibre?

    We plotted the Prophet predictions for Mercado trends data to visualize the forecast. While the uncertainty intervals were large, indicating low confidence, the forecast suggested a downward trend in search popularity. However, more data is needed to improve confidence levels.

   ### 3.  Plot the individual time series components of the model to answer specific questions.

    We visualized the individual time series components of the model to answer the following:

     A. Greatest Popularity Time: The time of day exhibiting the greatest popularity was found to be 23:59.
     B. Busiest Day of the Week: Tuesday emerged as the day with the most search traffic.
     C. Lowest Point for Search Traffic: October was identified as the month with the lowest point for search traffic in the calendar year.

   plot_components(mercado_model, forecast_mercado_trends): This function is called to generate visualizations of the individual components of the forecasted time series data using the Prophet model.
   mercado_model: This parameter specifies the Prophet model object that was previously trained on the historical search traffic data.
   forecast_mercado_trends: This parameter specifies the DataFrame containing the forecasted time series data generated by the Prophet model.