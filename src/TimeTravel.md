### Time Travel in Snowflake

**Time Travel**
  
  * travel back to a point in time before query Undrop the objects 
  * Undo accidental changes
  
**Objects**

  * Time travel for Databases, Schemas and Tables
  
**Retention**

  * zero to up to 90 days
  * can be configured at various levels [Database/ Schema/ Table/ User]


-----  

**Time Travel**
  
         -- update the Job column setting all rows to the same value
         update CUSTOMER set Job = 'Snowflake Architect';
         
         SELECT * FROM CUSTOMER;
         
         -- time travel to a time just before the update was run
         select * from CUSTOMER before(timestamp => '2019-05-26 03:20:15.855'::timestamp);
         
         -- time travel to 10 minutes ago (i.e. before we ran the update)
         select * from CUSTOMER AT(offset => -60*10);
         
         -- note down the query id of this query as we will use it in the time travel query as well
         update CUSTOMER set Job = NULL;
         --018c6f1f-00fd-b06c-0000-00000e99c991
         
         -- time travel to the time before the update query was run
         select * from CUSTOMER before(statement => '018c6f1f-00fd-b06c-0000-00000e99c991');
  

**Undo**

        -- undropping a table
        DROP TABLE CUSTOMER;
        SELECT * FROM CUSTOMER;
        
        UNDROP TABLE CUSTOMER;
        SELECT * FROM CUSTOMER;
        SELECT COUNT(*) FROM CUSTOMER;
        
        -- undropping a schema
        DROP SCHEMA PROD_CRM.PUBLIC;
        SELECT * FROM CUSTOMER;
        
        UNDROP SCHEMA PROD_CRM.PUBLIC;
        SELECT * FROM CUSTOMER;
        SELECT COUNT(*) FROM CUSTOMER;
        
        -- undropping a database
        DROP DATABASE PROD_CRM;
        SELECT * FROM CUSTOMER;
        
        UNDROP DATABASE PROD_CRM;
        SELECT * FROM CUSTOMER;
        SELECT COUNT(*) FROM CUSTOMER;