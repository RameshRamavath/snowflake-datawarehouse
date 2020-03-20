## Zero cloning in Snowflake

* Can create copy of table, database and schema - this is metadata operation, not actual data copy

* A snapshot of original data is made available to the cloned object

* The cloned object is independent of original object and can be modified

      # create table customer_copy clone customer
      
      # update customer_copy set job = 'Big Data' WHERE name = 'Ramesh'
         
         select * from customer where name = 'Ramesh' -- give ML Engineer
         select * from customer_copy where name = 'Ramesh'  -- gives 'Big Data'
      
---

## Zero cloning with time travel

*  clone the object as it was at a specific time or before a query

       # create table customer_v2 clone customer before (timestamp => 'time-stamp-value'::timestamp)
       
       # create table customer_v2 clone customer before (statement => 'query_id_last_update')