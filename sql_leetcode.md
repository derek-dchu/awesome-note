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

##### MySQL
```sql
SELECT b.Score Score, c.Rank Rank 
FROM
  ( SELECT a.Score Score, @rnk:=@rnk+1 Rank 
    FROM
      ( SELECT DISTINCT Score 
        FROM Scores
        ORDER BY Score DESC ) a, 
      ( SELECT @rnk:=0 ) r
   ) c
JOIN Scores b 
ON c.Score = b.Score 
ORDER BY c.Rank;
```

##### Oracle
```sql
SELECT score AS "Score", DENSE_RANK() OVER (ORDER BY NVL(score, -1) DESC) as "Rank"
FROM scores;
```


### Consecutive Numbers
Write a SQL query to find all numbers that appear at least three times consecutively.
```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```
For example, given the above Logs table, 1 is the only number that appears consecutively for at least three times.

##### MySQL
```sql
SELECT DISTINCT cTable.Num
FROM
    ( SELECT
        Num, 
        CASE
            WHEN @prev=Num THEN @count:=@count+1
            WHEN (@prev:=Num) IS NOT NULL THEN @count:=1
        END c
    FROM 
        Logs, 
        (SELECT @prev:=NULL) p 
    ) cTable
WHERE cTable.c = 3;
```


### Employees Earning More Than Their Managers
The Employee table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.
```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```
Given the Employee table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.
```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

##### SQL
```sql
SELECT e2.name Employee
FROM Employee e1 LEFT OUTER JOIN Employee e2
ON e1.id = e2.managerid
WHERE e1.salary < e2.salary;
```


### Duplicate Emails
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

##### SQL
```sql
SELECT email
FROM person
GROUP BY email
HAVING COUNT(email) > 1;
```


### Customers Who Never Order
Suppose that a website contains two tables, the Customers table and the Orders table. Write a SQL query to find all customers who never order anything.

Table: Customers.
```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```
Table: Orders.
```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```
Using the above tables as example, return the following:
```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

##### SQL
```sql
SELECT c.Name Customers
FROM Customers c LEFT OUTER JOIN Orders o
ON c.Id = o.Customerid
GROUP BY c.Id, c.Name
HAVING COUNT(o.Id) = 0;
```


### Department Highest Salary
The Employee table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.
```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```
The Department table holds all departments of the company.
```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```
Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, Max has the highest salary in the IT department and Henry has the highest salary in the Sales department.
```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

##### SQL
```
SELECT t.deptname Department, e2.Name Employee, e2.Salary Salary
FROM 
    Employee e2, 
    ( SELECT d.id deptid, d.name deptname, MAX(e1.salary) s
      FROM Department d left outer join Employee e1
      ON d.id = e1.departmentid
      GROUP BY d.id, d.name ) t
WHERE 
    e2.Departmentid = t.deptid
AND
    e2.Salary = t.s;
```


### Department Top Three Salaries
The Employee table holds all employees. Every employee has an Id, and there is also a column for the department Id.
```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
```
The Department table holds all departments of the company.
```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```
Write a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, your SQL query should return the following rows.
```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

##### SQL
```sql
SELECT D.Name AS Department, E.Name AS Employee, E.Salary AS Salary 
FROM Employee E, Department D
WHERE ( SELECT COUNT(DISTINCT(Salary)) 
        FROM Employee 
        WHERE 
            DepartmentId = E.DepartmentId 
        AND 
            Salary > E.Salary) < 3
AND E.DepartmentId = D.Id 
ORDER BY E.DepartmentId, E.Salary DESC;
```

##### MySQL
```sql
SELECT d.Name Department, rst.ename Employee, rst.s Salary
FROM 
  Department d, 
  ( SELECT r.deptid, e.Name ename, e.Salary s, FIND_IN_SET(e.Salary, r.salaries) rank
    FROM 
      Employee e, 
      ( SELECT DepartmentId deptid, GROUP_CONCAT(DISTINCT Salary ORDER BY Salary DESC SEPARATOR ',') salaries
        FROM Employee
        GROUP BY DepartmentId ) r
    WHERE e.DepartmentId = r.deptid
  ) rst
WHERE
  d.id = rst.deptid
AND
  rst.rank < 4
ORDER BY d.id, rst.rank;
```

##### Oracle
```sql
SELECT d.name, r.ename, r.s
FROM 
    department d, 
    ( SELECT e1.departmentid deptid, e1.name ename, e1.salary s, DENSE_RANK() OVER ( PARTITION BY e1.departmentid ORDER BY e1.salary DESC ) rank
      FROM employee e1 ) r
WHERE 
    d.id = r.deptid
AND 
    r.rank < 4;
```


### Delete Duplicate Emails
Write a SQL query to delete all duplicate email entries in a table named Person, keeping only unique emails based on its smallest Id.
```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
```
Id is the primary key column for this table.
For example, after running your query, the above Person table should have the following rows:
```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

##### SQL
```sql
DELETE
FROM Person
WHERE 
  Id
NOT IN ( SELECT MIN(Id)
         FROM Person
         GROUP BY Email );
```

##### MySQL
**Note:** MySQL doesn't allow us reference that table in an inner query when we doing an UPDATE/INSERT/DELETE on the table. Therefore, we need to copy the subquery into a temporary table.

```sql
DELETE
FROM Person
WHERE 
  Id
NOT IN ( SELECT *
         FROM
           ( SELECT MIN(Id)
             FROM Person
             GROUP BY Email ) Remain );
```


### Rising Temperature
Given a Weather table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.
```
+---------+------------+------------------+
| Id(INT) | Date(DATE) | Temperature(INT) |
+---------+------------+------------------+
|       1 | 2015-01-01 |               10 |
|       2 | 2015-01-02 |               25 |
|       3 | 2015-01-03 |               20 |
|       4 | 2015-01-04 |               30 |
+---------+------------+------------------+
```
For example, return the following Ids for the above Weather table:
```
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```

##### MySQL
*  Self-join and look for adjunct dates.

    ```sql
    SELECT w1.Id Id
    FROM Weather w1 JOIN Weather w2
    ON DATEDIFF(w1.Date,w2.Date)=1
    WHERE w1.Temperature > w2.Temperature;
    ```

*  Sorting (faster then above)

    ```sql
    SELECT Id
    FROM ( SELECT
             CASE
               WHEN
                 Temperature > @prevtemp
               AND
                 DATEDIFF(Date, @prevdate) = 1 
               THEN Id
                 ELSE NULL
             END AS Id,
             @prevtemp:=Temperature,
             @prevdate:=Date
           FROM 
             Weather, 
             (SELECT @prevtemp:=NULL) AS pt, 
             (SELECT @prevdate:=NULL) AS pd
           ORDER BY Date ) AS rst 
    WHERE Id IS NOT NULL;
    ```

##### Oracle
*  Self-join and look for adjunct dates.

    ```sql
    SELECT w1.Id Id
    FROM Weather w1 JOIN Weather w2
    ON w1.d = w2.d + 1
    WHERE w1.Temperature > w2.Temperature;
    ```

*  Using LAG() which query previous rows at the same time.

    ```sql
    SELECT Id
    FROM ( SELECT 
             Id,
             Temperature t,
             LAG (Temperature,1) OVER (ORDER BY d) AS prev_t
           FROM Weather )
    WHERE t > prev_t;
    ```