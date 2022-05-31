### Common Table Expressions (CTEs)


#### What are Common Table Expressions (CTEs)?
A Common Table Expression (CTE) is the result set of a query which exists temporarily and for use only within the context of a larger query. <br>
Much like a derived table, the result of a CTE is not stored and exists only for the duration of the query. 

Common Table expression (CTE) is a temporary named result set that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement. <br>
You can also use a CTE in a CREATE a view, as part of the view’s SELECT query. 


#### How are CTEs helpful?
CTEs, like database views and derived tables, enable users to more easily write and maintain complex queries via increased readability and simplification. <br>
This reduction in complexity is achieved by deconstructing ordinarily complex queries into simple blocks to be used, and reused if necessary, in rewriting the query.  <br>
Example use cases include: <br>
- Needing to reference a derived table multiple times in a single query <br>
- An alternative to creating a view in the database <br>
- Performing the same calculation multiple times over across multiple query components <br>

#### How to create CTE?
We can define CTEs by adding a WITH clause directly before SELECT, INSERT, UPDATE, DELETE, or MERGE statement. <br>
The WITH clause can include one or more CTEs separated by commas. <br>
The following syntax can be followed: 

```
[WITH  [, ...]]  
 
::=
cte_name [(column_name [, ...])]
AS (cte_query) 
```
After you define your WITH clause with the CTEs, you can then reference the CTEs as you would refer any other table.  <br>
However, you can refer a CTE only within the execution scope of the statement that immediately follows the WITH clause. After you’ve run your statement, the CTE result set is not available to other statements.