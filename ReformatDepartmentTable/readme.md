Table: `Department`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
(id, month) is the primary key of this table.
The table has information about the revenue of each department per month.
The month has values in ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"].
```

Write an SQL query to reformat the table such that there is a department id column and a revenue column for each month.

The query result format is in the following example:

```
Department table:
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+

Result table:
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+

Note that the result table has 13 columns (1 for the department id + 12 for the months).
```

## 思考流程

我們的目標在於取出各個部門(id)的每年各個月的收入，每 1 列是 1 個部門是 1 個 row，每 1 部門都有 12 個月的收入，也就是 12 個 column，再加上部門 id 總共是 13 個 column。

也就是 有幾個部門就有幾個 row，每個 row 都要有 12 個月收入以及部門 id，具體操作如下：


* 以群組分門別類取出收入，完成 row
```sql
SELECT *
FROM Deparment
GROUP BY id
```
* 然後算 13 個 column
```sql
SELECT 
    id,
    Jan_Revenue,
    Feb_Revenue,
    May_Revenue,
    Jul_Revenue,
    Aug_Revenue,
	Sep_Revenue,
	Oct_Revenue,
    Nov_Revenue,
    Dec_Revenue
FROM Deparment
GROUP BY id
```
* 由於沒有上述的欄位，必須透過我們自己計算並使用 `AS` 才行，而這裡可以使用 `SUM` 以及 `CASE WHEN` 來搭配：
```sql
SELECT  
	id,
    SUM(CASE month WHEN 'Jan' THEN revenue ELSE null END ) as Jan_Revenue,
	SUM(CASE month WHEN 'Feb' THEN revenue ELSE null END ) as Feb_Revenue,
	SUM(CASE month WHEN 'Mar' THEN revenue ELSE null END ) as Mar_Revenue,
	SUM(CASE month WHEN 'Apr' THEN revenue ELSE null END ) as Apr_Revenue,
	SUM(CASE month WHEN 'May' THEN revenue ELSE null END ) as May_Revenue,
	SUM(CASE month WHEN 'Jun' THEN revenue ELSE null END ) as Jun_Revenue,
	SUM(CASE month WHEN 'Jul' THEN revenue ELSE null END ) as Jul_Revenue,
	SUM(CASE month WHEN 'Aug' THEN revenue ELSE null END ) as Aug_Revenue,
	SUM(CASE month WHEN 'Sep' THEN revenue ELSE null END ) as Sep_Revenue,
	SUM(CASE month WHEN 'Oct' THEN revenue ELSE null END ) as Oct_Revenue,
	SUM(CASE month WHEN 'Nov' THEN revenue ELSE null END ) as Nov_Revenue,
	SUM(CASE month WHEN 'Dec' THEN revenue ELSE null END ) as Dec_Revenue
FROM Department 
GROUP BY id
```
