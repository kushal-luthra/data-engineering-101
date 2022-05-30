# SQL Performance Tuning : Summary 

<br>


#### Tip 1: Never use *(Star) to fetch all records from table
Sql query become fast if you use actual columns instead of * to fetch all the records from the table.<br>

Not Recommended - <br>
```
Select * from Employee;
```
Recommended  <br>
```
Select Eno,Ename,Address from Employee;
```

<br>


#### Tip 2: Try to avoid DISTINCT keyword from the query
Try to avoid DISTINCT keyword from select statements.<br>
DISTINCT keyword has high cost and low performance.<br>
When anyone uses DISTINCT keyword, it first SORTS the data from column and then fetches the distinct values.<br>
Use EXISTS operator instead of DISTINCT keyword.<br>

Not Recommended:<br>
```
SELECT 
    DISTINCT 
             d.dept_no, 
             d.department_name
FROM Department d,Employee e
WHERE d.dept_no= e.dept_no;
```

Recommended:<br>
```	
SELECT 
    d.dept_no, 
    d.department_name
FROM Department d
WHERE EXISTS ( SELECT ‘X’ FROM Employee e WHERE d.dept_no= e.dept_no);
```

<br>


#### Tip 3: Carefully use WHERE conditions in sql
Try to use correct operator as per requirement given.

Not Recommended:<br>
```
Select * 
from Employee 
WHERE salary != 65000;
```

Recommended:<br>
```
Select * 
from Employee 
WHERE salary > 65000 and salary < 65000;
```

<br>


#### Tip 4: Use Like operator instead of equal to (=)
Not Recommended:<br>
```
Select * 
from Employee 
where name=’Amit’;
```

Recommended:<br>
```
Select * 
from Employee 
where name like ‘Amit%’;
```

<br>


#### Tip 5: Avoid HAVING clause/GROUP BY statements
HAVING clause and GROUP BY statements have high cost.<br>
So try to avoid it in sql query.<br>

Not Recommended - <br>
```
Select * 
from Employee 
WHERE name=’Amit’  
GROUP BY department 
HAVING salary=45000;
```
Recommended - <br>
```
Select * 
from Employee 
WHERE name=’Amit’ and salary=45000;
```

More added: - <br>
Having clause- <br>
We use Having clause to eliminate some of the group values. <br>
Issue – Having clause restricts the rows AFTER they are read. <br> 
So if no restriction in “where clause”,  optimizer will use full table scan. <br>
This is really important coz all predicates in the HAVING Clause will not be used as access predicates. So they will not make optimizer use indexes, partitions etc. <br>
This is because to perform HAVING clause, it first reads all the rows and then eliminates unnecessary rows. <br>
 
<br>


#### Tip 6: Use of EXISTS and IN Operators
Basically, Operator **IN** has lowest performance.<br>
**IN** operator is used when Filter criteria is in subquery, whereas **EXISTS** operator is used when filter criteria is in main query.<br>

Example:<br>
**IN Operator** <br>
```
Select * 
from Employee 
where Department_name IN (
                          Select Department_name 
                          from Department 
                          where Dno=10);
```

**Exist operator** <br>
```
Select * 
from Employee 
where EXISTS (
               Select Department_name 
               from Department 
               where Dno=10);
```

**More added** <br>
When you run a query with IN clause, database will process it in below format –<br>
That is, in case of use of IN clause, each value of sub query is joined with outer query.<br>
Treats below Query -<br>
```
select * from T1 where x in (select x from T2);
```
as -<br>
```
select * from t1, (select x from t2) T2 where t1.x = t2.x;
```

But when you use EXIST clause, database **will exit as soon as it gets the first match**.<br>
So, in case of EXIST clause it runs executes query in below format –<br>
Treats below query -<br>
```
select * from T1 where exists (select x from T2 where t1.x=t2.x);
```
as -<br>
```
FOR x  IN (select * from t1) LOOP
	IF (EXISTS ( SELECT X FROM T2) ) THEN
		OUTPUT THE RECORD
	END IF;
END;
```

That is, using EXIST clause will imply database will run it like a FOR loop and as soon as match is found, it moves to next record.<br>

**So which one is faster – IN or EXIST?**<br>
a. This totally depends on situation.<br>
Use IN when - outer table = Big and Subquery = Small<br>
Use EXISTS when – outer table = Small and Subquery = Big<br>

b. Even above rules are not fixed.<br>
For example, if subquery has bigger table, but it has an index, in this case use of IN is suggested.<br>

c. So-<br>
EXISTS doesn’t work better than IN all the times.<br>
IN is better than EXISTS if –<br>
outer table = Big and Subquery = Small<br>
outer table = Small and Subquery = Big + Indexed<br>

