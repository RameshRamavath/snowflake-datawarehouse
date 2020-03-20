### Performance Optimization 
--

1  Use of **dedicated virtual warehouses** for specific work loads or groups

   * create roles and grant access to specific virtual warehouse
   
        * CREATE ROLE DATA_SCIENCE_DEV
        * GRANT USAGE ON WAREHOUSE <DATASCIENCE_WH> TO ROLE DATA_SCIENCE_DEV
        
        create login for users and assign them to roles
        
        

2  **Scaling up** to large warehouses when load is increasing

3  **Scale out** for unknown and unexpected workloads  - response to increasing demand

    * we can mention min and max number of warehouse within in cluster - based on demand snowflake spins additonal warehouse as an when required
    * new warehouse will have same config as our setup warehouse

4  **Design to maximize use of Cache**

    * Caching is automatic 
    * If same query used twice - snowflake will cache the results - for 24 hrs from the point of last re-use

5  use **CLuster keys** to partition large table - ex: transaction table on date column

    * CREATE TABLE <name> ... CLUSTER BY <column1, column2>
    