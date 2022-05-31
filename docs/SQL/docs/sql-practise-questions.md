Leetcode SQL

1. The Most Recent Three Orders
Table: Customers
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+
customer_id is the primary key for this table.
This table contains information about customers.
 

Table: Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| cost          | int     |
+---------------+---------+
order_id is the primary key for this table.
This table contains information about the orders made customer_id.
Each customer has one order per day.
``` 

Write an SQL query to find the most recent 3 orders of each user. <br>
If a user ordered less than 3 orders return all of their orders.<br>
Return the result table sorted by customer_name in ascending order and in case of a tie by the customer_id in ascending order. <br>
If there still a tie, order them by the order_date in descending order.<br>
The query result format is in the following example:<br>
```
Customers
+-------------+-----------+
| customer_id | name      |
+-------------+-----------+
| 1           | Winston   |
| 2           | Jonathan  |
| 3           | Annabelle |
| 4           | Marwan    |
| 5           | Khaled    |
+-------------+-----------+

Orders
+----------+------------+-------------+------+
| order_id | order_date | customer_id | cost |
+----------+------------+-------------+------+
| 1        | 2020-07-31 | 1           | 30   |
| 2        | 2020-07-30 | 2           | 40   |
| 3        | 2020-07-31 | 3           | 70   |
| 4        | 2020-07-29 | 4           | 100  |
| 5        | 2020-06-10 | 1           | 1010 |
| 6        | 2020-08-01 | 2           | 102  |
| 7        | 2020-08-01 | 3           | 111  |
| 8        | 2020-08-03 | 1           | 99   |
| 9        | 2020-08-07 | 2           | 32   |
| 10       | 2020-07-15 | 1           | 2    |
+----------+------------+-------------+------+

Result table:
+---------------+-------------+----------+------------+
| customer_name | customer_id | order_id | order_date |
+---------------+-------------+----------+------------+
| Annabelle     | 3           | 7        | 2020-08-01 |
| Annabelle     | 3           | 3        | 2020-07-31 |
| Jonathan      | 2           | 9        | 2020-08-07 |
| Jonathan      | 2           | 6        | 2020-08-01 |
| Jonathan      | 2           | 2        | 2020-07-30 |
| Marwan        | 4           | 4        | 2020-07-29 |
| Winston       | 1           | 8        | 2020-08-03 |
| Winston       | 1           | 1        | 2020-07-31 |
| Winston       | 1           | 10       | 2020-07-15 |
+---------------+-------------+----------+------------+
Winston has 4 orders, we discard the order of "2020-06-10" because it is the oldest order.
Annabelle has only 2 orders, we return them.
Jonathan has exactly 3 orders.
Marwan ordered only one time.
We sort the result table by customer_name in ascending order, by customer_id in ascending order and by order_date in descending order in case of a tie.
```

Ans.
```
select 
customer_name, 
customer_id, 
order_id, 
order_date  

from 
(select
    a.name as customer_name,
    a.customer_id,
    b.order_id,
    to_char(b.order_date, 'YYYY-MM-DD') as order_date,
    rank() over ( partition by a.customer_id order by b.order_date desc)   date_rank
    from 
    customers a inner join orders b 
    on a.customer_id = b.customer_id 
    order by a.customer_id, b.order_date  
)
where date_rank<=3
order by customer_name, customer_id, order_date desc
;
```

2A. Shortest Distance in a Plane<br>
https://leetcode.com/problems/shortest-distance-in-a-plane/ <br>
Table point_2d holds the coordinates (x,y) of some unique points (more than two) in a plane.<br>
Write a query to find the shortest distance between these points rounded to 2 decimals.<br>
 

| x  | y  |
|----|----|
| -1 | -1 |
| 0  | 0  |
| -1 | -2 |
 

The shortest distance is 1.00 from point (-1,-1) to (-1,2). So the output should be:

| shortest |
|----------|
| 1.00     |
 

Note: The longest distance among all the points are less than 10000.<br>

Ans.<br>
```
select min(dist) as shortest from 
	(
	select
		 a.x as x1,
		 a.y as y1,
		 b.x as x2,
		 b.y as y2,
		round(sqrt((a.x - b.x)*(a.x - b.x) + (a.y - b.y)*(a.y - b.y) ),2) as dist
	from point_2d a, point_2d b 
	where concat(a.x,a.y)!=concat(b.x,b.y) -- this bit of matching coordinates is IMP to ensure same coordinates are not being captured.
	);