**NOT EXISTS vs NOT IN** <br>
•	NOT EXISTS is not equivalent of NOT IN.<br>
•	NOT EXISTS cannot be used instead of NOT IN all the times. <br>
•	More specifically, if there is any NULL value in your data, they will show different result.<br>
•	If your subquery returns even one NULL value, NOT IN will not match any rows.<br>
•	On other hand, if you have a chance to use NOT EXISTS instead of NOT IN, you should test it.<br>
•	In most database versions of oracle, EXISTS and IN are treated similarly in terms of execution plan.<br>

<br>


#### Tip 7: Try to use UNION ALL instead of UNION 
UNION scans all data first and then eliminate duplicate so it has slow performance.<br>

Not Recommended<br>
```
Select * from Employee where dept_no=10
UNION
Select * from Employee where dept_no=20;
```
Recommended <br>
```
Select * from Employee where dept_no=10
UNION  ALL
Select * from Employee where dept_no=20;
```

<br>


#### Tip 8: Avoid use of Functions in Where condition.
Not Recommended <br>
```
Select * from Employee where Substr(name,1,3)=’Ami’;
```

Recommended <br>
```
Select * from Employee where name like ‘Ami%’;
```

<br>


#### Tip 9: convert OR to AND
If we use OR clause, it will PREVENT index usages.<br>
Instead, we should use AND where possible.<br>
Not Recommended <br>
```
select * from sales where prod_id = 13 or promo_id=14;
```

Recommended <br>
```
select * from sales where prod_id = 13
UNION All
select * from sales where promo_id=14 AND prod_id <> 13;
```

<br>


#### Tip 10: Subquery Unnesting 
Nested queries are very costly, and so transformer tries to convert them to equivalent join statements.<br>
Not Recommended <br>
```
select * from sales 
where cust_id IN (select cust_id from customers);
```

Recommended <br>
```
select sales.* from sales, customers 
where sales.cust_id=customers.cust_id;
```

<br>


#### Tip 11: IN and BETWEEN
```
select * from employees where emp_id in (2,3,4,5); 
```
The above is equivalent to 
```
select * from employees 
where emp_id = 2 OR emp_id=3 OR emp_id=4 OR emp_id=5
```
---this implies full table scan.

Solution -
```
select * from employees where emp_id between 2 and 5;
``` 

<br>


#### Tip 12: Fetching first N records
Suppose we want to see only 10 records in our select statement output.<br> 
There are 2 ways to do this –<br>
Using rownum  (Recommended)
```
SELECT * FROM EMPLOYEE where rownum<11;
```
Using fetch first (not recommended)
```
SELECT * FROM EMPLOYEE FETCH FIRST 10 ROWS ONLY;
```

In case of rownum- <br>
Here it reads first 10 rows use count STOPKEY operator, and so faster than fetch first method.<br>

In case of fetch first –<br>
Here we read whole table, and then applied a windowing function to select 1st 10 records.<br>

<br>

#### Tip 13: UNION vs UNION ALL:
UNION – combines data and drops duplicate rows.<br>
UNION ALL – combines data and retains duplicate rows.<br><br>
Suggest: Some key points-<br>
•	by default, UNION ALL is less costly than UNION, as latter sorts data internally to remove duplicates.<br>
•	But if your table is indexed, then sort operation in UNION wont be that costly, and so you can use UNION also.<br>
So –<br>
-	Use UNION if table is indexed and you don’t want duplicates in output.
-	Use UNION ALL if–<br>
    -	There is no duplicate in your data, or<br>
    -	You are ok with having duplicate data in output.<br>
-	But overall, UNION ALL gives better performance than UNION.<br>

<br>


#### Tip 14: INTERSECT Vs EXISTS operator
Intersect gives common rows of 2 intersection in a Sorted order.<br>
As part of intersect, 2 rows sources are first sorted, and then common records are fetched.<br>

In place of INTERSECT operator, we should try and use EXISTS clause, which is more efficient.<br> 
One caveat is that, in case of EXISTS clause, output is not sorted, unlike in case of INTERSECT clause.

Not Recommended <br>
```
SELECT employee_id 
FROM employees 
where employee_id between 145 and 179
INTERSECT
SELECT employee_id 
FROM employees 
WHERE first_name LIKE 'A%';
```
Recommended <br>
```
SELECT employee_id 
FROM employees e 
where employee_id between 145 and 179
and exists
         (SELECT employee_id 
          FROM employees t 
          WHERE first_name LIKE 'A%' and e.employee_id = t.employee_id);
```

<br>


