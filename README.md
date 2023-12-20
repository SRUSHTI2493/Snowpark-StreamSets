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


  **Step-3**  Fill Snowflake Pipeline Defaults which you have created though SQL Script
  ![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/c35cedaf-445e-4492-89e1-cf4872c1914a)

 # 3. Create a New Pipeline

 Once your credentials are saved, you are ready to build your first pipeline. Using the tabs on the left hand menu, select Build > Pipelines. Use the ➕ icon to create a new pipeline.

 <img width="950" alt="image" src="https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/6d4ebe30-d75e-48c9-b415-25741e31ae21">

 The New Pipeline window will appear, so enter a pipeline name and if desired, a description, and make sure that the Engine Type selected is Transformer for Snowflake.

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/c5d25757-7a05-4853-9838-773850471548)

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/238c2fe2-2852-414a-a51b-ecdcf8dadafe)

**Add any users or groups for shared access to the pipeline, choose Save & Next, and then Open in Canvas.**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/5fbade36-8ec5-4e8c-b0a0-077d77fe23b3)

# 4. Code Snippets, Info Boxes, and Tables

Every Transformer for Snowflake pipeline must have an origin and a destination. For this pipeline, select the Snowflake Table Origin from either the drop down menu at the top of the canvas or from the full menu of processors on the right.

Click on the Table origin, under the General tab, and name it ‘Employees', and on the Table tab, enter the table name EMPLOYEES. This will be the Dimension table that tracks the employee data.

Add a second Snowflake Table origin to the canvas. This origin will be for a feed of updated Employee data. Under the General tab, name the origin ‘Updates' and on the Table tab, enter UPDATE_FEED.

At this point, run the first section of SQL in the SQL Script file to create and populate the EMPLOYEES and UPDATE_FEED tables in a Snowflake Worksheet. You will need to substitute the names of your database and schema into the script if you already have your own that you want to use.

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/9f66e3f6-a178-4d27-a169-df3e0b3f4f02)

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/ccabdc60-9992-4b5c-9446-f55ef61b9d49)

**NEXT Select Snowflake Table**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/38f0ceea-2e1e-43b2-a1a5-d8f69d92e9a8)

# 5. Add Slowly Changing Dimension
Now add the Slowly Changing Dimension processor from the top menu (which only appears when the origin is selected on the canvas) or from the right side menu of all Processors. Make sure the Employees Table Origin is connected to the Slowly Changing Dimensions.

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/a13d78d1-c0cd-4fef-bb97-dcb88c89e75d)

First, connect the Employees origin to the Slowly Changing Dimension processor. Next, connect the Updates origin to the Slowly Changing Dimension process. The line connecting from Employees to the SCD processor should have a 1 where it connects to the SCD, and the line connecting the Updates origin should have a 2

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/abfb630b-95b1-4262-be78-b558307b32e5)

In the settings for the Slowly Changing Dimension, add the following configurations:

🔹 SCD Type: Type 2

🔹 Key Fields: ID

🔹 Specify Version Field: Checked

🔹 Version Field: VERSION

🔹 Specify Active Flag: Checked

🔹 Active Flag Field: ACTIVE_FLAG

🔹 Active Field Type: True-False

🔹 Specify Timestamp Fields: Checked

🔹 Start Timestamp Field: START_TIMESTAMP

🔹 End Timestamp Field: END_TIMESTAMP

🔹 Timestamp Expression: CURRENT_TIMESTAMP (Default Value)

🔹 Behavior for New Fields: Remove from Change Data

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/d2d17892-8f6f-4947-9b5e-41dde6e0687a)


# 6. Add Snowflake Table Destination
The final step to complete this pipeline is to add the Table Destination.

Select Snowflake Table from the "Select New Destination to connect" menu at the top of the canvas or the menu on the right, just be sure to select the Snowflake Table Destination, indicated by the red "D".

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/82bcdc71-b7c4-47d0-9793-06bb7f0e9cc2)

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/ba9f36a8-dda0-4fab-bdf7-96d88b8fe380)