```

3. Investments in 2016 <br>
https://leetcode.com/problems/investments-in-2016/ <br>

Write a query to print the sum of all total investment values in 2016 (TIV_2016), to a scale of 2 decimal places, for all policy-holders who meet the following criteria:<br>
- Have the same TIV_2015 value as one or more other policyholders.<br>
- Are not located in the same city as any other policyholder (i.e.: the (latitude, longitude) attribute pairs must be unique).<br>

Input Format:<br>

The insurance table is described as follows:<br>

| Column Name | Type          |
|-------------|---------------|
| PID         | INTEGER(11)   |
| TIV_2015    | NUMERIC(15,2) |
| TIV_2016    | NUMERIC(15,2) |
| LAT         | NUMERIC(5,2)  |
| LON         | NUMERIC(5,2)  |
where PID is the policyholder's policy ID, TIV_2015 is the total investment value in 2015, TIV_2016 is the total investment value in 2016, LAT is the latitude of the policy holder's city, and LON is the longitude of the policy holder's city.<br>

Sample Input<br>

| PID | TIV_2015 | TIV_2016 | LAT | LON |
|-----|----------|----------|-----|-----|
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |

Sample Output<br>

| TIV_2016 |
|----------|
| 45.00    |
Explanation

The first record in the table, like the last record, meets both of the two criteria.<br>
The TIV_2015 value '10' is as the same as the third and forth record, and its location unique.<br>

The second record does not meet any of the two criteria. Its TIV_2015 is not like any other policyholders.<br>

And its location is the same with the third record, which makes the third record fail, too.<br>

So, the result is the sum of TIV_2016 of the first and last record, which is 45.<br>

Ans.<br>

```
select round(sum(tiv_2016),2) as "TIV_2016" from (
select
    pid,
    count(pid) over (partition by tiv_2015) as tiv_2015_count,
    tiv_2016,
    count(*) over (partition by concat(concat(lat,'_'),lon)) as locn_count
from
    insurance order by lat,lon) where locn_count=1 and tiv_2015_count>1;
```
	
4. Calculate Salaries <br>
https://leetcode.com/problems/calculate-salaries/ <br>

Table Salaries:<br>

| Column Name   | Type    |
|---------------|---------|
| company_id    | int     |
| employee_id   | int     |
| employee_name | varchar |
| salary        | int     |

(company_id, employee_id) is the primary key for this table.<br>
This table contains the company id, the id, the name and the salary for an employee.<br>

Write an SQL query to find the salaries of the employees after applying taxes.<br>

The tax rate is calculated for each company based on the following criteria:<br>

0% If the max salary of any employee in the company is less than 1000$.<br>
24% If the max salary of any employee in the company is in the range [1000, 10000] inclusive.<br>
49% If the max salary of any employee in the company is greater than 10000$.<br>
Return the result table in any order. Round the salary to the nearest integer.<br>

The query result format is in the following example:<br>
``` 
Salaries table:
+------------+-------------+---------------+--------+
| company_id | employee_id | employee_name | salary |
+------------+-------------+---------------+--------+
| 1          | 1           | Tony          | 2000   |
| 1          | 2           | Pronub        | 21300  |
| 1          | 3           | Tyrrox        | 10800  |
| 2          | 1           | Pam           | 300    |
| 2          | 7           | Bassem        | 450    |
| 2          | 9           | Hermione      | 700    |
| 3          | 7           | Bocaben       | 100    |
| 3          | 2           | Ognjen        | 2200   |
| 3          | 13          | Nyancat       | 3300   |
| 3          | 15          | Morninngcat   | 1866   |
+------------+-------------+---------------+--------+

Result table:
+------------+-------------+---------------+--------+
| company_id | employee_id | employee_name | salary |
+------------+-------------+---------------+--------+
| 1          | 1           | Tony          | 1020   |
| 1          | 2           | Pronub        | 10863  |
| 1          | 3           | Tyrrox        | 5508   |
| 2          | 1           | Pam           | 300    |
| 2          | 7           | Bassem        | 450    |
| 2          | 9           | Hermione      | 700    |
| 3          | 7           | Bocaben       | 76     |
| 3          | 2           | Ognjen        | 1672   |
| 3          | 13          | Nyancat       | 2508   |
| 3          | 15          | Morninngcat   | 5911   |
+------------+-------------+---------------+--------+
For company 1, Max salary is 21300. Employees in company 1 have taxes = 49%
For company 2, Max salary is 700. Employees in company 2 have taxes = 0%
For company 3, Max salary is 7777. Employees in company 3 have taxes = 24%
The salary after taxes = salary - (taxes percentage / 100) * salary
For example, Salary for Morninngcat (3, 15) after taxes = 7777 - 7777 * (24 / 100) = 7777 - 1866.48 = 5910.52, which is rounded to 5911.
```
Ans.

```
select 
company_id,
employee_id,
employee_name,
round(
	salary*(case 
		when max_sal_per_company<1000 then 1 
		when max_sal_per_company>=1000 and max_sal_per_company<=10000 then 0.76
		else 0.51 
		end
		),0) as salary
