# Movies-ETL

![movies](Resources/vectorstock_20573944.png)
*****
*****

* By: Tyler Sojka
* October 2020
* Data ETL (Extraction, Transformation, Load) with python, pandas, postgresql, popsql, pgadmin, psycopg2, sqlalchemy

*****
*****

## Overview

The purpose of this project was to use the Extract, Transform, Load (ETL) process to create data pipelines. A data pipeline is a series of data manipulation steps that migrates raw data from disparate sources and move the data to a destination for storage and analysis. The ETL process creates data pipelines that also transforms the data along the way.

Throughout this project we will be working with scraped Wikipedia movie data stored as a JSON file, Kaggle data stored in a csv as well as a csv file of MovieLens movie ratings data. The ETL function we create will create an ETL pipeline from raw data to a SQL database by:

1. Extracting data from disparate sources using Python.
    * Using python and pandas, we will extract the data from our JSON and csv files.
2. Clean and transform data using Pandas.
3. Use regular expressions to parse data and to transform text into numbers.
4. Load data with PostgreSQL.

## Results

### Extract data

* Using python and pandas we extract our data from our various sources.
  * Read in the kaggle metadata and MovieLens ratings CSV files as Pandas DataFrames
  * Open and read the Wikipedia data JSON file.

### Transform data

* Clean wikipedia data
  * Consolidate all alternative languages
  * Consolidate all similar columns
  * Filter out all movies without a director
  * Filter out all columns with more than 10% null values
  * Clean box office column using regular expressions to catch all numeric values in different formats
  * Clean budget column similarly to box office
  * Clean release date column
  * Clean running time column
* Clean Kaggle Data
  * Convert budget, id, and popularity columns to numeric
  * Convert release date to datetime
  * Get rid of adult movies
* Merge Kaggle and Wikipedia DataFrames on imdb_id column
  * Handle duplicate rows by determining which has the best data and dropping the other after filling any null values
  * Clean up DataFrame
    * Organize all columns, rename any if needed
* Transform Ratings data
  * Organize DataFrame to get all the ratings for each movie
  * Pivot the data so that movieId is the index, the columns will be all the rating values, and the rows will be the counts for each rating value.
* Merge ratings with previously merged tables using a left merge to keep everything in movies_df

### Load

* Load all the data to a postgres database.
  * make a connection string to use to connect postgres to pandas in this format "postgres://[user]:[password]@[location]:[port]/[database]"
  * Create the database engine
  * To save the movies_df DataFrame to a SQL table, we only have to specify the name of the table and the engine in the to_sql() method.
  * to save the ratings data to postgres, we have to break it down into chunks

## Summary

After writing the function to extract, transform and load our data, we now have created an automated pipeline that takes in new data, performs the appropriate transformations, and loads the data into existing tables. This will allow us to continually clean and add any new data from these sources automatically.
