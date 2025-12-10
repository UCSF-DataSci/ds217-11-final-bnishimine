# Chicago Weather Analysis

## Executive Summary
This weather report analyzes data from Chicago Beaches along Lake Michigan. The dataset contained 196315 measurements from April 25, 2015 to December 3, 2025 across three different weather stations there. This project is a 9 section report that attempts to build a predictive model for humidity based off the data. The strongest model gleaned from the dataset was linear regression with an R^2 of only .013, indicating that further predictive models need to be built.

## Phase 1-2: Exploration

196315 Rows and 14 columns of data were initially loaded from a csv. The columns were Air Temperature, Wet Bulb Temperature, Humidity,  Rain Intensity, Interval Rain, Total Rain, Precipitation Type, Wind Direction,  Wind Speed, Maximum Wind Speed, Barometric Pressure, Solar Radiation,  Heading, and Battery Life. The Data was from April 25, 2015 to December 3, 2025.

**Key Data Quality Issues Identified:**
- 75 missing values in Air Temperature (0.04%)
- 75948 missing values in Wet Bulb Temperature (38.7%)
- The Missing values in Rain Intensity, Total Rain, Precipitation Type, and Heading (same 75948 records)
- All of these missing values were from the same weather station, indicating a pattern in the missing values
- 146 missing values in Barometric Pressure


![Figure 1: Initial Data Exploration](output/q1_visualizations.png)
*Figure 1: Mapping Air and Wet Bulb temperatures to each other, and showing the distribution of Humidity*

## Phase 3: Data Cleaning

Data cleaning was handled through both imputation and removal of values. For random missing values and outliers, imputation was utilized due to the temporality of the data. For patterns in missing values, removal was used.

**Cleaning Results:**
- Rows before cleaning: 196315
- Missing values: Forward-filled
  - Air Temperature: 75 missing → 0 missing
  - Barometric Pressure: 146 missing → 0 missing
- Missing values: Removed
  - Wet Bulb Temperature, Rain Intensity, Total Rain, Precipitation Type, and Heading: 75948 missing → 0 missing
- Outliers: Capped using IQR method (3×IQR bounds)
 - Filled using both ffill() and bfill()
- Duplicates: Removed (0 duplicates found)
- Data types: Measurement Timestamp and Measurement Timestamp Label converted to datetime.
- Rows after cleaning:  (120367 rows remaining)

Due to missing all of the data from Foster station, the decision was made to remove all data regarding that station, as using the information from other stations to fill that data may be inaccurate. For outliers, the expertise needed to specify bounds was not had, so IQR ranges were used to set limits instead. 

## Phase 4: Data Wrangling

As mentioned before, Measurement Timestamp was convered to the datetime datatype in order to utilize temporal analysis on the dataset. Measurement Timestamp was also set as the index for the dataframe. The format used was YYYY-MM-DD HH:MM:SS.

**Temporal Features Extracted:**
- `hour`: Hour of day (0-23)
- `day_of_week`: Day of week (0=Monday, 6=Sunday)
- `month`: Month of year (1-12)

## Phase 4: Feature Engineering

We created three derived variables and one rolling variable.

**Derived Features:**
- Temp Difference was the difference between Air Temperature and Wet Bulb Temperature
- Temp Ratio was the ratio between Air Temperature and Wet Bulb Temperature


**Rolling Window Features:**
- Wind Speed Rolling 24h: 24-hour rolling mean of wind speed

**Categorical Features:**
- Humidity Level was a categorical feature for humidity, with <50 representing a low value, 50 >= & <=80 representing a middling value, and <80 representing a high value.

## Phase 5: Pattern Analysis

**Temporal Trends:**
- Air Temperature: Clear seasonal trends, with it being higher during the summer and lower during the winter.
- Humidity: Some seasonal trend, not quite as strong as temperature trends.
- 
**Correlations:**
- Air Temperature shows a mild positive correlation with Total Rain, indicating that higher temperatures may be associated with increased rainfall in this dataset.
- Humidity has a moderate negative correlation with Temp Ratio, suggesting that as humidity increases, the difference of Wet Bulb to Air Temperature decreases.

