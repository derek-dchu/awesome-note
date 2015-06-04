# LeetCode

### Combine Two Tables
Table: Person
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
```
PersonId is the primary key column for this table.

Table: Address
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
```
AddressId is the primary key column for this table.

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

FirstName, LastName, City, State

###### SQL
```sql
SELECT p.firstname 'FirstName', p.lastname 'LastName', a.city 'City', a.state 'State'
FROM Person p LEFT OUTER JOIN Address a
ON p.personid = a.personid;
```


### Second Highest Salary
Write a SQL query to get the second highest salary from the Employee table.
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the second highest salary is 200. If there is no second highest salary, then the query should return `null`.

##### SQL
```sql
SELECT MAX(e1.salary) 'SecondHighestSalary'
FROM Employee e1
WHERE e1.salary <> (SELECT MAX(e2.salary)
                    FROM Employee e2);
```

**Note:** MAX() will return `null` if record is not found.