#### Tip 15: MINUS Vs NOT EXISTS
MINUS operator eliminates matched rows of 1st (with 2nd) and returns rest of the rows of 1st. <br>
NOT EXISTS clause can also do same work as MINUS, but has much better performance. <br><br>
Not Recommended <br>
```
SELECT employee_id FROM employees where employee_id between 145 and 179
MINUS
SELECT employee_id FROM employees WHERE first_name LIKE 'A%';
``` 
Recommended <br>
```
SELECT employee_id FROM employees e where employee_id between 145 and 179
and not exists
(SELECT employee_id FROM employees t WHERE first_name LIKE 'A%' and e.employee_id = t.employee_id);
```

<br>


#### Tip 16: Using Like conditions
To enable use of indexes, avoid use of wild card character at beginning of source text scan.<br>
If you are forced to use wild card character at beginning, you can create reverse index, and handle that problem using that index. <br>
In this case, when you reverse index, then source text will eliminate use of wild card at beginning.<br>
Eg – <br>
Suppose you want to find records where last name ends is “hhar”, then create reverse() index on last_name to reverse it and then use condition where reversed last name begins with “rahh”. <br>
Though reverse() index usage will have cost, but if your column is selective enough, it wont be much cost.

<br>


#### Tip 17: Using Functions on Indexed Columns will suppress index usage.
Use of function on indexed column will suppress index usage.<br>
So, rewrite query to avoid use of function.<br>
BAD QUERY
```
	select employee_id, first_name, last_name
	from employees 
	where trunc(hire_date,'YEAR') = '01-JAN-2002';
```
GOOD: rewritten query
```
	select employee_id, first_name, last_name
	from employees 
	where hire_date between '01-JAN-2002' and '31-DEC-2002';
```
Eg2 -<br>
Bad<br>
```
select * from mytable where substr(emp_name,1,2) = 'Po';
```
Good<br> 
```
select * from mytable where emp_name like 'Po%';
```

Note -<br> 
In Spark, we have HashAggregates and SortAggregates.<br> 
Hash Aggregates are more performant, and work only on mutable data types.<br>
That is, if all elements in your Select clause (except those in Group by clause) are mutable types like INT, FLOAT, etc, then spark will use Hash Aggregates.<br> 
This means, sometimes, for performance gain, we need to apply Function to transform values.<br>

See #14 at below link for details <br>
https://github.com/kushal-luthra/spark-development/blob/master/notes/spark_opimization.md

<br>

#### Tip 18: Handling NULL Values
Failing to deal with NULL values will lead to unintentional results or performance losses.<br>
Why -<br>
•B-Tree indexes do not index NULL values.<br>
•If there are any NULL values in your indexed columns and you need to get rows which have NULL values, optimizer will not use your index, and perform a full table scan instead.<br>
•That is, having Null values in your index may sometimes suppress index usage.<br>

**Ways to handle NULL value-based performance loss** <br>
a. Use IS NOT NULL condition in your WHERE clause. <br> 
Use IS NOT NULL condition in your WHERE clause if you don’t need to have NULL values in result set.<br> 
That is, even if you now your result will not be having any NULL values, you should use “is not null “ clause to make optimizer use indexes. <br> 
Eg –
Query 1: 
```
select emp_name, emp_id 
from employee 
where emp_id <> 1;
``` 
Query 2: 
```
select emp_name, emp_id 
from employee 
where emp_id <> 1 and emp_id is not null;
```
In query 1, we will see FULL Table scan and in case of query 2, we see index-based scan, and lower query cost.<br> 

b. Adding not null constraint to your columns and insert a specific value for NULL values like ‘0’ if value in a column is null. <br>

c. If reasonable, create a BITMAP index instead of B-Tree index. <br>
BITMAP indexes store NULL values. <br>
So even if there are null values in our column, optimizer will be able to use our indexes.<br> 
However, you need to take into consideration index efficiency between B-Tree and BITMAP, as former as more efficient than latter.<br>
- We use BITMAP indexes when – cardinality is LOW and index not modified often. <br>
- We use B-Tree index when – cardinality/selectivity  is high.<br>

#### Tip 19 : Use Truncate instead of Delete
**Truncate** is always faster than **DELETE** command.  <br>
This is because when you run delete command, oracle database generates lots of UNDO data for deleted rows and generating UNDO data is an expensive operation. <br>
Truncate doesn’t generate UNDO Data. <br>

But before using Truncate command, there are few things to note about it- <br>
•	No rollback –  <br> Truncate operation cannot be rollbacked, and Flashback is also not so easy after truncate operations. <br>
    You may need to use Flashback Data Archive or some other external tools in this case. <br> <br>
