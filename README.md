# TemaBit-Fozzy-Group

### Опис проекту.
* На підставі історичних даних з продажу товарів сформувати прогноз продажів на період 14 днів у розрізі товар/день.

# EDA
* Кількість записів: 226486
* Columns: Unnamed: 0, date, category_id, sku_id, sales_price, sales_quantity
* All columns are filled and missed data is absent.
* Transforming column 'date' on datetime format.

* Distribution of sales by day: Visualization shows that the number of sales has a significant variability, 
which may be due to seasonality or other factors. Also , we have a significant drop in sales_quantity

* Distribution of sales by category: Visualization shows that the category "23" has a significant q-ty compared to "7" and "17"

* Distribution of sales by product: SKUs_id 64522 has the highest q-ty of sales

* Average sale price per day: We can notice some growing of sales from 01-2020

### Look like after the growing in sales price from 01-2020 we have a significant sales drop in q-ty

# Feature Engineering

* Create day indicators for week, month, year, day and day off
* Days Indicator (0 - weekday, 1 - day off)

# Modeling the data
* Coding of categorical data (One-Hot Encoding). We don't have ordinal data.

* Encoding the data using the On-Hot Encoding for categorical variables:
categorical_features = ['category_id', 'day_of_week', 'is_weekend', 'day_of_month', 'month', 'week_of_year', 'year']
* Using - StandardScaler
* Features and target variable:
X = df_encoded_scaled.drop(columns=['sales_quantity', 'date'])
* Set up k-cross validation

* Creating pipeline for each of the model:
    'Linear Regression'
    'Random Forest'

* Evaluating using k-cross validation:
Linear Regression: {'mae': 40.94313488957486, 'mse': 38512.75854392624, 'rmse': 196.2130234013318, 'r2': 0.09102424565854775}
Random Forest: {'mae': 4.586505624631587, 'mse': 2601.3751144752287, 'rmse': 50.91043274170248, 'r2': 0.9385500935139071}

* Feature importance for Random Forest:
         feature  importance
113  sales_price    0.899878
111    year_2019    0.014703
47       month_4    0.006741
46       month_3    0.004794
108    year_2016    0.003549

* Drop the specified columns
columns_to_drop = ['sales_quantity', 'date'] + \
                  [col for col in df_encoded_scaled.columns if 'day_of_week' in col] + \
                  [col for col in df_encoded_scaled.columns if 'week_of_year' in col] + \
                  [col for col in df_encoded_scaled.columns if 'day_of_month' in col]

* Set up TimeSeriesSplit

* Train the Random Forest model and output the results:
Test MAE: 2.4571973450151363
Test MSE: 26.127275243094513
Test RMSE: 5.111484641774297
Test R2: 0.638513179295416

* Define parameter grid and Fit for RandomizedSearchCV
* Best hyperparameters: {'n_estimators': 300, 'min_samples_split': 10, 'min_samples_leaf': 4, 'max_depth': 10}
* Train the Random Forest model with best hyperparameters
* Calculate metrics for best model
Best Model Test MAE: 2.7122141033186478
Best Model Test MSE: 27.653433710220966
Best Model Test RMSE: 5.258653222092227
Best Model Test R2: 0.6173978441890988

### Otput
* Conventional RandomForest has significantly worse error rates (MAE, MSE, RMSE) than models with feature selection and hyperparameter tuning, but very high R2.
* RandomForest with Feature Selection shows the best results in terms of MAE, MSE, and RMSE, indicating more accurate predictions. R2 is slightly lower but still acceptable.
* RandomForest with Feature Selection and Hyperparameter Tuning performs slightly worse than the model without tuning, which may indicate possible overtraining or not good enough hyperparameter selection.

* For further work, I think it is better to choose RandomForest with Feature Selection (model 2), because it shows the best results in terms of forecast accuracy (lowest MAE, MSE and RMSE). The model with hyperparameter tuning (model 3) shows worse results, which may indicate the need for further refinement of the tuning process.


# Testing part

* Downloading model and features parameters
* Reading df_future.csv
* Transforming column 'date' on datetime format

* Selecting data from 1/11/2020 till 14/11/2020
* Deleting column 'Unnamed' - this data is not nessesary
* Create day indicators for week, month, year, day and day off
* Encoding the data using the On-Hot Encoding for categorical variables:
categorical_features = ['category_id', 'day_of_week', 'is_weekend', 'day_of_month', 'month', 'week_of_year', 'year']

* Concatenate DataFrame
* Deleting columns'sales_quantity' before filtering
* Delete all datetime columns before filling in missing values
* Filling missing values
* Scaling data
* Predict
* Saving output to test_df
* Presenting first 5 rows:
        date  sku_id  predicted_sales_quantity
0 2020-11-01    1045                  1.544798
1 2020-11-02    1045                  1.689202
2 2020-11-03    1045                  1.689202
3 2020-11-04    1045                  1.689202
4 2020-11-05    1045                  1.689202




