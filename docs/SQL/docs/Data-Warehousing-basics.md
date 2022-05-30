# Data Warehouse

### What is Data Warehouse?
Put simply, it’s a central place where data is stored for the purpose of analysis and reporting.<br>
The data may be collected from a variety of sources.<br>
It’s organised to provide complete data for users that can be easily understood in a business context.<br>
Different from databases in that it’s purpose is for analysis<br>

-In companies, we use dimensional modelling to design our data warehouses.<br>
Dimensional modelling always uses two types of tables – facts and dimensions.<br>

a. Fact Tables-<br>
Contain the measurements, metrics or facts of a business process i.e. Transactions (items & baskets)<br>
Each fact will include measures (e.g. spend) and context data.<br>
Describe the measurements/facts in a transaction table – what was bought/how much it cost, etc. <br>

b. Dimension Tables-<br>
Stores attributes that describe the objects in a fact table i.e. Stores, Products, Customers<br>

They are linked together – this is a relational model -<br>
a. Primary key -<br> 
Is the attribute or a set of attributes in an entity whose value(s) guarantee only one tuple (row) exists for each value.<br>
The fact table will also include foreign keys that relate each fact to our dimension tables.<br>

b. Foreign key <br> The primary key of another table referenced here.<br>
Each entity in a dimension table will contain an attributes that describe that entity. <br>There will also be a key that is used to join the dimension table to the fact table.<br>


#### Characteristics of Data warehouse
- Data warehouse is a database which is separate from operational database which stores historical information also.<br>
- Data warehouse database contains transaction(OLTP) as well as analytical data(OLAP).<br>
- Data warehouse helps higher management  to take strategic as well as tactical decisions using historical or current data.<br>
- Data warehouse helps consolidated historical data analysis.<br>
- Data warehouse helps business user to see the current trends to run the business.<br>
- Data warehouse is used for reporting and data analysis purpose.<br>

#### Types of Data warehouse systems
a. Data Mart<br>
Data Mart is a simplest set of Data warehouse which is used to focus on single functional area of the business.<br>