•	Truncate is a DDL operation –  <br> So when you run Truncate, your transaction will be committed.  <br>It performs commit before and after Truncate operation.  <br>Since it does 2 commits, and even if truncate operation fails in between, the changes you did before will be permanent in any case. <br> <br>
•	Truncate a partition -  <br>We don’t need to truncate whole table all the times. You can truncate partition as well. <br> <br>
•	Truncate doesn’t fire DML triggers -  <br> So you wont be able to log your truncate operation because of that.  <br>But it can fire the DDL triggers. <br> <br>
•	Truncate makes unusable indexes usable again.  But delete does not. <br>
<br>

#### Tip 20: Data Type Mismatches 
If data types of column and compared value dont match, this may suppress index usage.<br>
```
select cust_id, cust_code from customers where cust_code = 101;

Vs

select cust_id, cust_code from customers where cust_code = '101';
```
<br>

#### Tip 21: Tuning Ordered queries- Order By clause 
Order by mostly **requires SORT operations**.<br>
This sort operation is done in PGA or in disk (if PGA doesn’t have enough memory)<br>
This disk is shown as ‘temporary table space’ in execution plan.<br>
Issue – sorting in disk is a costly operation.<br>

Solution –<br>
• Create a B-Tree index on column used in Order by Clause, or<br>
• Modify a B-Tree index to include column used in Order by Clause.<br>
• Why–>  B-Tree indexes store columns in Order and using B-Tree index will eliminate sort operations.<br>

<br>


#### Tip 22 : Retrieving MIN and MAX Values
B-Tree indexes increase the performance a lot for min and max value searches.<br>
If no B-Tree index, optimizer will need to read whole table.<br>

If our query has another column or another aggregate function in your query, it will be reading whole index or whole table.<br>
For example-<br>
When you see below, if we are looking for min() and max() values individually, output is just 2 for each.<br>
But when we want to get min() and max() together, database will read full table, and hence cost is 8 times. <br>This is coz we have 2 aggregate functions in our query.<br>
Bad - <br>
```
	select min(), max() from mytable;
```
Good - <br>
```
	select * FROM 
	    (select min() from mytable) min_cust, 
	    (select max() from mytable) max_cust;
```

<br>


#### Tip 23 : Views
Simple view = view created from single table. <br>
Complex view = view created by using multiple tables. <br>
Some suggestions w.r.t. views- <br>
1.	If you don’t need to use all the tables in a view, then don’t use the view.  <br>Instead use the actual tables.  <br>Otherwise, server will have to join all tables, do aggregation etc on them for a view. i.e. use view for the purpose for which it was created. <br> <br>
2.	Else create another view.  <br> <br>
3.	Don’t join complex views with a table or another view - <br> This is because most of the times view is first executed completely at first, and then result is used as row source to other table or view.  <br>So, in this case you be reading lots of unnecessary data and performing unnecessary join and group by.  <br>This will increase cost a lot. <br> <br>
4.	Avoid performing outer join to the views –  <br> because if you use equality predicate on view column, the optimizer gets wrong if the table of that column has an outer join in the view as well.  <br> Because outer join may not know performance of view and may lead to bad execution as well. <br>
    E.g. – if we do outer join, optimizer may not be able to push predicate inside the view definition at times of execution plan. <br> <br>


Materialized Views- <br>
Unlike basic and complex views, materialized views store both query and data. <br>
Materialized view data can be refreshed manually or via auto-refresh. <br>
But materialized view maintenance is a burden to database – it needs to be kept up to date for each modification on each change. <br>
As compared to normal views, materialized view will improve performance as we will select data directly from materialized view, and there will be no sorts, joins etc. <br>
We can create index, partitions etc on materialized view like in an ordinary table. <br>

**Summary** <br>
 • If you don’t need to use all the tables in a view, then don’t use the view. Instead use the actual tables.<br> 
 • Don’t join complex views with a table or another view. <br>
 • Avoid performing outer join to the views. <br>
 • Use Materialized View -  <br> Unlike basic and complex views, materialized views store both query and data. Materialized view data can be refreshed manually or via auto-refresh. But materialized view maintenance is a burden to database .<br>
 
<br>


#### Tip 24: Frequent commit is not desired
 * make REDO logs bulky as we may be committing prior to period. <br>
 * make lock on modified rows, making them unavailable to other applications.<br>

 <br>


#### Tip 25: Multitable DML operations (skip for big data)
Sometimes we have to read same table as input to different tables in our data warehouse. <br>
So, if we have 5 different tables requiring input from 1 table, we should ideally be reading input table just once, and keep on feeding into different output tables as per requirements. <br>
For this we have 2 options – <br>
•	INSERT ALL <br>
•	MERGE INTO <br>