from
(
select 
 company_id , employee_id , employee_name , salary, 
 max(salary) over (partition by company_id ) as max_sal_per_company
 from salaries);
 
 ```
 
 5. Countries You Can Safely Invest In<br>
 https://leetcode.com/problems/countries-you-can-safely-invest-in/ <br>
 
 Table Person:

| Column Name    | Type    |
|----------------|---------|
| id             | int     |
| name           | varchar |
| phone_number   | varchar |

id is the primary key for this table.<br>
Each row of this table contains the name of a person and their phone number.<br>
Phone number will be in the form 'xxx-yyyyyyy' where xxx is the country code (3 characters) and yyyyyyy is the phone number (7 characters) where x and y are digits. <br>
Both can contain leading zeros.<br>

Table Country:

| Column Name    | Type    |
|----------------|---------|
| name           | varchar |
| country_code   | varchar |

country_code is the primary key for this table.<br>
Each row of this table contains the country name and its code. country_code will be in the form 'xxx' where x is digits.<br>
 

Table Calls:<br>

| Column Name | Type |
|-------------|------|
| caller_id   | int  |
| callee_id   | int  |
| duration    | int  |



There is no primary key for this table, it may contain duplicates.<br>
Each row of this table contains the caller id, callee id and the duration of the call in minutes. caller_id != callee_id<br>
A telecommunications company wants to invest in new countries. The company intends to invest in the countries where the average call duration of the calls in this country is strictly greater than the global average call duration.<br>

Write an SQL query to find the countries where this company can invest.<br>

Return the result table in any order.<br>

The query result format is in the following example.<br>
```
Person table:
+----+----------+--------------+
| id | name     | phone_number |
+----+----------+--------------+
| 3  | Jonathan | 051-1234567  |
| 12 | Elvis    | 051-7654321  |
| 1  | Moncef   | 212-1234567  |
| 2  | Maroua   | 212-6523651  |
| 7  | Meir     | 972-1234567  |
| 9  | Rachel   | 972-0011100  |
+----+----------+--------------+

Country table:
+----------+--------------+
| name     | country_code |
+----------+--------------+
| Peru     | 051          |
| Israel   | 972          |
| Morocco  | 212          |
| Germany  | 049          |
| Ethiopia | 251          |
+----------+--------------+

Calls table:
+-----------+-----------+----------+
| caller_id | callee_id | duration |
+-----------+-----------+----------+
| 1         | 9         | 33       |
| 2         | 9         | 4        |
| 1         | 2         | 59       |
| 3         | 12        | 102      |
| 3         | 12        | 330      |
| 12        | 3         | 5        |
| 7         | 9         | 13       |
| 7         | 1         | 3        |
| 9         | 7         | 1        |
| 1         | 7         | 7        |
+-----------+-----------+----------+

Result table:
+----------+
| country  |
+----------+
| Peru     |
+----------+
The average call duration for Peru is (102 + 102 + 330 + 330 + 5 + 5) / 6 = 145.666667
The average call duration for Israel is (33 + 4 + 13 + 13 + 3 + 1 + 1 + 7) / 8 = 9.37500
The average call duration for Morocco is (33 + 4 + 59 + 59 + 3 + 7) / 6 = 27.5000 
Global call duration average = (2 * (33 + 3 + 59 + 102 + 330 + 5 + 13 + 3 + 1 + 7)) / 20 = 55.70000
Since Peru is the only country where average call duration is greater than the global average, it's the only recommended country.
```
Ans.
```
with call_table as (select 
    distinct
        caller_id, 
        callee_id,
        duration
from
calls
)
select distinct country from (
								select 
								t.name as country,
								c.country_code,
								avg(c.duration) over (partition by c.country_code) as country_avg,
								avg(c.duration) over () as overall_avg
								from (
											select 
												substr(b.phone_number,1,3) as country_code,
												a.duration 
												from 
													call_table a 
												left join 
													Person b 
												on a.caller_id = b.id 
											union all
											select 
												substr(b.phone_number,1,3) as country_code,
												a.duration 
												from 
													call_table a 
												left join 
													Person b 
												on a.callee_id = b.id
									) c
									  left join Country t
									  on trim(c.country_code) = trim(t.country_code)
								) 
								where country_avg>overall_avg;
