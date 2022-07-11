####  Second Highest Salary
Link : https://leetcode.com/problems/second-highest-salary/ <br>

Table: Employee<br>

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key column for this table.
Each row of this table contains information about the salary of an employee.
``` 

Write an SQL query to report the second highest salary from the Employee table. If there is no second highest salary, the query should report null.<br>
The query result format is in the following example.<br>

```
Example 1:

Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
Example 2:

Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
```

Ans <br>
```
select  max(salary) as SecondHighestSalary from
(select 
    salary, 
    dense_rank() over (order by salary desc) as sal_rank 
 from employee
 ) 
where sal_rank=2;
```

Note -> if Nth highest salary --> <br>
Link : https://leetcode.com/problems/nth-highest-salary/ <br>
```
CREATE FUNCTION getNthHighestSalary(N IN NUMBER) RETURN NUMBER IS
result NUMBER;
BEGIN
    /* Write your PL/SQL query statement below */
    
    select max(salary) into result
    from 
        (
        select salary, 
               dense_rank() over (order by salary desc) as sal_rank
            from employee
        ) where sal_rank=N;
    
    RETURN result;
END;

```

####  Find Median Given Frequency of Numbers
Link : https://leetcode.com/problems/find-median-given-frequency-of-numbers/ <br>
Table: Numbers <br>
```

+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
| frequency   | int  |
+-------------+------+
num is the primary key for this table.
Each row of this table shows the frequency of a number in the database.

``` 

The median is the value separating the higher half from the lower half of a data sample. <br>
Write an SQL query to report the median of all the numbers in the database after decompressing the Numbers table. Round the median to one decimal point. <br>
The query result format is in the following example. <br>

```
Example 1:

Input: 
Numbers table:
+-----+-----------+
| num | frequency |
+-----+-----------+
| 0   | 7         |
| 1   | 1         |
| 2   | 3         |
| 3   | 1         |
+-----+-----------+
Output: 
+--------+
| median |
+--------+
| 0.0    |
+--------+
Explanation: 
If we decompress the Numbers table, we will get [0, 0, 0, 0, 0, 0, 0, 1, 2, 2, 2, 3], so the median is (0 + 0) / 2 = 0.
```
Ans <br>

```
select round(avg(num),1) as median
from
        (
        select
            num,
            frequency,
            lag(cum_sum,1,0) over (order by num) as prev_sum,
            cum_sum,
            medium_num
        from
                (select 
                    num,
                    frequency,
                    sum(frequency) over (order by num) as cum_sum,
                    sum(frequency) over () / 2 as medium_num
                from numbers)   a 
            ) b
where medium_num between prev_sum and  cum_sum;
```

#### Find Cumulative Salary of an Employee
Table: Employee <br>

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| month       | int  |
| salary      | int  |
+-------------+------+
(id, month) is the primary key for this table.
Each row in the table indicates the salary of an employee in one month during the year 2020.

```

Write an SQL query to calculate the cumulative salary summary for every employee in a single unified table. <br>

The cumulative salary summary for an employee can be calculated as follows: <br>
- For each month that the employee worked, sum up the salaries in that month and the previous two months. This is their 3-month sum for that month. If an employee did not work for the company in previous months, their effective salary for those months is 0. <br>
- Do not include the 3-month sum for the most recent month that the employee worked for in the summary. <br>
- Do not include the 3-month sum for any month the employee did not work. <br>
- Return the result table ordered by id in ascending order. In case of a tie, order it by month in descending order. <br>

The query result format is in the following example. <br>

 