b. Online Analytical Processing (OLAP) -<br>
[Refer](https://www.complexsql.com/olap-vs-oltp/) <br>
OLAP is used at strategic level and contains aggregated data, covering number of years.<br>
The key purpose to use OLAP system is to reduce the query response time and increase the effectiveness of reporting.<br>
So, data is denormalized.<br>
OLAP uses Star-Schema,Snowflakes schema or Fact-Dimensions.<br>
OLAP database stores aggregated historical data in multidimensional schema.<br>
eg - summaries data.<br>

c. Online Transactional Processing (OLTP) -<br>
[Refer](https://www.complexsql.com/olap-vs-oltp/) <br>
It is operational database, maintaining large number of small daily transactions like insert,update and delete.<br>
Data is normailzed.<br>
OLTP uses Entity Relations.<br>
OLTP system maintains concurrency and it avoids the centralization so as to avoid the single point of failures.<br>
Data concurrency and integrity = focus.<br>

d. Predictive Analysis<br>

#### Difference between Data Warehouse and Data Mart
a. Definition -<br>
The Data Warehouse is a large repository of data collected from different organizations or departments within a corporation.<br>
The data mart is an only sub-type of a Data Warehouse. It is designed to meet the need of a certain user group.<br>

b. Focus -<br>
Data warehouse focuses on multiple business areas.<br>
Data mart focuses only on single subject area.<br>

c. Usage -<br>
DW - It helps to take a strategic decision.<br>
DM  - The data mart is used to take tactical decisions for growth of business.<br>

d. Type of system -<br>
DW - This is centralized system where one fact is at center surrounded by dimension tables.<br>
DM - Data mart system is de-centralized system<br>

e. data model -<br>
DW = top down<br>
DM - bottom up<br>

f. source -<br>
Data warehouse data comes from multiple heterogeneous data sources.<br>
Data mart data is data of only one business area.Many times it will come from only one data source.<br>

g. Implementation Time -<br>
Data warehouse contains all data which will come from multiple data sources. It will take time to build data warehouse. The Time to build data warehouse is months to years.<br>
Data mart is small data warehouse which will contain the data of only a single business area. The implementation time to build data mart is in months.<br>

### Data Warehousing
-It’s the process of TRANSFORMING data into information and making it available to users in a TIMELY enough manner to make a difference.<br>

### Different use cases of ETL
a. Data Warehousing -<br>
User needs to fetch the historical data as well as current data for developing data warehouse. The Data warehouse data is nothing but combination of historical data as well as transactional data. Its data sources might be different.User needs to fetch the data from multiple heterogeneous systems and load it in to single target system which is also called as data warehouse.<br>

b. Data Migration<br>
ETL tools are widely used in data migration projects. <br>
If the organization is managing the data in oracle 10 g previously and now organization wants to go for SQL server cloud database then there is need to migrate the data from Source to Target.<br>
To do this kind of migration the ETL tools are very useful. <br>
If user wants to write the code of ETL it is very time consuming process.<br>
To make this simple the ETL tools are very useful in which the coding is simple as compare to PL SQL or T-SQL code.<br>
So ETL process i very useful in Data migration projects.<br>

c. Data Integration<br>
Now a days big organizations are acquiring small firms. <br>
Obviously the data source for the different organizations may be different.We need to integrate the data from one organization to other organization. <br>
These kind of integration projects need the ETL process to extract the data,transform the data and load the data.<br>

d. Third Party data management-<br>
many a times company outsources process to different vendors.<br>
eg - telecom - billing managed by one and CRM by other vendor. <br>
If CRM company needs some data from the company who is managing the Billing. That company will receive a data feed from the other company. <br>
To load the data from the feed ETL process is used.<br>

### ETL
#### Extract
Extract source data from our client-<br>
Once the data we will receive has been agreed, it is transferred from the client to us via our secure FTP system called Axway. This data is known as source data.<br>
Once the data has been received, it is validated according to the retailer-specific rules which are outlined in the DIS.<br>
If the data does not reflect what is outlined in the DIS changes may need to be made – either by updating the DIS or requesting a resupply.<br>
Once we agree the data is in the correct format it can be read in.<br>
Data Extraction can be - Full or Partial (Delta).<br>

DIS - The data we receive is mapped in a document known as Data Interface Specification (DIS).<br>

QA check in Source / RAW layer-<br>
A key section of RAW is quality assurance (QA). We carry out standard checks to ensure data is “healthy” and without errors<br>
Main focus of the checks is the fact tables, and relation on the fact data with the key dimension tables.<br>
Checks include:<br>
 * Number of baskets in the basket and item tables<br>
 * Levels of spend<br>
 * Missing foreign/primary keys<br>
Any issues found are recorded and can either be resolved with the solution DSG or may require input from the client<br>
Once these checks are successfully completed the build moves forward into the PREP stage.<br>
 
#### Transform
Transform this client data to meet the operational and business needs of our internal database.<br>
Within prep we transform the client data into a standardized format within the guidelines of Marketplace<br>
What types of transformations do we perform?<br>
i. Reject bad data -<br>
Bad data i.e. record where key info is missing.<br>
What is rejected depends on the business rules.<br>
We keep a record of rejected data by extracting them to a separate table, mark the missing field.<br>

ii. Remove duplicates<br>
Duplicate data can have negative impact on results. <br>
Important to understand if it is really a duplicate before removing.<br>

iii. Convert fields<br>
Convert from character to date and numeric fields where relevant e.g. spends/quantities.<br>

iv. Text manipulations<br>
Changes format e.g. change lookup value to descriptive form.<br>

v. Merges with other tables<br>
Merge lookups/useful fields that should be on specific table.<br>

vi. Aggregate data<br>
Sometimes need to roll up products in same basket or even create basket table - involves summations.<br>

vii. Rename fields<br>
Rename to make more meaningful – esp if in a foreign language.<br>

viii. Create standard fields<br>
Essential to marketplace, same naming conventions e.g. dib_bask_code.<br>

#### Load
Load into our analytical data mart within Marketplace<br>
The load is automated, so you will not be expected to know exactly what occurs.<br>
Here is an overview:<br>
Inbound Outbound -extracts and updates required data into standard structure<br>
Staging -<br>
 *manage slowly changing dimensions,<br>
 *generate surrogate keys and<br>
 *create skeleton records<br>

**SCD** - a dimension is considered a SCD when its attributes remain almost constant over time, requiring relatively minor alterations to represent the evolved state.<br>
**Surrogate Keys** - system-generated and non-persistent integer keys which replace foreign keys.<br>
**Skeleton records** - Generated when a foreign key in a fact table does not have a match in the dimension table. A dummy or ‘skeleton’ record is created in the dimension table.

There are following 3 Types of Data Loading Strategies :<br>
i. Initial load : Populating all the data tables from source system and loads it in to data warehouse table.<br>
ii. Incremental Load : Applying the ongoing changes as necessary in periodic manner.<br>
iii. Full Refresh : Completely erases the data from one or more tables and reload the fresh data.<br>

### Star and Snowflake schema
#### Star Schema
In the star schema design, a single object (the fact table) sits in the middle and is radically connected to other surrounding objects (dimension lookup tables) like a star.<br>
Each dimension is represented as a single table.<br>
The primary key in each dimension table is related to a foreign key in the fact table.<br>

All measures in the fact table are related to all the dimensions that fact table is related to.<br>
In other words, they all have the same level of granularity.<br>

A star schema can be simple or complex.<br>
A simple star consists of one fact table; a complex star can have more than one fact table.<br>

#### Snowflake Schema
It is an extension of star schema.<br>
In a star schema, each dimension is represented by a single dimensional table, whereas in a snowflake schema, that dimensional table is normalized into multiple lookup tables, each representing a level in the dimensional hierarchy.<br>

Adv -<br>
improvement in query performance due to minimized disk storage requirements and joining smaller lookup tables.<br>

Disadvantage-<br>
additional maintenance efforts needed due to the increase number of lookup tables.<br>

### Fact Table Granularity
The first step in designing a fact table is to determine the granularity of the fact table.<br>
By granularity, we mean the lowest level of information that will be stored in the fact table. <br>
This constitutes two steps:<br>

i. Determine which dimensions will be included - this depends on business process being targetted.<br>

ii. Determine where along the hierarchy of each dimension the information will be kept -<br>
This depends on requirements. Eg - if client wants hourly reports, then fact table will keep hour as lowest level of granularity.<br>
If daily reports are fine, then date_id is lowest level of granularity.<br>

The determining factors usually goes back to the requirements.<br>

#### Fact And Fact Table Types <br>
There are three types of facts:<br>
i. Additive: Additive facts are facts that can be summed up through all of the dimensions in the fact table.<br>
ii. Semi-Additive: Semi-additive facts are facts that can be summed up for some of the dimensions in the fact table, but not the others.<br>
iii. Non-Additive: Non-additive facts are facts that cannot be summed up for any of the dimensions present in the fact table.<br>

eg1 - Additive Fact -<br>
Consider a retailer fact table with following columns -<br>
- Date
- Store
- Product
- Sales_Amount

The purpose of this table is to record the sales amount for each product in each store on a daily basis.<br>

Sales_Amount is an additive fact, because you can sum up this fact along any of the three dimensions present in the fact table -- date, store, and product.<br>

eg2A - Semi-Additive Fact and Non-Additive Fact -<br>
Say we are a bank with the following fact table:<br>
- Date
- Account
- Current_Balance
- Profit_Margin

The purpose of this table is to record the current balance for each account at the end of each day, as well as the profit margin for each account for each day.<br>

Current_Balance and Profit_Margin are the facts.<br>

Current_Balance is a semi-additive fact, as it makes sense to add them up for all accounts (what's the total current balance for all accounts in the bank?), but it does not make sense to add them up through time (adding up all current balances for a given account for each day of the month does not give us any useful information).<br>

Profit_Margin is a non-additive fact, for it does not make sense to add them up for the account level or the day level.<br>

eg 2B -<br>
semi -additive - distinct customers who shopped in a day = semi additive.<br>
Across all stores, this number can be aggregated. For example, store A has 300 customers and store B has 200 customers. So total 500 customers.<br>
But cant add across date dimension. So no summation possible across days in a week.<br>

non-additive = %age loyalty transaction in a day.<br>
For example, store A has 30% sales as loyalty count, and store B has 40%.<br>
But we cant add these two figures to find overall loyalty sales.<br>

Based on the above classifications, there are two types of Fact TABLES:<br>
* Cumulative: This type of fact table describes what has happened over a period of time. For example, this fact table may describe the total sales by product by store by day. The facts for this type of fact tables are mostly additive facts. The first example presented here is a cumulative fact table.<br>
* Snapshot: This type of fact table describes the state of things in a particular instance of time, and usually includes more semi-additive and non-additive facts. The second example presented here is a snapshot fact table.<br>

### Slowly Changing Dimensions
The "Slowly Changing Dimension" problem is a common one particular to data warehousing. In a nutshell, this applies to cases where the attribute for a record varies over time.<br>
There are in general three ways to solve this type of problem, and they are categorized as follows:<br>

####Type 1
The new record replaces the original record. <br>
No trace of the old record exists. <br>
In other words, no history is kept.<br>
Advantage -<br>
* easiest to handle as no need to maintain history.<br>

Disadvantage-<br>
*History is lost. Cant track past behavior.<br>

So, Type 1 slowly changing dimension should be used when it is not necessary for the data warehouse to keep track of historical changes.<br>

####Type 2
A new record is added into the customer dimension table. <br>
Therefore, the customer is treated essentially as two people.<br>
Both the original and the new record will be present. <br>
The new record gets its own primary key.<br>

Advantages:<br>
- This allows us to accurately keep all historical information.<br>

Disadvantages:<br>
- This will cause the size of the table to grow fast. <br>
  In cases where the number of rows for the table is very high to start with, storage and performance can become a concern.<br>
- This necessarily complicates the ETL process.<br>

#### Type 3
The original record is modified to reflect the change.<br>
We add more column to track change. <br>
But this is feasible only if changes to be tracked are finite.<br>
For example, phone or address changes more than once will complicate things.<br>

### Data Integrity
Data integrity refers to the validity of data, meaning data is consistent and correct. <br>
In the data warehousing field, we frequently hear the term, "Garbage In, Garbage Out." <br>
If there is no data integrity in the data warehouse, any resulting report and analysis will not be useful.<br>

In a data warehouse or a data mart, there are 3 areas of where data integrity needs to be enforced:<br>

a. Database level<br>
We can enforce data integrity at the database level. <br>
Common ways of enforcing data integrity include:<br>

i. Referential integrity<br>
The relationship between the primary key of one table and the foreign key of another table must always be maintained. <br>
For example, a primary key cannot be deleted if there is still a foreign key that refers to this primary key.<br>

ii. Primary key / Unique constraint<br>
Primary keys and the UNIQUE constraint are used to make sure every row in a table can be uniquely identified.<br>

iii. Not NULL vs. NULL-able<br>
For columns identified as NOT NULL, they may not have a NULL value.<br>

iv. Valid Values<br>
Only allowed values are permitted in the database. <br>
For example, if a column can only have positive integers, a value of '-1' cannot be allowed.<br>

b. ETL process<br>
For each step of the ETL process, data integrity checks should be put in place to ensure that source data is the same as the data in the destination. <br>
Most common checks include record counts or record sums.<br>

c. Access level<br>
We need to ensure that data is not altered by any unauthorized means either during the ETL process or in the data warehouse. <br>
To do this, there needs to be safeguards against unauthorized access to data (including physical access to the servers), as well as logging of all data access history. <br>
Data integrity can only ensured if there is no unauthorized access to the data.<br>

4F. Factless Fact Table<br>
A factless fact table is a fact table that does not have any measures.<br>
It is essentially an intersection of dimensions.<br>
On the surface, a factless fact table does not make sense, since a fact table is, after all, about facts.<br>
However, there are situations where having this kind of relationship makes sense in data warehousing.<br>

eg1 - student class attendance record.<br>
In this case, the fact table would consist of 3 dimensions: the student dimension, the time dimension, and the class dimension. <br>
This factless fact table would look like the following:<br>
**Fact Table "school_attendance"**


| date_id    | classId | student_id |
|------------|---------|------------|
| 02-02-2020 | 1       | 101        |
| 02-02-2020 | 1       | 102        |
| 02-02-2020 | 1       | 103        |
----------------------------------
The only measure that you can possibly attach to each combination is "1" to show the presence of that particular combination. <br>
However, adding a fact that always shows 1 is redundant because we can simply use the COUNT function in SQL to answer the same questions.<br>

eg 2 - online sales in CRV. columns -<br>
date_id, store_id, till_id, pos_id<br>
In essence it contains only 1 column = basket_key.<br>
If a basket is in this table, it means its online sale, else offline sale.<br>

eg3 - Promotion data.<br>
Table structure could be -<br>
date_id | store_id| promo_type| promo_id| basket_key<br>

promo_type = Promotion can be online, in-store, flat discount, coupon, voucher, etc.<br>
Above table contains info of promotion applied on a basket.<br>
No measurable fact exists here.<br>
But why needed ?<br>
Transaction data contains info of what item was sold on promotion.<br>
But promotion data contains information of all the promotion during the purchase period.<br>
That is, all products having promotion applied on them, including those which were not sold in spite of promotion. And so, this promotion table becomes pivotal even though it contains no measurable fact.<br>

**Why need factless facts?** <br>
Factless fact tables offer the most flexibility in data warehouse design.<br>
For example, one can easily answer the following questions with this factless fact table:<br>
 * How many students attended a particular class on a particular day?<br>
 * How many classes on average does a student attend on a given day?<br>
Without using a factless fact table, we will need two separate fact tables to answer the above two questions.<br>
With the above factless fact table, it becomes the only fact table that's needed.<br>

#### Junk Dimension
There are columns in Fact table which can have only a few or 2 kind of values - true or false, 1 or 0, etc.<br>
eg =<br>
bulk Vs non-bulk<br>
online vs offline<br>
promo vs non-promo vs hybrid sale etc.<br>

From business point of view, capturing above info in Fact table is very important.<br>
Issue -having these info will only make our fact table bulky and eventually unmanageable.<br>

Soln - junk dimension.<br>
eg - CRV basket channel seg -<br>
shop_channel_code in 0,1,2 or 3 - covers both bulk/non-bulk and online/offline.<br>

this would reduce 2 columns in fact table to 1.<br>
we can expand scope of above column to include promo info, and in that way we replace 3 fact columns by 1.<br>
This will result in a data warehousing environment that offer better performance as well as being easier to manage.<br>

[reference] (https://www.1keydata.com/datawarehousing/junk-dimension.html)