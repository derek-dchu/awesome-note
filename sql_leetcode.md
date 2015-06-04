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

### Nth Highest Salary
Write a SQL query to get the nth highest salary from the Employee table.
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return `null`.

##### MySQL
```sql
DELIMITER //
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N-1;
  RETURN (
      SELECT DISTINCT salary
      FROM Employee
      ORDER BY salary DESC
      LIMIT N, 1
  );
END//
DELIMITER ;
```

##### Oracle
```sql
SELECT *
FROM
    ( SELECT sal, ROWNUM rnum
      FROM (SELECT DISTINCT NVL(salary, -1) as sal
            FROM person)
      ORDER BY sal DESC
    )
WHERE rnum = N;
```


### Rank Scores
Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.
```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```
For example, given the above Scores table, your query should generate the following report (order by highest score):
```
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

##### Oracle
```sql
SELECT score AS "Score", DENSE_RANK() OVER (ORDER BY NVL(score, -1) DESC) as "Rank"
FROM scores;
```