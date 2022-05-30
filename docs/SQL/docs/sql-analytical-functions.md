# SQL Analytical Functions

[Source](http://etutorials.org/SQL/Mastering+Oracle+SQL/Chapter+14.+Advanced+Analytic+SQL/14.1+Analytic+SQL+Overview/)

### over (partition by / order by )
eg -
```
SELECT o.region_id region_id, o.cust_nbr cust_nbr,

 SUM(o.tot_sales) tot_sales,

 SUM(SUM(o.tot_sales)) OVER (PARTITION BY o.region_id) region_sales

FROM orders o

WHERE o.year = 2001

GROUP BY o.region_id, o.cust_nbr;
```


### Ranking Functions
[source](http://etutorials.org/SQL/Mastering+Oracle+SQL/Chapter+14.+Advanced+Analytic+SQL/14.2+Ranking+Functions/)

#### ROW_NUMBER, RANK and DENSE_RANK 
- ROW_NUMBER - Returns a unique number for each row starting with 1. <br>For rows that have duplicate values, numbers are arbitrarily assigned.<br>
- DENSE_RANK - Assigns a unique number for each row starting with 1, except for rows that have duplicate values, in which case the same ranking is assigned.<br>
- RANK - Assigns a unique number for each row starting with 1, except for rows that have duplicate values, in which case the same ranking is assigned and a gap appears in the sequence for each duplicate ranking.<br>
eg 1 -
```

SELECT region_id, cust_nbr, 

  SUM(tot_sales) cust_sales,

  RANK( ) OVER (ORDER BY SUM(tot_sales) DESC) sales_rank,

  DENSE_RANK( ) OVER (ORDER BY SUM(tot_sales) DESC) sales_dense_rank,

  ROW_NUMBER( ) OVER (ORDER BY SUM(tot_sales) DESC) sales_number

FROM orders

WHERE year = 2001

GROUP BY region_id, cust_nbr

ORDER BY sales_number;


REGION_ID   CUST_NBR  CUST_SALES SALES_RANK SALES_DENSE_RANK SALES_NUMBER

---------- ---------- ---------- ---------- ---------------- ------------

         8         18    1253840         11               11           11

         5          2    1224992         12               12           12

         9         23    1224992         12               12           13

         9         24    1224992         12               12           14

        10         30    1216858         15               13           15

```


eg 2 - The following query generates rankings for customer sales within each region rather than across all regions.<br> 
Note the addition of the PARTITION BY clause:<br>

```
SELECT 
  region_id, cust_nbr, 
  
  SUM(tot_sales) cust_sales,

  RANK( ) OVER (PARTITION BY region_id  ORDER BY SUM(tot_sales) DESC) sales_rank,

  DENSE_RANK( ) OVER (PARTITION BY region_id ORDER BY SUM(tot_sales) DESC) sales_dense_rank,

  ROW_NUMBER( ) OVER (PARTITION BY region_id ORDER BY SUM(tot_sales) DESC) sales_number

FROM orders

WHERE year = 2001

GROUP BY region_id, cust_nbr

ORDER BY region_id, sales_number;

REGION_ID    CUST_NBR CUST_SALES SALES_RANK SALES_DENSE_RANK SALES_NUMBER

---------- ---------- ---------- ---------- ---------------- ------------

         5          4    1878275          1                1            1

         5          2    1224992          2                2            2

         5          5    1169926          3                3            3

         5          3    1161286          4                4            4

         5          1    1151162          5                5            5

         6          6    1788836          1                1            1

         6          9    1208959          2                2            2
		 
``` 


#### Handling NULLs<br>
All ranking functions allow you to specify where in the ranking order NULL values should appear. <br>
This is accomplished by appending either NULLS FIRST or NULLS LAST after the ORDER BY clause of the function, as in:<br>

```
SELECT region_id, cust_nbr, SUM(tot_sales) cust_sales,

  RANK( ) OVER (ORDER BY SUM(tot_sales) DESC NULLS LAST) sales_rank

FROM orders

WHERE year = 2001

GROUP BY region_id, cust_nbr;
```


### NTILE<br>
Another way rankings are commonly used is to generate buckets into which sets of rankings are grouped.<br> 
For example, you may want to find those customers whose total sales ranked in the top 25%. <br>
The following query uses the NTILE function to group the customers into four buckets (or quartiles):<br>

```
SELECT region_id, cust_nbr, SUM(tot_sales) cust_sales,

  NTILE(4) OVER (ORDER BY SUM(tot_sales) DESC) sales_quartile

FROM orders

WHERE year = 2001

GROUP BY region_id, cust_nbr

ORDER BY sales_quartile, cust_sales DESC;

REGION_ID    CUST_NBR CUST_SALES SALES_QUARTILE

---------- ---------- ---------- --------------

         9         25    2232703              1

         8         17    1944281              1

         7         14    1929774              1
```


#### CUME_DIST and PERCENT_RANK
CUME_DIST function (short for Cumulative Distribution) calculates the ratio of the number of rows that have a lesser or equal ranking to the total number of rows in the partition. <br> 

PERCENT_RANK function calculates the ratio of a row's ranking to the number of rows in the partition using the formula:<br>
```
(RRP -- 1) / (NRP -- 1)
```
where RRP is the "rank of row in partition," and NRP is the "number of rows in partition."<br>



### Windowing Functions
#### ROWS BETWEEN <> AND <>
Some of the sample values can be -
##### ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
eg -
```
SELECT month, 
  SUM(tot_sales) monthly_sales,
  SUM(SUM(tot_sales)) OVER ( ORDER BY month 
							 ROWS BETWEEN UNBOUNDED PRECEDING AND 
										  UNBOUNDED FOLLOWING     ) total_sales
FROM orders
WHERE year = 2001 
  AND region_id = 6
GROUP BY month
ORDER BY month;

     MONTH MONTHLY_SALES TOTAL_SALES
         1        610697     6307766
         2        428676     6307766
         3        637031     6307766
         4        541146     6307766
```

##### ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
eg 1 - max current value-
```
SELECT month, 
  SUM(tot_sales) monthly_sales,
  MAX(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) max_preceeding
FROM orders
WHERE year = 2001 
  AND region_id = 6
GROUP BY month
ORDER BY month;

     MONTH MONTHLY_SALES MAX_PRECEEDING
         1        610697         610697
         2        428676         610697
         3        637031         637031
         4        541146         637031
         5        592935         637031
         6        501485         637031
```
eg 2 - cumulative SUM -
```
SELECT month, 
  SUM(tot_sales) monthly_sales,
  SUM(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) max_preceeding
FROM orders
WHERE year = 2001 
  AND region_id = 6
GROUP BY month
ORDER BY month;

     MONTH MONTHLY_SALES RUNNING_TOTAL
         1        610697         610697
         2        428676        1039373
         3        637031        1676404
         4        541146        2217550
         5        592935        2810485
```

##### ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING - 
eg -calculate avg of rolling 3 values (current row, rev row and next row);
```
SELECT month, 
  SUM(tot_sales) monthly_sales,
  AVG(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) rolling_avg
FROM orders
WHERE year = 2001 
  AND region_id = 6
GROUP BY month
ORDER BY month;

     MONTH MONTHLY_SALES ROLLING_AVG
         1        610697    519686.5
         2        428676  558801.333
         3        637031  535617.667
         4        541146  590370.667
         5        592935  545188.667
```

#### RANGE BETWEEN
- ROWS BETWEEN - based on row ordered as per condition of ordering insdie over() clause.
- RANGE BETWEEN - based on value ranges specified under over() clause.

eg1 - to generate a three-month rolling average (similar to above ROWS BETWEEN question).
In our table, month is numeric integer value, and so RANGE works perfectly fine here.
This substitution works because the month column contains integer values, so adding and subtracting 1 from the current month yields a three-month range.

But if its character, then below wont be suited.

```
SELECT 
  month,
  SUM(tot_sales) monthly_sales,
  AVG(SUM(tot_sales)) OVER (ORDER BY month RANGE BETWEEN 1 PRECEDING AND 1 FOLLOWING) rolling_avg
FROM orders
WHERE year = 2001  AND region_id = 6
GROUP BY month
ORDER BY month;

---------- ------------- -----------
MONTH      MONTHLY_SALES ROLLING_AVG
---------- ------------- -----------
         1        610697    519686.5
         2        428676  558801.333
         3        637031  535617.667
```

eg2 - if we do a range of +/- 1.999, then also we get same values:
```
SELECT month,
  SUM(tot_sales) monthly_sales,
  AVG(SUM(tot_sales)) OVER (ORDER BY month RANGE BETWEEN 1.99 PRECEDING AND 1.99 FOLLOWING) rolling_avg
FROM orders
WHERE year = 2001  AND region_id = 6
GROUP BY month
ORDER BY month;

---------- ------------- -----------
    MONTH  MONTHLY_SALES ROLLING_AVG
---------- ------------- -----------
         1        610697    519686.5
         2        428676  558801.333
         3        637031  535617.667
```
eg3 - working with **DATE Range**. <br>
ROWS wont be of much use if we are working on Date Range.<br>
If you are generating a window based on a DATE column, you can specify a range in increments of days, months, or years. <br>
Since the orders table has no DATE columns, the next example shows how a date range can be specified against the order_dt column of the cust_order table:<br>

```
SELECT 
  TRUNC(order_dt) day,
  SUM(sale_price) daily_sales,
  AVG(SUM(sale_price)) OVER ( 
                              ORDER BY TRUNC(order_dt) 
								              RANGE BETWEEN INTERVAL '2' DAY PRECEDING AND INTERVAL '2' DAY FOLLOWING     
                            ) five_day_avg
FROM cust_order
WHERE sale_price IS NOT NULL 
  AND order_dt BETWEEN TO_DATE('01-JUL-2001','DD-MON-YYYY')
  AND TO_DATE('31-JUL-2001','DD-MON-YYYY')
GROUP BY TRUNC(order_dt);


--------- ----------- ------------
DAY       DAILY_SALES FIVE_DAY_AVG
--------- ----------- ------------
16-JUL-01         112          146
18-JUL-01         180          114
20-JUL-01          50          169
21-JUL-01          50   165.333333
22-JUL-01         396   165.333333
```

### FIRST_VALUE and LAST_VALUE
They are used with windowing functions to identify the values of the first and last values in the window.<br>
sample que :"How did each month's sales compare to the first month?"<br>

eg -<br>
In the case of the three-month rolling average query shown previously, you could display the values of all three months along with the average of the three, as in:<br>

```
SELECT month,
  FIRST_VALUE(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) prev_month,
  SUM(tot_sales) monthly_sales,
  LAST_VALUE(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) next_month,
  AVG(SUM(tot_sales)) OVER (ORDER BY month ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) rolling_avg
FROM orders
WHERE year = 2001 
  AND region_id = 6
GROUP BY month
ORDER BY month;

     MONTH PREV_MONTH MONTHLY_SALES NEXT_MONTH ROLLING_AVG
         1     610697        610697     428676    519686.5
         2     610697        428676     637031  558801.333
         3     428676        637031     541146  535617.667
         4     637031        541146     592935  590370.667
         5     541146        592935     501485  545188.667
```
### LAG/LEAD Functions
```
"LAG(VAL, N, <default value>) OVER () " -- N=1 by default.
"LEAD(VAL, N, <default value>) OVER () " -- N=1 by default.
```
Query - "Compute the total sales per month for the Mid-Atlantic region, including the percent change from the previous month" requires data from both the current and preceding rows to calculate the answer.

Step 1 - get prev month's data using LAG function.<br>
```
SELECT month, 
  SUM(tot_sales) monthly_sales,
  LAG(SUM(tot_sales), 1) OVER (ORDER BY month) prev_month_sales
FROM orders
WHERE year = 2001
  AND region_id = 6
GROUP BY month
ORDER BY month;

---------- ------------- ----------------
     MONTH MONTHLY_SALES PREV_MONTH_SALES
---------- ------------- ----------------
         1        610697
         2        428676           610697
         3        637031           428676
```
Step 2 - handle NULL values. <br>
If you see above, for month 1, PREV_MONTH_SALES is NULL.<br>
So, can calculate % change in sales.<br>
Here we keep current month sales as default value, and % sales in this case is 0%.<br>

```
SELECT 
	months.month month, 
	months.monthly_sales monthly_sales,
  ROUND((months.monthly_sales - months.prev_month_sales)/ months.prev_month_sales, 3) * 100 percent_change
FROM
 (
	SELECT month, 
		   SUM(tot_sales) monthly_sales,
		   LAG(SUM(tot_sales), 1, SUM(tot_sales)) OVER (ORDER BY month) prev_month_sales

    FROM orders
    WHERE year = 2001
    AND region_id = 6
    GROUP BY month) months
ORDER BY month;

---------- ------------- --------------
     MONTH MONTHLY_SALES PERCENT_CHANGE
---------- ------------- --------------
         1        610697              0
         2        428676          -29.8
         3        637031           48.6
```
		 
		 
		 
		 