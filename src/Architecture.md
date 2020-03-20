#### Key features

1. No PK/FK enforcement
2. Have rich Snowflake SQL

   * DDL/DML commands
   * SQL Functions
   * UDF or Stored Procedure (in Java script)

3. View and Materialized views
4. ACID transactions
5. Aggregations and Windowing functions

#### Key Integrations

1. Integration tools like - Informatica, Talend etc
2. BI Tools like  -  Tableau, Spotfire, Qlikview etc
3. Big Data - Kafka, Spark, DataBricks etc
4. JDBC/ODBC connectors
5. Language connectors - Python, GO, Node.js etc


### What is unique about Snowflake compared to other data warehouse solutions??

1. Scalability - storage and compute wise
2. Performance

  * No indexing
  * No performance tuning
  * No Partitioning 
  * No physical storage design 
  
    All handles by Snowflake design itself
 
3. Tunable for - Pay per use

   But all these features are comes with other DW solutions also - what's the unique? -- Architecture ??

## Architecture

**Separation of storage & compute**

 * database - Storage layer - using DDL
  
    * create database, table
    * create schema
    * load data into table - data in tables are stored in Amazon S3
    
    End User  --> Amazon EC2 (pay per use for compute) --> Amazon S3 (pay per use for storage)
   
 * Virtual data warehouse - these are scalable based on workloads
 
    * basically Amazon EC2 instances
    * can be of various sizes (X-small to 4X-large --> Single node to 16+ nodes)
    * can be **multi-cluster VM's** -- based on usage new VM's will be created on the fly
    * can create separate VM's for DDL, Data Load and Data Processing
    
    But similar separation of storage and compute also given by EMR and DataBricks - what new in snowflake ??
    
 
 ## database  - snowflake is based on shared storage
 
  Database tables => Amazon S3 files
  
  * Why S3??
  
    - **challenges**
      
      *I/O latency [HDFS on prem cluster is faster than S3]
      * CPU Overhead
      * Object storage [Overwrite only]
      
    - **Features/nice to have**
    
      * High availability
      * Durability
      * API's to read data parts / range based data reads
      
  * How snowflake is using these S3 features??
  
    - Break table into multiple smaller batches/ micro partitions [not more than 500MB]
    - re-organize data within each partition into columnar [Column values are within partition are stored together]
    - Compress each column data within partition - individual column compressions
    - Add header to each micro partition [Column offset and length of each column] - these micro batches are immutable files
    - this metadata in Service layer [metadata service] 
  
  
 

#### Hands on
----

 1. We can create separate warehouse for different teams/roles, so that specific users can use resources of particular warehouse!!
 2. Go to warehouse - choose warehouse size etc and create roles if not already and assign roles to newly created warehouse
 3. 