```

5. Rectangles Area<br>
https://leetcode.com/problems/rectangles-area/ <br>
Table: Points<br>

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| x_value       | int     |
| y_value       | int     |
+---------------+---------+
id is the primary key for this table.<br>
Each point is represented as a 2D Dimensional (x_value, y_value).<br>
Write an SQL query to report of all possible rectangles which can be formed by any two points of the table.<br> 

Each row in the result contains three columns (p1, p2, area) where:<br>

p1 and p2 are the id of two opposite corners of a rectangle and p1 < p2.<br>
Area of this rectangle is represented by the column area.<br>
Report the query in descending order by area in case of tie in ascending order by p1 and p2.<br>

```
Points table:
+----------+-------------+-------------+
| id       | x_value     | y_value     |
+----------+-------------+-------------+
| 1        | 2           | 8           |
| 2        | 4           | 7           |
| 3        | 2           | 10          |
+----------+-------------+-------------+

Result table:
+----------+-------------+-------------+
| p1       | p2          | area        |
+----------+-------------+-------------+
| 2        | 3           | 6           |
| 1        | 2           | 2           |
+----------+-------------+-------------+

p1 should be less than p2 and area greater than 0.
p1 = 1 and p2 = 2, has an area equal to |2-4| * |8-7| = 2.
p1 = 2 and p2 = 3, has an area equal to |4-2| * |7-10| = 6.
p1 = 1 and p2 = 3 It's not possible because the rectangle has an area equal to 0.
```

Ans.

```	
-- Approach 1:
select 
	a.id as P1,
	b.id as P2,
	abs(a.x_value - b.x_value)*abs(a.y_value - b.y_value) as area
from
	points a, points b
where 	
	a.id < b.id   and 
	abs(a.x_value - b.x_value)*abs(a.y_value - b.y_value)>0
order by 
		abs(a.x_value - b.x_value)*abs(a.y_value - b.y_value) desc, 
		a.id, 
		b.id; 
-- Approach 2:
select     
    a.p1, b.p2, 
    abs(a.x1-b.x2)*abs(a.y1-b.y2) as area
from     
(select 
    id as P1,
    x_value as x1,
    y_value as y1,
    concat(x_value, y_value) as p1_coordinate
from points) a, 
(select 
    id as P2,
     x_value as x2,
    y_value as y2,
    concat(x_value, y_value) as p2_coordinate
from points    ) b
where 
    p1<p2
    and
    p1_coordinate!=p2_coordinate --no duplicate/same point
    and 
    abs(x1-x2)*abs(y1-y2)>0
order by abs(a.x1-b.x2)*abs(a.y1-b.y2) desc, a.p1, b.p2		
```

6. Active users : Imp -- date diff between consecutive days. <br>
https://leetcode.com/problems/active-users/		 <br>

Table Accounts: <br>

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |

the id is the primary key for this table. <br>
This table contains the account id and the user name of each account. <br>
 

Table Logins: <br>

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| login_date    | date    |
+---------------+---------+
There is no primary key for this table, it may contain duplicates. <br>
This table contains the account id of the user who logged in and the login date. A user may log in multiple times in the day. <br>
 

Write an SQL query to find the id and the name of active users. <br>

Active users are those who logged in to their accounts for 5 or more consecutive days. <br>

Return the result table ordered by the id. <br>

The query result format is in the following example: <br>

Accounts table:

| id | name     |
|----|----------|
| 1  | Winston  |
| 7  | Jonathan |

Logins table: <br>

| id | login_date |
|----|------------|
| 7  | 2020-05-30 |
| 1  | 2020-05-30 |
| 7  | 2020-05-31 |
| 7  | 2020-06-01 |
| 7  | 2020-06-02 |
| 7  | 2020-06-02 |
| 7  | 2020-06-03 |
| 1  | 2020-06-07 |
| 7  | 2020-06-10 |


Result table: <br>

| id | name     |
|----|----------|
| 7  | Jonathan |


User Winston with id = 1 logged in 2 times only in 2 different days, so, Winston is not an active user. <br>
User Jonathan with id = 7 logged in 7 times in 6 different days, five of them were consecutive days, so, Jonathan is an active user. <br>

Ans.

```
select distinct 
		d.id, 
		d.name 
	from 
		(
			select 
				b.id,
				c.name,
				b.login_date,
				b.found_recs_old_login_date,
				to_date(b.login_date,'YYYY-MM-DD') - to_date(b.found_recs_old_login_date,'YYYY-MM-DD') as date_diff
			from (
				 select 
					a.id,
					a.login_date,
					lag( a.login_date,4,'1990-01-01') over (partition by a.id order by  a.login_date) as found_recs_old_login_date 
				 from 
				 (select distinct id, to_char(login_date,'YYYY-MM-DD') as login_date from Logins ) a  
				) b
			left join 
				  Accounts c
			on b.id = c.id  
		) d 
