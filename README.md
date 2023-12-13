#  📈 A Dive Into Slowly Changing Dimensions with Snowpark and StreamSets

## 1. Overview
StreamSets Transformer for Snowflake is a hosted service embedded within the StreamSets DataOps Platform that uses the Snowpark Client Libraries to generate SnowSQL queries that are executed in Snowflake. Build your pipelines in the StreamSets canvas and when you execute that pipeline, StreamSets generates a DAG. StreamSets then uses the DAG and the Snowpark Client Libraries to generate SnowSQL. That SnowSQL is sent over to Snowflake to be executed in the Warehouse of your choice.

## Transformer for snowflake 
 
 ### How does it work

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/bb63241a-29b0-45ec-ad35-26d92790c8f4)

Transformer for Snowflake accelerates the development of data pipelines by providing features that go beyond a drag and drop interface to construct the equivalent of basic SQL. Snowpark enables StreamSets to construct the SnowSQL queries at the pipeline runtime, allowing pipeline definitions to be more flexible than SnowSQL queries written by hand. StreamSets also provides processors that combine the logic for common use cases into a single element on your canvas.

## 2. Prerequisites
➡️ Access to StreamSets DataOps Platform account (https://cloud.login.streamsets.com/login)

➡️ A Snowflake account and a schema that your user has CREATE TABLE privileges on (https://signup.snowflake.com/)

➡️ SQL Script located here 

```
/////////////////////////////////////////////////
// Section 1: Build and Load Tables /////////////
/////////////////////////////////////////////////

CREATE OR REPLACE DATABASE DEMO;
CREATE OR REPLACE SCHEMA HCM;

CREATE OR REPLACE TABLE DEMO.HCM.EMPLOYEES (
    ID INTEGER,
    FNAME VARCHAR(16777216),
    LNAME VARCHAR(16777216),
    TITLE VARCHAR(16777216),
    ACTIVE_FLAG BOOLEAN,
    VERSION INTEGER,
    START_TIMESTAMP TIMESTAMP,
    END_TIMESTAMP TIMESTAMP
);

INSERT INTO 
    DEMO.HCM.EMPLOYEES
VALUES
    (2,'Dwight','Schrute','Sales Rep',TRUE,1,current_timestamp(),null),
    (3,'Ryan','Howard','Temp',TRUE,1,current_timestamp(),null);
    

CREATE OR REPLACE TABLE DEMO.HCM.UPDATE_FEED
 (
        ID INTEGER,
        FNAME VARCHAR(16777216),
        LNAME VARCHAR(16777216),
        TITLE VARCHAR(16777216),
        CAKE_PREFERENCE VARCHAR(16777216)
    );

INSERT INTO
    DEMO.HCM.UPDATE_FEED
VALUES
    (2,'Dwight','Schrute','Assistant to the Regional Manager','(Beet) Red Velvet');

/////////////////////////////////////////////////
// Section 2: Check results /////////////////////
/////////////////////////////////////////////////

SELECT * FROM DEMO.HCM.EMPLOYEES;
    
```

### 3. What You'll Learn
In this guide, you will learn how to build pipelines using Transformer for Snowflake that are executed directly in your Snowflake Data Cloud, including:

➡️ How to build and preview pipelines

➡️ How to use the Slowly Changing Dimension Processor

➡️ How to apply transformations to multiple columns at one time

➡️ How to create and execute a job

# 2. Configure your Snowflake Credentials and Connection Defaults

   **Step-1** sign up (https://cloud.login.streamsets.com/login)

   **Step-2**  got to Account section  ⏩  Snowflake Settings tab. 

   <img width="958" alt="image" src="https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/96ee0fef-0ae7-47af-b385-49ecaf09230a">

                                                           
   <img width="941" alt="image" src="https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/00a65990-3d59-4b88-935c-3f15d5b99330">

  **Step-2**    Click On Snowflake Credentials -> Add, Give Your Snowflake account Credentials.

  ![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/f7bb6737-39e4-49e5-af32-63e24072b550)


   
