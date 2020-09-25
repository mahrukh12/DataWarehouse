Project

    Create a Data Warehouse with AWS Redshift
    
Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results

In this project we are going to use two Amazon Web Services resources:

    S3
    AWS Redshift

The data sources to ingest into data warehouse are provided by two public S3 buckets:

    Songs bucket (s3://udacity-dend/song_data), contains info about songs and artists. All files are in the same directory.
    Event bucket (s3://udacity-dend/log_data), contains info about actions done by users, what song are listening, ... We have differents directories so we need a descriptor file (also a JSON) in order to extract data from the folders by path. We used a descriptor file (s3://udacity-dend/log_json_path.json) because we don't have a common prefix on folders

The objects contained in both buckets are JSON files. The song bucket has all the files under the same directory but
the event ones don't, so we need a descriptor file (also a JSON) in order to extract data from the folders by path. We used a descriptor file because we don't have a common prefix on folders
Log Dataset structure: Log Dataset

Song dataset structure:

{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null
, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", 
"title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

Schema for Song Play Analysis

The Redshift table we will create for the analytics team will be a star schema which is optimized for queries on song play analysis. The star schema contains the following fact and dimension tables:

    1 songplay (fact) - records event data for each song play
        songplay_id
        start_time
        user_id
        level
        song_id
        artist_id
        session_id
        location
        user_agent
    2 users (dimension) - users in the app
        user_id
        first_name
        last_name
        gender
        level
    3 song (dimension) - songs in music database
        song_id
        title
        artist_id
        year
        duration
    4 artist (dimension) - artists in music database
        artist_id
        name
        location
        lattitude
        longtitude
    5 time (dimension) - timestamps of records in songplays
        start_time
        hour
        day
        week
        month
        year
        weekday

Redshift Cluster Description

Redshift cluster properties:

    Node type: dc2.large
    Number of nodes: 2
    Cluster identifier: udacity-redshift
    Database name: dev
    Database port: 5439
    Master username: awsuser
    IAM role: myRedshiftRole
        Allows Redhshift to communicate with S3 with AmazonS3ReadOnlyAccess role
    Default VPC: vpc-86cf25fe
    Security group: redshift_security_group
        Opens TCP port for all incoming traffic in order to connect with database

ETL Pipeline

    Create staging and final tables in Redshift: python create_tables.py
    Copy data from s3 into Redshift staging tables: python etl.py
    Insert data from staging into final tables in Redshift (already performed by running etl.py in step 2)
    Verify final tables have correct data using Redshift query editor (see next section).

Author

    Mahrukh Malik