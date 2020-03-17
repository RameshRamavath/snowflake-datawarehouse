
### Main features

1. Built for Cloud - Optimized for Public Cloud services - CLoud only Software as a Solution (SaaS) - No HardWare or Software to maintain as its SaaS solution
2. Storage and Compute are charged independently (Decoupling of Storage and Compute) - Pay for only actual use and compute
3. Scalability  - because it uses S3 or Zero Blob - the storage available is virtually un-limited -
   Virtual warehouse enabling compute scaling


### Getting started

Trial account - https://trial.snowflake.com
Choose the cloud platform and region

After activation opens up in WorkSheet view!! - where we can run our queries

One can access

1. Databases
2. Shares
3. Warehouses - Virtual instances of warehouses
   -- Can configure
      Size (X-small, small, medium, Large, X-large, up to4X-large)
      maximum clusters
      Scaling policy (standard, Economy)
      Auto suspend  - maximum idle time before Warehouse suspends automatically
4. WorkSheets
5. History


### How to access SnowFlake

1. Web interface


### Hands on

** Create database and table

    Create database transactions;

    create table risk (
    transaction_id int,
    cust_name string,
    amount float,
    date string
    );

    select * from risk;

    Snowflake data types:

** Loading data

   Make sure data is available in staging area so that the database can access it

   In Snowflake this staging area can be -
   1. Internal storage/staging are or
   2. S3 Bucket

      Bulk Loading from S3

      Load data to S3 Bucket
      create a stage in Snowflake pointing to S3 bucket
        create or replace stage <stage_name> url='s3://snowflake-essentials/';   -- snowflake-essentials - is bucket name
      copy data to our table

      copy into <table_name>
        from s3://snowflake-essentials/our_first_table_data.csv
        file_format = (type = csv field_delimiter = '|' skip_header = 1);