where d.date_diff=4 
order by d.id;
```

							 
7.  Apples & Oranges <br>
https://leetcode.com/problems/apples-oranges/ <br>

Table: Sales <br>

| Column Name   | Type    |
|---------------|---------|
| sale_date     | date    |
| fruit         | enum    | 
| sold_num      | int     | 

(sale_date,fruit) is the primary key for this table. <br>
This table contains the sales of "apples" and "oranges" sold each day. <br>

Write an SQL query to report the difference between number of apples and oranges sold each day. <br>

Return the result table ordered by sale_date in format ('YYYY-MM-DD'). <br>

The query result format is in the following example: <br>

```
Sales table:
+------------+------------+-------------+
| sale_date  | fruit      | sold_num    |
+------------+------------+-------------+
| 2020-05-01 | apples     | 10          |
| 2020-05-01 | oranges    | 8           |
| 2020-05-02 | apples     | 15          |
| 2020-05-02 | oranges    | 15          |
| 2020-05-03 | apples     | 20          |
| 2020-05-03 | oranges    | 0           |
| 2020-05-04 | apples     | 15          |
| 2020-05-04 | oranges    | 16          |
+------------+------------+-------------+

Result table:
+------------+--------------+
| sale_date  | diff         |
+------------+--------------+
| 2020-05-01 | 2            |
| 2020-05-02 | 0            |
| 2020-05-03 | 20           |
| 2020-05-04 | -1           |
+------------+--------------+

Day 2020-05-01, 10 apples and 8 oranges were sold (Difference  10 - 8 = 2).
Day 2020-05-02, 15 apples and 15 oranges were sold (Difference 15 - 15 = 0).
Day 2020-05-03, 20 apples and 0 oranges were sold (Difference 20 - 0 = 20).
Day 2020-05-04, 15 apples and 16 oranges were sold (Difference 15 - 16 = -1).
```

Ans.

```
KL -
select
		coalesce(to_char(a.sale_date,'YYYY-MM-DD'),to_char(b.sale_date,'YYYY-MM-DD')) as sale_date,
		coalesce(a.sold_num,0) - coalesce(b.sold_num,0) as diff
from 
	(select sale_date , sold_num from sales where fruit='apples') a  
full join
	(select sale_date , sold_num from sales where fruit='oranges') b
on 
	to_char(a.sale_date,'YYYY-MM-DD')=to_char(b.sale_date,'YYYY-MM-DD')
order by 
	coalesce(to_char(a.sale_date,'YYYY-MM-DD'),to_char(b.sale_date,'YYYY-MM-DD'));

NG -
	select sale_date, 
		   sum(case when fruit = 'apples' then sold_num else 0-sold_num end) as "diff"
	from sales
	group by sale_date
```

8. Evaluate Boolean Expression <br>
https://leetcode.com/problems/evaluate-boolean-expression/ <br>

Table Variables: <br>

| Column Name   | Type    |
|---------------|---------|
| name          | varchar |
| value         | int     |

name is the primary key for this table. <br>
This table contains the stored variables and their values. <br>
 

Table Expressions: <br>

| Column Name   | Type    |
|---------------|---------|
| left_operand  | varchar |
| operator      | enum    |
| right_operand | varchar |

(left_operand, operator, right_operand) is the primary key for this table. <br>
This table contains a boolean expression that should be evaluated. <br>
operator is an enum that takes one of the values ('<', '>', '=') <br>
The values of left_operand and right_operand are guaranteed to be in the Variables table. <br>

Write an SQL query to evaluate the boolean expressions in Expressions table. <br>

Return the result table in any order. <br>

The query result format is in the following example. <br>

```
Variables table:
+------+-------+
| name | value |
+------+-------+
| x    | 66    |
| y    | 77    |
+------+-------+

