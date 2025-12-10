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