![Figure 2: Pattern Analysis](output/q5_patterns.png)
*Figure 2: Advanced pattern analysis showing monthly temperature trends, how those match up with humidity and wind speeds, and a correlation heatma with extended variable inclusion*

## Phase 6: Modeling Preperation

Humidity was chosen as the target variable. Predictor variables chosen were Wet Bulb Temperature, Wind Speed 24h Rolling, and Total Rain.

**Temporal Train/Test Split:**
- We used an 80/20 Split based on temporality
- Training: 96,293 rows
- Testing: 24,074 rows
  

## Phase 7: Modeling

Linear Regression and Random Forest were the two models chosen.

**Model Performance:**

| Model | R² Score | RMSE | MAE |
|-------|----------|------|-----|
| Linear Regression | 0.013 | 14.932°C | 12.072°C |
| Random Forest | -0.448 | 18.093°C | 14.148°C |

**Key Findings**
- Neither model performed well
- Linear Regression performed only slightly better than chance
- Random Forest performed much worse than chance

**Feature Importance**
- Random Forest
- Total Rain: 46.7% Importance
- Wet Bulb Temperature: 27.2% Importance
- Wind Speed 24h Rolling: 26.1% Importance.

![Figure 3: Model Performance](output/q8_final_visualizations.png)
*Figure 3: Visualisations of actual versus predictors for linear regression, and feature importance for random forest.*


## Phase 8: Results

We were not able to create a successful model to predict humidity, with both linear regression and random forest having a sub par R^2. Possible shortcomings may be present in the way data was cleaned and in variable selection.


## Visualizations


![Figure 1: Initial Data Exploration](output/q1_visualizations.png)
*Figure 1: Mapping Air and Wet Bulb temperatures to each other, and showing the distribution of Humidity*

![Figure 2: Pattern Analysis](output/q5_patterns.png)
*Figure 2: Advanced pattern analysis showing monthly temperature trends, how those match up with humidity and wind speeds, and a correlation heatma with extended variable inclusion*

![Figure 3: Model Performance](output/q8_final_visualizations.png)
*Figure 3: Visualisations of actual versus predictors for linear regression, and feature importance for random forest.*

## Model Results

**Model Selection**
- Linear Regression is chosen as our better model
- With only a R^2 of .013, it is still not a convincing model
- The RMSE and MAE are 14.932 and 12.072, which are relatively high
  
## Time Series Patterns
**Long-term Trends:**
- Yearly patterns seem consistent over long periods of time
- Indicates stable yearly trends
- Specifically for the temperatures (Air Temperature and Wet Bulb Temperature) and Humidity

## Limitations & Next Steps
**Data Quality**
- Missing an entire stations worth of data may have weakened our predictive capabilities
- A lack of topic knowledge may have limited the effectiveness of our outlier removal

**Model Weakness**
- Neither of our models were effective at predicting the test dataset
- Indicates that our models will also not be predictive of future weather events

**Feature Engineering**
- Temporal features were not utilized as effectively as possible
- Especially due to the seasonality of temperature

**Next Steps:**

**Model Improvement**
- Experimenting with further feature engineering on temporality may allow for rapid improvement of dataset.
- External data sources with more related information may improve predictability

**Feature Engineering**
- Utilizing the existing temporal data 
- Creating more interactions between certain datapoints (eg. wind speed and temperature)
- Creating more categorical data

## Conclusion
The analysis from this dataset left a lot to be desired, failing to build an effective predictive model. Important takeaways were still derived from this dataset, including the seasonality of temperature and humidity. This dataset also gives some important considerations when it comes to cleaning data, especially when it comes to dealing with missing values, due to the large volume of missing data. This dataset is an important beginning point for predictive analyses.