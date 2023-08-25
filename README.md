# census_ml
Census Machine Learning

## Introduction

Explore HeatWave Machine Learning on your data stored in MySQL. In this example, we want to predict a household income of greater than 50K on existing household data. 

## Task 1: Create the census database

1. Login to OCI console and use Cloud Shell
2. Use mysql shell to connect to your HeatWave instance, specify your HeatWave IP address
   ```
   mysqlsh> \c admin@ip_address:3306
   ```
3. Create the database **census** and table structures
   ```
   mysqlsh> \sql
   CREATE DATABASE census;
   USE census;
   CREATE TABLE census_train ( age INT, workclass VARCHAR(255), fnlwgt INT, education VARCHAR(255), `education-num` INT, `marital-status` VARCHAR(255), occupation VARCHAR(255), relationship VARCHAR(255), race VARCHAR(255), sex VARCHAR(255), `capital-gain` INT, `capital-loss` INT, `hours-per-week` INT, `native-country` VARCHAR(255), revenue VARCHAR(255));
   CREATE TABLE census_test LIKE census_train;
   ```
4. Download the census data
   ```
   wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/CnLZDs6SuhHC4URov9ek69j0-bv32EyO433b8iZlwrfTX8VKNVg2Qu82Xsr9VZX6/n/idazzjlcjqzj/b/census/o/census_train.csv
   wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/A5TPt2fOnss-bFNffE2ITIuiXDwQ2wHV90YEP3_Jrqrpehy_7fQxVC39oFMR4NFK/n/idazzjlcjqzj/b/census/o/census_test.csv
   ```
6. Import the data into the 2 tables, one for training the model and the other for testing the model for accuracy
   ```
   mysqlsh>
   call sys.ML_TRAIN('census.census_train', 'revenue', JSON_OBJECT('task', 'classification'), @model);
   select model_handel from ML_SCHEMA_admin.MODEL_CATALOG;
   ```
7. Load the trained model and run the prediction on test table
   ```
   mysqlsh>
   call sys.ML_MODEL_LOAD('census.census_train_admin_xxxxxxxxxxxx', NULL);
   call sys.ML_PREDICT_TABLE('census.census_test', 'census.census_train_admin_1692948109037', 'census.census_prediction', NULL);
   ```
8. Verify the predicted results in the **census_prediction** table
   ```
   mysqlsh>
   use census;
   select * from census_prediction limit 10;
   ```
   

   


