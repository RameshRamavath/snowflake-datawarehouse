###Data Loading Options

Solution depends on the **volume** and **frequency** of loading

1. Bulk Loading - **COPY** command
   
   - Data load from files already available in cloud storage
   - Copying (i.e. staging) data files from a local machine to an internal (i.e. Snowflake) cloud storage location before loading the data into tables
   - Can transform the data while loading
     * Column reordering    
     * Column omission       
     * Casts   
     * Truncating text strings that exceed the target column length
   
   
2. Continuous - Using **Snowpipe**

   - option to load small files incrementally in micro batches once files are added to stage and ready for ingestion
   - make latest data available for analysis as soon as the raw data is available
   - **Data Pipelines** for complex transformations
     * This workflow generally leverages Snowpipe to load “raw” data into a staging table and then uses a series of table streams and tasks to transform and optimize the new data for analysis

   
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
