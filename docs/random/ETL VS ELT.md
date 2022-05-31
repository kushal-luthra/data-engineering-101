
####  Database
•	Meant for Transactional data - OLTP (online transaction processing) <br>
•	Such data should be Structured data <br>
•	Meant for recent data - day to day data. Example - online banking transaction. <br>
•	Common databases are Oracle, Mysql <br>
•	It is Schema on Write – i.e. while writing any data into database, its data type and table structure gets validated, and if any mismatch occurs, it raises exception. <br>
•	the cost to store the data in database is high. <br>

#### Datawarehouse – DWH
•	Its purpose is Analytical processing, wherein we require a lot of historical data to find the insights. <br> 
•	Querying in data warehouse involves writing complex queries that scans across history.  <br>
•	Why not use database for this purpose?  <br>
o	The moment we run complex queries on our database with an intent to do some analysis then your day to day transaction will become slow. <br>
•	We take the data from databases and migrate it to Datawarehouse to do analytical processing. <br>
•	we get the data from multiple sources. <br>
•	Meant for Structured Data. <br>
•	It is also Schema on write. <br>
	Eg – Teradata <br>
•	storage cost is high but lesser than your database. <br>
•	In case of data warehouse, its an ETL process – i.e. data is extracted, then transformed, and finally Loaded into database. <br>
-	Issue – we lose flexibility in terms of managing data. <br>
-	This is because even before writing, we are to decide how to store it. We cant always look into future and plan use case of our data. <br>
<br>
Suppose your data is in database, wherein you ->  <br>
- extract the data <br>
- Transform it (is a complex process) <br>
- Load it to Datawarehouse <br>
This approach reduces our flexibility. <br>

#### Data Lake
•	Its aim, like data warehouse, is to get insights from huge amount of data. <br>
•	Here the data is present in its raw form. <br>
•	It can be structured or unstructured. <br>
•	Eg - Log File - we can directly have this file in raw form in data lake. <br>
•	Its an **ELT process** - Extract Load & Transform. <br>
Eg - HDFS, Amazon S3 <br>
•	Benefit –  <br>
o	Cost effective – cheapest storage <br>
o	Schema on Read. <br>
o	create structure to visualize or see the data. <br>
o	Since data stored in raw form, it gives you enough flexibility. <br>