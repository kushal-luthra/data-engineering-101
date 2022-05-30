# SQL Performance Tuning : Summary 

#### Tip 1: Never use *(Star) to fetch all records from table

#### Tip 2: Try to avoid DISTINCT keyword from the query <br>
Not Recommended:

```
SELECT DISTINCT d.dept_no, d.department_name
	FROM Department d,Employee e
	WHERE d.dept_no= e.dept_no;
```

Recommended:<br>
```	
SELECT 
    d.dept_no d.department_name
FROM Department d
WHERE EXISTS ( SELECT ‘X’ FROM Employee e WHERE d.dept_no= e.dept_no);
```

#### Tip 3: Carefully use WHERE conditions in sql. <br>

#### Tip 4: Use Like operator instead of equal to (=)
 
#### Tip 5: Avoid HAVING clause/GROUP BY statements
 
#### Tip 6: Use of EXISTS and IN Operators

#### Tip 7: Try to use UNION ALL instead of UNION as UNION scans all data first and then eliminate duplicate so it has slow performance.

#### Tip 9: convert OR to AND

#### Tip 10: Subquery Unnesting 

#### Tip 11: IN and BETWEEN

#### Tip 12: Fetching first N records:  SELECT * FROM EMPLOYEE where rownum<11

#### Tip 13: UNION vs UNION ALL:
 • by default, UNION ALL is less costly than UNION, as latter sorts data internally to remove duplicates.<br>
 • But if your table is indexed, then sort operation in UNION wont be that costly, and so you can use UNION also.<br>

#### Tip 14: INTERSECT Vs EXISTS operator
 
#### Tip 15: MINUS Vs NOT EXISTS

#### Tip 16: Using Like conditions
To enable use of indexes, avoid use of wild card character at beginning of source text scan.<br>
If you are forced to use wild card character at beginning, you can create reverse index, and handle that problem using that index. In this case, when you reverse index, then source text will eliminate use of wild card at beginning.<br>

#### Tip 17: Using Functions on Indexed Columns will suppress index usage.
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

#### Tip 18: Handling NULL Values
•B-Tree indexes do not index NULL values.<br>
•If there are any NULL values in your indexed columns and you need to get rows which have NULL values, optimizer will not use your index, and perform a full table scan instead.<br>
•That is, having Null values in your index may sometimes suppress index usage.<br>

Solution -<br>
- Use IS NOT NULL condition in your WHERE clause. <br> 
- Adding not null constraint to your columns and insert a specific value for NULL values like ‘0’ if value in a column is null. <br>
- If reasonable, create a BITMAP index instead of B-Tree index. <br> 

#### Tip 19 : Use Truncate instead of Delete
 
#### Tip 20: Data Type Mismatches 
If data types of column and compared value dont match, this may suppress index usage.<br>

#### Tip 21: Tuning Ordered queries- Order By clause 
Order by mostly requires sort operations.<br>
This sort operation is done in PGA or in disk (if PGA doesn’t have enough memory)<br>
This disk is shown as ‘temporary table space’ in execution plan.<br>
Issue – sorting in disk is a costly operation.<br>

Solution –<br>
• Create a B-Tree index on column used in Order by Clause, or<br>
• Modify a B-Tree index to include column used in Order by Clause.<br>
• Why – B-Tree indexes store columns in Order and using B-Tree index will eliminate sort operations.<br>

#### Tip 22 : Retrieving MIN and MAX Values
B-Tree indexes increase the performance a lot for min and max value searches.<br>
If no B-Tree index, optimizer will need to read whole table.<br>
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
#### Tip 23 : Views
 • If you don’t need to use all the tables in a view, then don’t use the view. Instead use the actual tables.<br> 
 • Don’t join complex views with a table or another view. <br>
 • Avoid performing outer join to the views. <br>
 • Unlike basic and complex views, materialized views store both query and data. Materialized view data can be refreshed manually or via auto-refresh. But materialized view maintenance is a burden to database .<br>
 
#### Tip 24: Frequent commit is not desired: 
 * make REDO logs bulky as we may be committing prior to period.<br>
 * make lock on modified rows, making them unavailable to other applications.<br>