Example 1: <br>
```
Input: 
Employee table:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 1  | 1     | 20     |
| 2  | 1     | 20     |
| 1  | 2     | 30     |
| 2  | 2     | 30     |
| 3  | 2     | 40     |
| 1  | 3     | 40     |
| 3  | 3     | 60     |
| 1  | 4     | 60     |
| 3  | 4     | 70     |
| 1  | 7     | 90     |
| 1  | 8     | 90     |
+----+-------+--------+
Output: 
+----+-------+--------+
| id | month | Salary |
+----+-------+--------+
| 1  | 7     | 90     |
| 1  | 4     | 130    |
| 1  | 3     | 90     |
| 1  | 2     | 50     |
| 1  | 1     | 20     |
| 2  | 1     | 20     |
| 3  | 3     | 100    |
| 3  | 2     | 40     |
+----+-------+--------+
Explanation: 
Employee '1' has five salary records excluding their most recent month '8':
- 90 for month '7'.
- 60 for month '4'.
- 40 for month '3'.
- 30 for month '2'.
- 20 for month '1'.
So the cumulative salary summary for this employee is:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 1  | 7     | 90     |  (90 + 0 + 0)
| 1  | 4     | 130    |  (60 + 40 + 30)
| 1  | 3     | 90     |  (40 + 30 + 20)
| 1  | 2     | 50     |  (30 + 20 + 0)
| 1  | 1     | 20     |  (20 + 0 + 0)
+----+-------+--------+
Note that the 3-month sum for month '7' is 90 because they did not work during month '6' or month '5'.

Employee '2' only has one salary record (month '1') excluding their most recent month '2'.
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 2  | 1     | 20     |  (20 + 0 + 0)
+----+-------+--------+

Employee '3' has two salary records excluding their most recent month '4':
- 60 for month '3'.
- 40 for month '2'.
So the cumulative salary summary for this employee is:
+----+-------+--------+
| id | month | salary |
+----+-------+--------+
| 3  | 3     | 100    |  (60 + 40 + 0)
| 3  | 2     | 40     |  (40 + 0 + 0)
+----+-------+--------+
```
Ans <br>
```
select
    id, 
    month,
    sum(salary) over (partition by id order by month range between 2 PRECEDING and current row)  as salary
from    
    (
    select 
        id,
        month,
        salary,
        max(month) over (partition by id order by month rows between unbounded preceding and unbounded following) as max_month_for_employee     
    from employee
     )
 where month!=max_month_for_employee
 order by id, month desc;  
```

#### Average Salary: Departments VS Company
Link : https://leetcode.com/problems/average-salary-departments-vs-company/ <br>

```
Table: Salary

+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| employee_id | int  |
| amount      | int  |
| pay_date    | date |
+-------------+------+
id is the primary key column for this table.
Each row of this table indicates the salary of an employee in one month.
employee_id is a foreign key from the Employee table.
 

Table: Employee

+---------------+------+
| Column Name   | Type |
+---------------+------+
| employee_id   | int  |
| department_id | int  |
+---------------+------+
employee_id is the primary key column for this table.
Each row of this table indicates the department of an employee.
```

Write an SQL query to report the comparison result (higher/lower/same) of the average salary of employees in a department to the company's average salary. <br>
Return the result table in any order. <br>
The query result format is in the following example. <br>
Example 1: <br>

```
Input: 
Salary table:
+----+-------------+--------+------------+
| id | employee_id | amount | pay_date   |
+----+-------------+--------+------------+
| 1  | 1           | 9000   | 2017/03/31 |
| 2  | 2           | 6000   | 2017/03/31 |
| 3  | 3           | 10000  | 2017/03/31 |
| 4  | 1           | 7000   | 2017/02/28 |
| 5  | 2           | 6000   | 2017/02/28 |
| 6  | 3           | 8000   | 2017/02/28 |
+----+-------------+--------+------------+
Employee table:
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 2             |
| 3           | 2             |
+-------------+---------------+
Output: 
+-----------+---------------+------------+
| pay_month | department_id | comparison |
+-----------+---------------+------------+
| 2017-02   | 1             | same       |
| 2017-03   | 1             | higher     |
| 2017-02   | 2             | same       |
| 2017-03   | 2             | lower      |
+-----------+---------------+------------+
Explanation: 
In March, the company's average salary is (9000+6000+10000)/3 = 8333.33...
The average salary for department '1' is 9000, which is the salary of employee_id '1' since there is only one employee in this department. So the comparison result is 'higher' since 9000 > 8333.33 obviously.
The average salary of department '2' is (6000 + 10000)/2 = 8000, which is the average of employee_id '2' and '3'. So the comparison result is 'lower' since 8000 < 8333.33.

With he same formula for the average salary comparison in February, the result is 'same' since both the department '1' and '2' have the same average salary with the company, which is 7000.
```
Ans  <br>
```
select distinct
    pay_month,
    department_id,
    case 
        when avg_dept<avg_company then 'lower'
        when avg_dept=avg_company then 'same'
        when avg_dept>avg_company then 'higher'
    end as comparison
from    
    (
            select 
                department_id,
                date_format(pay_date, '%Y-%m') as pay_month,
                 avg(employee_salary) over (partition by pay_date  ) as avg_company,
                 avg(employee_salary) over (partition by pay_date, department_id  ) as avg_dept
            from    
                (select
                    s.employee_id,
                    s.amount as employee_salary,
                    s.pay_date,
                    e.department_id   
                from salary s 
                inner join employee e
                on s.employee_id = e.employee_id) a
    ) b     
```

