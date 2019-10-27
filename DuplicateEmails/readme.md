# Duplicate Emails
Write a SQL query to find all duplicate emails in a table named Person.


```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

For example, your query should return the following for the above table:
```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```
Note: All emails are in lowercase.

## 流程
* 取得可辨識是否為重複的 數值
* 計算數值是否為重複

## `Group By` + `Having`

```sql
SELECT Email FROM Person GROUP BY Email
HAVING COUNT(Email) > 1;
```

## `Group By` + `Subquery`

```sql

SELECT Email FROM Person WHERE Id NOT IN (
    SELECT Id FROM Person GROUP BY email
)


```

等待驗證
