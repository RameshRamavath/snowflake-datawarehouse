### Data Loading Options

Solution depends on the **volume** and **frequency** of loading

1. Bulk Loading - **COPY** command
   
   - Data load from files already available in cloud storage
   - Copying (i.e. staging) data files from a local machine to an internal (i.e. Snowflake) cloud storage location before loading the data into tables
   - Can transform the data while loading
     * Column reordering    
     * Column omission       
     * Casts   
     * Truncating text strings that exceed the target column length
     
   - COPY command uses compute service resources
   - manage resources from user side
   
   
2. Continuous - Using **Snowpipe**

   - option to load small files **incrementally in micro batches** once files are added to stage and ready for ingestion
   - used for loading **streaming data**
   - make latest data available for analysis as soon as the raw data is available
   - **Data Pipelines** for complex transformations
     * This workflow generally leverages Snowpipe to load “raw” data into a staging table and then uses a series of table streams and tasks to transform and optimize the new data for analysis

   - uses server less architecture for scaling up and down automatically
   - Doest't uses virtual warehouse resources
   
###### Loading from data files staged on cloud platforms 

   - Regardless of the cloud platform for your Snowflake account, we can load data from following platforms
   
     * Internal (i.e. Snowflake) stages
     
     * Amazon S3
     
     * Google Cloud Storage
     
     * Microsoft Azure Blob storage
     
     
###### External Tables

   - External tables enable querying existing data stored in external data storage for analysis without first loading it into Snowflake. 
   - This solution is especially beneficial to customers who already have a lot of data stored external but only want to query a portion of the data
     
     * for example the most recent data
     
   -  can create materialized views on subsets of this data for improved query performance
   - useful if we want use **small amount data**
   

### Steps for loading data


1 Prepare the files - into optimal format for loading


    ## prepare files 
    
     1. Proper delimited fields
     2. Number of columns in each row should be consistent
     3. Handle delimiter character inside column values
     
    ## Optimize file sizes
    
     1. for better use of parllelism - maintain file size 10MB - 100MB
     2. Split very large files into smaller ones
     
    ## Use proper data types
    
     
2  Stage the data - Internal or external 

    ## Why
    
     1. Intermediate step for ETL
     2. "Stage" is temporary area, where data that is to be loaded is accessable to snowflake
     3. Stage - may be cloud or our local machine
     
    ## Staging data for cloud storage
    
     1. most comman stage  - AWS S3 and Azure Blob store
     
    ## Staging data on internal storage
    
     1. data can be stored on local file system before we load into snowflake
     
    ## command
    
      create or replace  stage <stage_name> url = <cloud_stage_url> credentials = <username, passwords etc>
      

#### Bulk Load - Execute the COPY command
-

      
      ## Bulk copy data
      
       - useful for common batch loads
       - Command
       
          copy into <table_name>
             from  @<stage_name>
             pattern = '.*.csv'      -- one can give exact file name or pattern 
             file_format = (type = csv field_delimiter = '|' skip_header = 1)
             
             
          copy into <table_name>
             from  @<stage_name>/2020-03-20/test.txt    -- we can complete path also
             file_format = (type = csv field_delimiter = '|' skip_header = 1)           
       
       
       ** snowflake tracks the files already processed and do not loads same file again (by triggering same job again)
       ** snowflake maintains proccessed files metadata for 90 days !!
       

4  Mange regular loads - organize files and schedule our daily loads

---------

#### Bulk Loading JSON data
-

1 stage the data

2 load as raw (string) into temp table

    -- create a table in which we will load the raw JSON data
    CREATE TABLE json_raw (
      json_string VARIANT                -- VARIANT type can hold any type of data
    )
    
    COPY INTO <table name>
      FROM @json_example_stage/example_json_file.json
      file_format = (type = json);
      
    ## selecting top level fields from JSON
     
    SELECT 
    	json_string:data_set,
    	json_string:extract_date
    FROM json_raw;

3 analyze and prepare  -> using SQL analyse the data and prepare for flattening data

     ## use of --- lateral flatten --- function
     
     SELECT
         value:name::String,     -- value is keyword which represents output of join b/w table & flatten output
         value:state::String,    -- assign datatype you want
         value:org_code::String as org_code, # rename the output before storing into separate tables
     	 json_string:extract_date
     FROM
         json_raw
         , lateral flatten( input => json_string:organisations );  # organisations field we want to flatten
         
         
4 Flatten and load into target table -- using flatten function in snowflake

5 Regular updates


### Snowpipe
-

* Mechanism to load data into table as soon as its available in stage

* Kind of loading data in micro batched manner

* useful for arriving data like transactions or events

* server less - does not uses virtual warehouses

**How does it work**

* snowpipe definitions contain **COPY** statements for loading data which can be triggered automatically or manually

**Steps for loading data**

1 **Stage the data** - define a stage where our continuous steaming data arrives (S3 or Azure Blob)

2 **Test COPY command** - create a target table and test out COPY command

3 **Crete a new snowpipe** - using above copy command

4 **Configure cloud event**  - use cloud native event triggers to trigger snowpipe

    ##  create an external stage using an S3 bucket
    CREATE OR REPLACE STAGE snowpipe_copy_example_stage url='s3://snowpipe-streaming/transactions';
    
    ##  list the files in the bucket
    LIST @snowpipe_copy_example_stage;
    
    CREATE TABLE transactions
    (
    
    Transaction_Date DATE,
    Customer_ID NUMBER,
    Transaction_ID NUMBER,
    Amount NUMBER
    );
    
    CREATE OR REPLACE PIPE transaction_pipe 
    auto_ingest = true
    AS COPY INTO transactions FROM @snowpipe_copy_example_stage
    file_format = (type = csv field_delimiter = '|' skip_header = 1);
    
    SELECT * FROM transactions;
    
    SHOW PIPES;
    
    ## Set up event based trigger in S3 buket 
    
    go to bucket --> our folder --> Properties --> Events --> add notifications [fill properties properly]
    
    SNS queue -- destination -- use URN from show pipes [command]
    




   
   
   