Expressions table:
+--------------+----------+---------------+
| left_operand | operator | right_operand |
+--------------+----------+---------------+
| x            | >        | y             |
| x            | <        | y             |
| x            | =        | y             |
| y            | >        | x             |
| y            | <        | x             |
| x            | =        | x             |
+--------------+----------+---------------+

Result table:
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
| x            | >        | y             | false |
| x            | <        | y             | true  |
| x            | =        | y             | false |
| y            | >        | x             | true  |
| y            | <        | x             | false |
| x            | =        | x             | true  |
+--------------+----------+---------------+-------+
As shown, you need find the value of each boolean exprssion in the table using the variables table.
```

Ans.
```
select 
		left_operand as "left_operand",
		operator as "operator",
		right_operand as "right_operand",
		case 
			when operator='>' then case when left_value>right_value then 'true' else 'false' end 
			when operator='<' then case when left_value<right_value then 'true' else 'false' end 
			when operator='=' then case when left_value=right_value then 'true' else 'false' end 
		end as "value" 
		from 
			(
				select
					a.left_operand  ,
					b.value as left_value,
					a.operator ,
					a.right_operand ,
					c.value as right_value
				from 
					expressions a
				left join 
					variables b
				on a.left_operand = b.name
				left join 
					variables c
				on a.right_operand = c.name
			);
```

9. Customers Who Bought Products A and B but Not C  <br>
https://leetcode.com/problems/customers-who-bought-products-a-and-b-but-not-c/submissions/ <br>		

Table: Customers <br>

| Column Name         | Type    |
|---------------------|---------|
| customer_id         | int     |
| customer_name       | varchar |

customer_id is the primary key for this table. <br>
customer_name is the name of the customer. <br>

Table: Orders

| Column Name   | Type    |
|---------------|---------|
| order_id      | int     |
| customer_id   | int     |
| product_name  | varchar |

order_id is the primary key for this table. <br>
customer_id is the id of the customer who bought the product "product_name". <br>

Write an SQL query to report the customer_id and customer_name of customers who bought products "A", "B" but did not buy the product "C" since we want to recommend them buy this product. <br>

Return the result table ordered by customer_id. <br>

The query result format is in the following example. <br>

```
Customers table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Daniel        |
| 2           | Diana         |
| 3           | Elizabeth     |
| 4           | Jhon          |
+-------------+---------------+

Orders table:
+------------+--------------+---------------+
| order_id   | customer_id  | product_name  |
+------------+--------------+---------------+
| 10         |     1        |     A         |
| 20         |     1        |     B         |
| 30         |     1        |     D         |
| 40         |     1        |     C         |
| 50         |     2        |     A         |
| 60         |     3        |     A         |
| 70         |     3        |     B         |
| 80         |     3        |     D         |
| 90         |     4        |     C         |
+------------+--------------+---------------+

Result table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 3           | Elizabeth     |
+-------------+---------------+
Only the customer_id with id 3 bought the product A and B but not the product C.
```
Ans. <br>

```
select 
	customer_id,
	customer_name
from (
		select 
			distinct  
					a.customer_id,  
					b.customer_name, 
					a.product_name 
		   from 
			orders a 
				left join      
			customers b      
			on a.customer_id = b.customer_id     
		where a.product_name in ('A','B','C') 
	)
    group by customer_id,customer_name
    having sum(case when product_name='A' then 1 when product_name='B' then 2 else 100 end)=3
    order by customer_id;
```
10.  Capital Gain/Loss <br>
https://leetcode.com/problems/capital-gainloss/ <br>

Table: Stocks <br>

| Column Name   | Type    |
|---------------|---------|
| stock_name    | varchar |
| operation     | enum    |
| operation_day | int     |
| price         | int     |

(stock_name, day) is the primary key for this table. <br>
The operation column is an ENUM of type ('Sell', 'Buy') <br>
Each row of this table indicates that the stock which has stock_name had an operation on the day operation_day with the price. <br>
It is guaranteed that each 'Sell' operation for a stock has a corresponding 'Buy' operation in a previous day. <br>
 

Write an SQL query to report the Capital gain/loss for each stock. <br>

The capital gain/loss of a stock is total gain or loss after buying and selling the stock one or many times. <br>

Return the result table in any order. <br>

The query result format is in the following example: <br>
```
Stocks table:
+---------------+-----------+---------------+--------+
| stock_name    | operation | operation_day | price  |
+---------------+-----------+---------------+--------+
| Leetcode      | Buy       | 1             | 1000   |
| Corona Masks  | Buy       | 2             | 10     |
| Leetcode      | Sell      | 5             | 9000   |
| Handbags      | Buy       | 17            | 30000  |
| Corona Masks  | Sell      | 3             | 1010   |
| Corona Masks  | Buy       | 4             | 1000   |
| Corona Masks  | Sell      | 5             | 500    |
| Corona Masks  | Buy       | 6             | 1000   |
| Handbags      | Sell      | 29            | 7000   |
| Corona Masks  | Sell      | 10            | 10000  |
+---------------+-----------+---------------+--------+