#### Sales by Day of the Week
https://leetcode.com/problems/sales-by-day-of-the-week/ <br>
```
Table: Orders

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| order_date    | date    | 
| item_id       | varchar |
| quantity      | int     |
+---------------+---------+
(ordered_id, item_id) is the primary key for this table.
This table contains information on the orders placed.
order_date is the date item_id was ordered by the customer with id customer_id.
 

Table: Items

+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| item_id             | varchar |
| item_name           | varchar |
| item_category       | varchar |
+---------------------+---------+
item_id is the primary key for this table.
item_name is the name of the item.
item_category is the category of the item.
```

You are the business owner and would like to obtain a sales report for category items and the day of the week. <br>
Write an SQL query to report how many units in each category have been ordered on each day of the week. <br>
Return the result table ordered by category. <br>
The query result format is in the following example. <br>

Example 1: <br>

```
Input: 
Orders table:
+------------+--------------+-------------+--------------+-------------+
| order_id   | customer_id  | order_date  | item_id      | quantity    |
+------------+--------------+-------------+--------------+-------------+
| 1          | 1            | 2020-06-01  | 1            | 10          |
| 2          | 1            | 2020-06-08  | 2            | 10          |
| 3          | 2            | 2020-06-02  | 1            | 5           |
| 4          | 3            | 2020-06-03  | 3            | 5           |
| 5          | 4            | 2020-06-04  | 4            | 1           |
| 6          | 4            | 2020-06-05  | 5            | 5           |
| 7          | 5            | 2020-06-05  | 1            | 10          |
| 8          | 5            | 2020-06-14  | 4            | 5           |
| 9          | 5            | 2020-06-21  | 3            | 5           |
+------------+--------------+-------------+--------------+-------------+
Items table:
+------------+----------------+---------------+
| item_id    | item_name      | item_category |
+------------+----------------+---------------+
| 1          | LC Alg. Book   | Book          |
| 2          | LC DB. Book    | Book          |
| 3          | LC SmarthPhone | Phone         |
| 4          | LC Phone 2020  | Phone         |
| 5          | LC SmartGlass  | Glasses       |
| 6          | LC T-Shirt XL  | T-Shirt       |
+------------+----------------+---------------+
Output: 
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Category   | Monday    | Tuesday   | Wednesday | Thursday  | Friday    | Saturday  | Sunday    |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
| Book       | 20        | 5         | 0         | 0         | 10        | 0         | 0         |
| Glasses    | 0         | 0         | 0         | 0         | 5         | 0         | 0         |
| Phone      | 0         | 0         | 5         | 1         | 0         | 0         | 10        |
| T-Shirt    | 0         | 0         | 0         | 0         | 0         | 0         | 0         |
+------------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
Explanation: 
On Monday (2020-06-01, 2020-06-08) were sold a total of 20 units (10 + 10) in the category Book (ids: 1, 2).
On Tuesday (2020-06-02) were sold a total of 5 units in the category Book (ids: 1, 2).
On Wednesday (2020-06-03) were sold a total of 5 units in the category Phone (ids: 3, 4).
On Thursday (2020-06-04) were sold a total of 1 unit in the category Phone (ids: 3, 4).
On Friday (2020-06-05) were sold 10 units in the category Book (ids: 1, 2) and 5 units in Glasses (ids: 5).
On Saturday there are no items sold.
On Sunday (2020-06-14, 2020-06-21) were sold a total of 10 units (5 +5) in the category Phone (ids: 3, 4).
There are no sales of T-shirts.
```
Ans  <br>
```
with cte_table as 
(select
    b.item_category,
    dayname(a.order_date) as day_of_week,
    COALESCE(sum(a.quantity),0) as qty
from 
    orders a 
    RIGHT join
    items b
    on a.item_id = b.item_id
group by b.item_category, dayname(a.order_date)
)
select
    item_category as "CATEGORY",
    sum(case when upper(day_of_week)='MONDAY' then qty else 0 end) as 'MONDAY',
    sum(case when  upper(day_of_week)='TUESDAY' then qty else 0 end) as 'TUESDAY',
    sum(case when  upper(day_of_week)='WEDNESDAY' then qty else 0 end) as 'WEDNESDAY',
    sum(case when  upper(day_of_week)='THURSDAY' then qty else 0 end) as 'THURSDAY',
    sum(case when  upper(day_of_week)='FRIDAY' then qty else 0 end) as 'FRIDAY',
    sum(case when  upper(day_of_week)='SATURDAY' then qty else 0 end) as 'SATURDAY',
    sum(case when  upper(day_of_week)='SUNDAY' then qty else 0 end) as 'SUNDAY'
    
from cte_table
group by item_category
order by item_category;

```

