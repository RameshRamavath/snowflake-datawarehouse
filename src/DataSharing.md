Data sharing in snowflake
--

* Instantaneous - no need to transform, extract or transfer data
* up-to-date - any changes in data by provider reflects to the consumer
* secured and governed - Access governed by the provider

* producer and consumer [consumer uses their own compute resources]

* Non-snowflake users can also use our databases and tables - concept of reader accounts [here producer compute resources are used]

  * create managed account for readers
  * show managed accounts gives the URL, User and password details of non-snowflake users



##### all these can done through SQL or snowflake UI [Look for shares option]

* inbound and outbound shares
* outbound -> create new share -> give name, database, tables etc -> add consumers [reader/full] & give consumer account no