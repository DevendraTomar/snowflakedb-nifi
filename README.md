# Real Time Data Ingestion (SnowFlake Db) using NiFi

<b>Project : SnowFlakeDb-NiFi</b><br/>

<b>Motivation:</b> <br/>
This repository talks about how we can ingest real time data into snowflake using NiFi.

<b>Architecture:</b> <br/>

 ![](architecture.png)

<b>Prequisites:</b> <br/>
1. Create necessary database, schemas and tables in snowflake database.<br/>
Example : <br/>
create database demo;<br/>
use demo;<br/>
create schema tweets;<br/>
create or replace table demo.tweets.userdata (json_data variant);<br/>
<br/>

2. Create a stage backed by S3 (AWS) or Blob Storage (Azure)<br/>
Example : <br/>
CREATE OR REPLACE STAGE "demo"."tweets".STAGE_S3 URL = 's3://sample/testdata/' CREDENTIALS = (AWS_KEY_ID = 'XXXXXXXXXXXXXXXXXXXX' AWS_SECRET_KEY = 'XXXXXXXXXXXXXXXXXXXX') file_format = (TYPE= JSON);<br/><br/>
//For this example we are using json data to ingest into snowflake-db<br/>

3. Now once all this is setup , create a warehouse in Snowflake and place some json files in the above mentioned s3 location and run following command <br/>
copy into demo.tweets.userdata from @STAGE_S3; <br/><br/>
//As soon as this command is run , the snowflake stage will pick all the delta (json files) from s3 and ingest the same into Snowflake.<br/>

4. Now what we need to do it to orchestrate the same with NiFi and we are done.<br/>