Result table:
+---------------+-------------------+
| stock_name    | capital_gain_loss |
+---------------+-------------------+
| Corona Masks  | 9500              |
| Leetcode      | 8000              |
| Handbags      | -23000            |
+---------------+-------------------+
```
Leetcode stock was bought at day 1 for 1000$ and was sold at day 5 for 9000$. Capital gain = 9000 - 1000 = 8000$. <br>
Handbags stock was bought at day 17 for 30000$ and was sold at day 29 for 7000$. Capital loss = 7000 - 30000 = -23000$. <br>
Corona Masks stock was bought at day 1 for 10$ and was sold at day 3 for 1010$. It was bought again at day 4 for 1000$ and was sold at day 5 for 500$.  <br>
At last, it was bought at day 6 for 1000$ and was sold at day 10 for 10000$.  <br>
Capital gain/loss is the sum of capital gains/losses for each ('Buy' --> 'Sell') operation  <br>
= (1010 - 10) + (500 - 1000) + (10000 - 1000) = 1000 - 500 + 9000 = 9500$.

Ans.

```
select
	stock_name,
	sum(case when operation ='Sell' then price else 0-price end) as capital_gain_loss 
from 
	stocks
group by stock_name
order by stock_name;
```
 
12. Number of Trusted Contacts of a Customer <br>
https://leetcode.com/problems/number-of-trusted-contacts-of-a-customer/ <br>

Table: Customers <br>

| Column Name   | Type    |
|--------------|---------|
| customer_id   | int     |
| customer_name | varchar |
| email         | varchar |

customer_id is the primary key for this table. <br>
Each row of this table contains the name and the email of a customer of an online shop. <br>

Table: Contacts <br>

| Column Name   | Type    |
|--------------|---------|
| user_id       | id      |
| contact_name  | varchar |
| contact_email | varchar |

(user_id, contact_email) is the primary key for this table. <br>
Each row of this table contains the name and email of one contact of customer with user_id. <br>
This table contains information about people each customer trust. The contact may or may not exist in the Customers table. <br>

Table: Invoices <br>

| Column Name  | Type    |
|--------------|---------|
| invoice_id   | int     |
| price        | int     |
| user_id      | int     |


invoice_id is the primary key for this table. <br>
Each row of this table indicates that user_id has an invoice with invoice_id and a price. <br>

Write an SQL query to find the following for each invoice_id: <br>

customer_name: The name of the customer the invoice is related to. <br>
price: The price of the invoice. <br>
contacts_cnt: The number of contacts related to the customer. <br>
trusted_contacts_cnt: The number of contacts related to the customer and at the same time they are customers to the shop. <br> 
(i.e His/Her email exists in the Customers table.) <br>
Order the result table by invoice_id. <br>

The query result format is in the following example: <br>

Customers table: <br>

| customer_id | customer_name | email              |
|-------------|---------------|--------------------|
| 1           | Alice         | alice@leetcode.com |
| 2           | Bob           | bob@leetcode.com   |
| 13          | John          | john@leetcode.com  |
| 6           | Alex          | alex@leetcode.com  |


Contacts table:

| user_id     | contact_name | contact_email      |
|-------------|--------------|--------------------|
| 1           | Bob          | bob@leetcode.com   |
| 1           | John         | john@leetcode.com  |
| 1           | Jal          | jal@leetcode.com   |
| 2           | Omar         | omar@leetcode.com  |
| 2           | Meir         | meir@leetcode.com  |
| 6           | Alice        | alice@leetcode.com |



Invoices table:  <br>

| invoice_id | price | user_id |
|------------|-------|---------|
| 77         | 100   | 1       |
| 88         | 200   | 1       |
| 99         | 300   | 2       |
| 66         | 400   | 2       |
| 55         | 500   | 13      |
| 44         | 60    | 6       |


Result table:

| invoice_id | customer_name | price | contacts_cnt | trusted_contacts_cnt |
|------------|---------------|-------|--------------|----------------------|
| 44         | Alex          | 60    | 1            | 1                    |
| 55         | John          | 500   | 0            | 0                    |
| 66         | Bob           | 400   | 2            | 0                    |
| 77         | Alice         | 100   | 3            | 2                    |
| 88         | Alice         | 200   | 3            | 2                    |
| 99         | Bob           | 300   | 2            | 0                    |


Alice has three contacts, two of them are trusted contacts (Bob and John). <br>
Bob has two contacts, none of them is a trusted contact. <br>
Alex has one contact and it is a trusted contact (Alice). <br>
John doesn't have any contacts. <br>

Ans.
```
select
	a.invoice_id ,
	b.customer_name,
	a.price,
	(select count(*) from contacts where user_id =  a.user_id) as contacts_cnt ,
	(select count(*) from contacts where user_id = a.user_id and contact_email in (select email from customers) ) as trusted_contacts_cnt 