# 7. Preview Results
Now that the pipeline has sources and a target, there should be no validation warnings showing, and the pipeline can be previewed.

Activate the preview by selecting the eye icon in the top right of the canvas.

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/391195c0-6dc9-4bde-9e8d-a8295527db1f)
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/c41efd10-bff6-4fe6-9651-b768faac8121)
 
**The preview will generate the SQL that represents the pipeline and it will return the results for each processor. Since there are fewer than ten total records in the two origin tables, there is no need to adjust the default Preview Batch Record size of 10 records.
Once the preview is loaded, clicking on any of the processors will show a sample of records from the input and output streams. Selecting the Slowly Changing Dimension processor should result in an output like this:**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/99ba5a8e-dd52-4a50-9f5b-c47a9d43715d)

🔹 The input side shows the first two records that are already loaded in the EMPLOYEES table and the third record comes from the UPDATE_FEED table.

🔹 There are no update records for ID 3, Ryan Howard, and that record does not appear in the output stream.

🔹 The UPDATE_FEED does contain a title update for ID 2, Dwight Schrute, so the output stream has 2 corresponding records. One record that will serve as the update record to make the old record with the Title "Sales Rep" inactive, and a record that will be inserted as the new active record, with the title "Assistant to the Regional Manager".

🔹 Note that the UPDATE_FEED record on the input has an additional column CAKE_PREFERENCE, but since the Behavior for New Fields setting was set to Remove from Change Data in step 4, this column is just ignored.

# 8. Add a Transformation
To transform the data in the pipeline, add an Apply Functions processor between the Updates origin and the Slowly Changing Dimension processor. Click on the line connecting the two processors, and then select the Apply Functions

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/43c627bb-d9db-4b23-9907-eaa5d7134d96)

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/59ee49a4-3aaf-47eb-96c7-df987f59cc90)
**On the Functions tab of the processor, add a simple function to make all the values upper case. This can be done simply by setting Field Expression to the regular expression .*, which will include all columns. Set the Function Type to String, and choose the UPPER function from the String Function list.**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/c69bc42e-4a35-4a16-87ec-7aa4128fc3e2)

**Now preview the results, and select the Apply Functions processor to see the input and output.**
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/51ab188d-5c6b-4eea-82d2-bf5e88c5cccb)

# 9. Create and Run Pipeline as a Job
**Before being able to run the pipeline, it needs to be published. Once the preview is closed, check the pipeline in.**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/0c1a9e7f-4211-431c-bcb1-7982f2facb96)
**Enter a Commit Message and select Publish and Next.**

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/05964335-13e9-47e4-b5dd-f08f32559a62)

**Next, choose Save and Create New Job.**
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/cb85a656-7bb6-44f8-9568-d9b6f684280c)

**Update the Job name if desired, add a job description, and select Next. On the Select Pipeline option, select Next, and select Save & Next for the next two options.**
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/1d6e6d55-2938-4936-b327-1f54bb096b8d)

**The Job page will now appear, and the job status can be seen at the top of the canvas and in the window below the canvas.**
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/052db512-5470-41d0-a2d2-fc1a50b76fd0)


# 10. Check Results in Snowflake

Run the query SELECT * FROM DEMO.HCM.EMPLOYEES; to see the updated results in the EMPLOYEES table. Here it can be seen that the record with the updated title and upper case names and title was added as a second version of the record, and the previous record was expired.

![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/2984763c-0e28-4722-896f-8e16b5cc97d8)
**While in Snowflake, check out the Query History to see the Merge record that the StreamSets job created.**
![image](https://github.com/SRUSHTI2493/Snowpark-StreamSets/assets/87080882/f63615c6-0628-44e9-b2f2-aa83809cea44)

# 11. Conclusion
Congratulations on completing this lab!......
