# Data Modeling with Apache Cassandra

## Purpose

The purpose of this project is to create an Apache Cassandra database for a music streaming app called Sparkify to the data they've been collecting on songs and user activity. The raw data is in a directory of CSV files, and we will build a ETL pipeline that transfers data from a set of CSV files within a directory to create a streamlined CSV file to model and insert data into Apache Cassandra tables.

## Dataset

There is one dataset called `event_data` which is in a directory of CSV files partitioned by date. The filepaths are given as `event_data/<yyyy>-<mm>-<dd>-events.csv` where `<yyyy>` indicates the year, `<mm>` indicates the month and `<dd>` indicates the year. The fields of of `event_data` are:
- artist (string)
- auth (string)
- firstName (string)
- gender (char)
- itemInSession (int)
- lastName (string)
- length (float)
- level (string)
- location (string)
- method (string)
- page (string)
- registration (float)
- sessionId (int)
- song (string)
- status (int)
- ts (float)
- userId (int)

## Pre-requisites
The project makes the following assumptions:
* Python 3 is available
* `pandas` and `cassandra` are available
* Apache Cassandra is available on localhost


## Queries

As this is a NoSQL database(Apache Cassandra in this case), we will design the schema based on the queries we know we want to perform. For this project, we have three queries:

1. Find artist, song title and song length that was heard during sessionId=338, and itemInSession=4.
    - `SELECT artist, song, length from table_1 WHERE sessionId=338 AND itemInSession=4`
2. Find name of artist, song (sorted by itemInSession) and user (first and last name) for userid=10, sessionId=182.
    - `SELECT artist, song, firstName, lastName FROM table_2 WHERE userId=10 and sessionId=182`
3. Find every user name (first and last) who listened to the song 'All Hands Against His Own'.
    - `SELECT firstName, lastName WHERE song='All Hands Againgst His Own'`

## Schema

For the first query: 
- Column 1 = sessionId
- Column 2 = itemInSession
- Column 3 = artist
- Column 4 = song
- Column 5 = length
- Primary key = (sessionId, itemInSession)

For the second query:
- Column 1 = userId
- Column 2 = sessionId
- Column 3 = artist
- Column 4 = song
- Column 5 = itemInSession
- Column 6 = firstName
- Column 7 = lastName
- Primary key = (userId, sessionId) with clustering column itemInSession

For the third query:
- Column 1 = song
- Column 2 = userId
- Column 3 = firstName
- Column 4 = lastName
- Primary key = (song,userId)

## ETL Pipeline

1. Iterate through each event file in event_data to process and create a new CSV file 'event_datafile_new.csv' in Python
2. For each row of the csv data, insert the data in the appropriate column as described in Schema
3. Perform the Select query as described in Queries

## Files

`Project_1B_Project_Template.ipynb`

Jupiter notebook for the ETL pipeline used for pre-processing the csv files and load into the Apache Cassandra tables.

`event_data/`

Folder containing CSV files partitioned by date.

`event_datafile_new.csv`

A smaller event data csv file created to insert data into the Apache Cassandra tables.

`images`

Contains screenshot of the denormalised data present in event_datafile_new.csv file.

`README.md`

This file which discusses the specifics of the project.

## Build Instructions

Run each portion of `Project_1B_Project_Template.ipynb`.