#### Report Contiguous Dates
https://leetcode.com/problems/report-contiguous-dates/ <br>

```
Table: Failed

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| fail_date    | date    |
+--------------+---------+
fail_date is the primary key for this table.
This table contains the days of failed tasks.
 

Table: Succeeded

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| success_date | date    |
+--------------+---------+
success_date is the primary key for this table.
This table contains the days of succeeded tasks.
``` 

A system is running one task every day. Every task is independent of the previous tasks. The tasks can fail or succeed. <br>
Write an SQL query to generate a report of period_state for each continuous interval of days in the period from 2019-01-01 to 2019-12-31. <br>
period_state is 'failed' if tasks in this interval failed or 'succeeded' if tasks in this interval succeeded. Interval of days are retrieved as start_date and end_date. <br>
Return the result table ordered by start_date. <br>
The query result format is in the following example. <br>

Example 1: <br>

```
Input: 
Failed table:
+-------------------+
| fail_date         |
+-------------------+
| 2018-12-28        |
| 2018-12-29        |
| 2019-01-04        |
| 2019-01-05        |
+-------------------+
Succeeded table:
+-------------------+
| success_date      |
+-------------------+
| 2018-12-30        |
| 2018-12-31        |
| 2019-01-01        |
| 2019-01-02        |
| 2019-01-03        |
| 2019-01-06        |
+-------------------+
Output: 
+--------------+--------------+--------------+
| period_state | start_date   | end_date     |
+--------------+--------------+--------------+
| succeeded    | 2019-01-01   | 2019-01-03   |
| failed       | 2019-01-04   | 2019-01-05   |
| succeeded    | 2019-01-06   | 2019-01-06   |
+--------------+--------------+--------------+
Explanation: 
The report ignored the system state in 2018 as we care about the system in the period 2019-01-01 to 2019-12-31.
From 2019-01-01 to 2019-01-03 all tasks succeeded and the system state was "succeeded".
From 2019-01-04 to 2019-01-05 all tasks failed and the system state was "failed".
From 2019-01-06 to 2019-01-06 all tasks succeeded and the system state was "succeeded".
```

Ans  <br>
```
with 
cte1 as 
        (
            select
                'failed' as status,
                date_format(fail_date,'%Y-%m-%d') as mydate
            from 
            Failed
            where fail_date between '2019-01-01' and '2019-12-31'
            union 
            select
                'succeeded' as status,
                date_format(success_date,'%Y-%m-%d') as mydate
            from 
            Succeeded
            where success_date between '2019-01-01' and '2019-12-31'
        ), 
cte2 as 
    (select 
            mydate,
            status,
            row_number() over (order by mydate)  -   dense_rank() over (partition by status order by mydate) as grp
    from cte1
    )
select
    status as period_state, 
    min(mydate) as start_date,
    max(mydate) as end_date
from 
cte2
group by grp , status 
order by  min(mydate); 
```

