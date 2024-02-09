# AirQuality-ML-APP
Exploring the process of leveraging machine learning (ML) to forecast air quality. And step-by-step instructions on integrating this model into a user-friendly application using Flask

**Background:**

Polluted Air is responsible for various health issues such as increases the risk of heart disease, respiratory infections, lung cancer, tuberculosis, chronic respiratory disease, diabetes, kidney disease and low birth weight, all of which can lead to premature death. And it is a major environmental threat, for instance it contaminate the surface of bodies of water and soil.Meteorology such as Temperature, Wind Speed, Relative Humidity, etc. contribute to air pollution, along with emissions from cars, industries, power plants, crop burning, etc. Air Quality Index (AQI) which is a scorecard of six categories that describe ambient air quality relative to the relevant standards. This project is focused on predicting AQI using meteorological variables.

**Data Collection and Processing:**

AQI and meteorogical datadata is collected from three different Counties in Arkansas,USA for 2018-2022 from NOAA(National Oceanic and Atmospheric Administration) and EPA(Environmental Protection Agency). Daily Max AQI data is considered for counties with multiple AQI monitors, and weather data is taken from nearest met station of the specific AQI monitor. There are a number of variables, but for this project Wind Speed(AWND), Precipitation(PRCP), SNOW, Temperature Max (TMAX) and Min(TMIN) data is considered. The basic data quality check is performed and missing values are processed by interpolation using python.

**Data Analysis**
I used python coding to analyze various aspects of the data. For instance, only 0.43% of data has AQI>100, whereas 92.22% data is in 0-50 class. Correlation heatmap shows correlation co-efficient between variables,and we can see Max Temperature (positively)  and  Precipitation (negatively) are strognly correlated to AQI. 

<img src="https://github.com/iqbal-T19/image/blob/main/AQI%20counts_Overall.png?raw=true" alt="Image 1" style="width: 400px; height: 300px; object-fit: cover;" /> <img src="https://github.com/iqbal-T19/image/blob/main/Corr%20plot.png?raw=true" alt="Image 2" style="width: 400px; height: 300px; object-fit: cover;" />

The seasonality plot and timeseries plot shows, AQI has seasonal effect and Summer season shows highest AQI variability!

<img src="https://github.com/iqbal-T19/image/blob/main/TimeSeries_Plot.png?raw=true" alt="TimeSeries_Plot" style="width: 400px; height: 300px; object-fit: cover;" /><img src="https://github.com/iqbal-T19/image/blob/main/Seasonality_Pulaski.png?raw=true" alt="Pulaski_Seasonality" style="width: 400px; height: 300px; object-fit: cover;" />
<img src="https://github.com/iqbal-T19/image/blob/main/Seasonality_Crittenden.png?raw=true" alt="Crittenden_seasnality" style="width: 400px; height: 300px; object-fit: cover;"/><img src="https://github.com/iqbal-T19/image/blob/main/Seasonality_Washington.png?raw=true"  alt="Washington_seasnality" style="width: 400px; height: 300px; object-fit: cover;"/>

**Feature Importance**
This project evaluated feature importance of the variables to select features that are most important to use in the model. Since, This is a classification problem, feature importance is evaluated utilizing RandomForestClassifier, CART (DecisionTreeClassifier), and XGBoost models.

<pre>
```
df = pd.read_csv('Combined_Final.csv')
# Creating AQI classes 
bins = [-1, 50, 100, float('inf')]  # The bins for the classes
labels = [0, 1, 2]  # The labels for the classes (0: 0-50, 1: 51-100, 2: >100)
df['AQI_Class'] = pd.cut(df['AQI_O3'], bins=bins, labels=labels)
# Defining features and target variable
X = df[['AWND', 'PRCP', 'SNOW', 'TMAX', 'TMIN']]
y = df['AQI_Class']
# Spliting the data into training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
# Using RandomForestClassifier
rfc = RandomForestClassifier(random_state=42)
rfc.fit(X_train, y_train)
# Using CART model
cart = DecisionTreeClassifier(random_state=42)
cart.fit(X_train, y_train)
# Using XGBOOST model
xgb_model = xgb.XGBClassifier(use_label_encoder=False, eval_metric='mlogloss', random_state=42)
xgb_model.fit(X_train, y_train)
# Getting feature importances
importances_rfc = rfc.feature_importances_
importances_cart = cart.feature_importances_
importances_xgb = xgb_model.feature_importances_
importances_rfc_percentage = 100 * (importances_rfc / importances_rfc.sum())
importances_cart_percentage = 100 * (importances_cart / importances_cart.sum())
importances_xgb_percentage = 100 * (importances_xgb / importances_xgb.sum())
```
</pre>

<img src="https://github.com/iqbal-T19/image/blob/main/Feature_Importance.png?raw=true" alt="Feature Importance Plot" />