from
	invoices a
left join 
	customers b
on 
	a.user_id = b.customer_id
order by invoice_id;

-- Other Approach: Using Joins
select
		a.invoice_id ,
		b.customer_name,
		a.price,
		count(e.contact_email) as contacts_cnt  ,
		sum(case when e.email is not NULL then 1 else 0 end) as trusted_contacts_cnt 
from
		invoices a
left join 
		customers b
on 
		a.user_id = b.customer_id
left join 
(
    select 
        c.user_id,
        c.contact_email,
        d.email
        from contacts c left join 
            customers d 
        on c.contact_email = d.email
) e
on a.user_id = e.user_id
group by a.invoice_id , b.customer_name, a.price
order by a.invoice_id;
```



12. Activity Participants <br>
https://leetcode.com/problems/activity-participants/ <br> 

Table: Friends <br>

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |
| activity      | varchar |


id is the id of the friend and primary key for this table.<br>
name is the name of the friend.<br>
activity is the name of the activity which the friend takes part in.<br>

Table: Activities <br>

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |

id is the primary key for this table.<br>
name is the name of the activity.<br>
 
Write an SQL query to find the names of all the activities with neither maximum, nor minimum number of participants.<br>

Return the result table in any order. Each activity in table Activities is performed by any person in the table Friends.<br>

The query result format is in the following example:<br>

```
Friends table:
+------+--------------+---------------+
| id   | name         | activity      |
+------+--------------+---------------+
| 1    | Jonathan D.  | Eating        |
| 2    | Jade W.      | Singing       |
| 3    | Victor J.    | Singing       |
| 4    | Elvis Q.     | Eating        |
| 5    | Daniel A.    | Eating        |
| 6    | Bob B.       | Horse Riding  |
+------+--------------+---------------+

Activities table:
+------+--------------+
| id   | name         |
+------+--------------+
| 1    | Eating       |
| 2    | Singing      |
| 3    | Horse Riding |
+------+--------------+

Result table:
+--------------+
| activity     |
+--------------+
| Singing      |
+--------------+

Eating activity is performed by 3 friends, maximum number of participants, (Jonathan D. , Elvis Q. and Daniel A.)
Horse Riding activity is performed by 1 friend, minimum number of participants, (Bob B.)
Singing is performed by 2 friends (Victor J. and Jade W.)
```

Ans.

```
select activity from (
				select
					activity,
					count(*) as act_count,
					min(count(*)) over () as min_cnt,
					max(count(*)) over () as max_cnt
				from friends
				group by activity
				order by activity
					) 
where min_cnt<act_count and act_count<max_cnt;
```


13. Second Degree Follower <br>
https://leetcode.com/problems/second-degree-follower/submissions/ <br>

In facebook, there is a follow table with two columns: followee, follower. <br>

Please write a sql query to get the amount of each follower’s follower if he/she has one. <br>

For example: <br>

| followee    | follower   |
|-------------|------------|
|     A       |     B      |
|     B       |     C      |
|     B       |     D      |
|     D       |     E      |


should output:

| follower    | num        |
|-------------|------------|
|     B       |  2         |
|     D       |  1         |


Explaination: <br>
Both B and D exist in the follower list, when as a followee, B's follower is C and D, and D's follower is E. <br>
A does not exist in follower list. <br>
 
Note: <br>
Followee would not follow himself/herself in all cases. <br>

Ans. <br>
there could be duplicates in table, so use count distinct for counting followers. <br>

```
select 
	main as follower, 
	count(distinct follower) as num 
from (
	select 
		a.follower as main,
		b.follower    as follower
	from follow a inner join follow b
	on a.follower = b.followee
	)
group by main 
order by main;
```

