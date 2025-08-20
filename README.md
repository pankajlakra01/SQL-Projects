--Create Database
Create database EmployeesDatabase;
Use EmployeesDatabase

-- Create the Departments table
-- Stores department names and their unique IDs.
CREATE TABLE Departments (
    dept_no CHAR(4) PRIMARY KEY,
    dept_name VARCHAR(40) NOT NULL UNIQUE
);

-- Create the Employees table
-- Stores employee details.
CREATE TABLE Employees (
    emp_no INT PRIMARY KEY,
    birth_date DATE NOT NULL,
    first_name VARCHAR(14) NOT NULL,
    last_name VARCHAR(16) NOT NULL,
    gender ENUM('M', 'F') NOT NULL,
    hire_date DATE NOT NULL
);

-- Create the Dept_Emp table (Junction Table)
-- Maps employees to the departments they have worked in.
CREATE TABLE Dept_Emp (
    emp_no INT,
    dept_no CHAR(4),
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
    PRIMARY KEY (emp_no, dept_no),
    FOREIGN KEY (emp_no) REFERENCES Employees(emp_no) ON DELETE CASCADE,
    FOREIGN KEY (dept_no) REFERENCES Departments(dept_no) ON DELETE CASCADE
);

-- Create the Titles table
-- Stores the job titles an employee has held.
CREATE TABLE Titles (
    emp_no INT,
    title VARCHAR(50) NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE,
    PRIMARY KEY (emp_no, title, from_date),
    FOREIGN KEY (emp_no) REFERENCES Employees(emp_no) ON DELETE CASCADE
);

-- Create the Salaries table
-- Stores the salary history of each employee.
CREATE TABLE Salaries (
    emp_no INT,
    salary INT NOT NULL,
    from_date DATE NOT NULL,
    to_date DATE NOT NULL,
    PRIMARY KEY (emp_no, from_date),
    FOREIGN KEY (emp_no) REFERENCES Employees(emp_no) ON DELETE CASCADE
);
50 SQL Queries for the Employees Database
Here are 50 queries designed to help you learn and practice, categorized by difficulty and concept.
Basic Retrieval Queries
1. Retrieve all information about all employees.
code
SQL
SELECT * FROM Employees;
2. List the first name, last name, and gender of all employees.
code
SQL
SELECT first_name, last_name, gender FROM Employees;
3. Find all employees hired after January 1, 2000.
code
SQL
SELECT * FROM Employees WHERE hire_date > '2000-01-01';
4. Get the names of all departments.
code
SQL
SELECT dept_name FROM Departments;
5. Find all unique job titles.
code
SQL
SELECT DISTINCT title FROM Titles;
6. Retrieve all salaries greater than $80,000.
code
SQL
SELECT * FROM Salaries WHERE salary > 80000;
7. List all male employees.
code
SQL
SELECT * FROM Employees WHERE gender = 'M';
8. List all employees, ordered by their last name alphabetically.
code
SQL
SELECT * FROM Employees ORDER BY last_name ASC;
9. Find the top 10 highest-paid employees based on their current salary.
code
SQL
SELECT * FROM Salaries ORDER BY salary DESC LIMIT 10;
10. Find all employees whose last name starts with 'P'.
code
SQL
SELECT * FROM Employees WHERE last_name LIKE 'P%';
Aggregate Queries
11. Count the total number of employees.
code
SQL
SELECT COUNT(*) AS total_employees FROM Employees;
12. Find the average salary of all employees.
code
SQL
SELECT AVG(salary) AS average_salary FROM Salaries;
13. Find the highest salary recorded.
code
SQL
SELECT MAX(salary) AS highest_salary FROM Salaries;
14. Find the lowest salary recorded.
code
SQL
SELECT MIN(salary) AS lowest_salary FROM Salaries;
15. Count the number of employees in each department.
code
SQL
SELECT dept_no, COUNT(emp_no) AS number_of_employees
FROM Dept_Emp
GROUP BY dept_no;
16. Find the total number of male and female employees.```sql
SELECT gender, COUNT(*) AS total_count
FROM Employees
GROUP BY gender;
code
Code
**17. Find the departments with more than 30,000 employees.**
```sql
SELECT dept_no, COUNT(emp_no) AS employee_count
FROM Dept_Emp
GROUP BY dept_no
HAVING COUNT(emp_no) > 30000;
18. Find the average salary for each job title.
code
SQL
-- Note: This is simplified. For accuracy, you'd join with Salaries.
SELECT t.title, AVG(s.salary) AS average_salary
FROM Titles t
JOIN Salaries s ON t.emp_no = s.emp_no
GROUP BY t.title;
19. Find the total salary expenditure per department.
code
SQL
SELECT de.dept_no, SUM(s.salary) AS total_salary_expenditure
FROM Salaries s
JOIN Dept_Emp de ON s.emp_no = de.emp_no
GROUP BY de.dept_no;
20. Find employees who have held more than one title.
code
SQL
SELECT emp_no, COUNT(title) AS number_of_titles
FROM Titles
GROUP BY emp_no
HAVING COUNT(title) > 1;
JOIN Queries
21. Retrieve the first name, last name, and salary of all employees.
code
SQL
SELECT e.first_name, e.last_name, s.salary
FROM Employees e
JOIN Salaries s ON e.emp_no = s.emp_no;
22. List all employees and the name of the department they work in.
code
SQL
SELECT e.first_name, e.last_name, d.dept_name
FROM Employees e
JOIN Dept_Emp de ON e.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no;
23. Find the current title for each employee.
code
SQL
SELECT e.emp_no, e.first_name, t.title
FROM Employees e
JOIN Titles t ON e.emp_no = t.emp_no
WHERE t.to_date = '9999-01-01'; -- A common convention for 'current'
24. Get the complete history of departments for a specific employee (e.g., emp_no = 10001).
code
SQL
SELECT d.dept_name, de.from_date, de.to_date
FROM Dept_Emp de
JOIN Departments d ON de.dept_no = d.dept_no
WHERE de.emp_no = 10001;
25. List all department managers, including their department name, first name, and last name.
code
SQL
-- Assuming a `Dept_Manager` table exists or filtering by title
SELECT d.dept_name, e.first_name, e.last_name
FROM Employees e
JOIN Titles t ON e.emp_no = t.emp_no
JOIN Dept_Emp de ON e.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no
WHERE t.title = 'Manager' AND t.to_date = '9999-01-01';
26. Find the salary of each manager.
code
SQL
SELECT e.first_name, e.last_name, s.salary
FROM Employees e
JOIN Titles t ON e.emp_no = t.emp_no
JOIN Salaries s ON e.emp_no = s.emp_no
WHERE t.title = 'Manager' AND t.to_date = '9999-01-01' AND s.to_date = '9999-01-01';
27. Retrieve a list of employees with their current department and salary.
code
SQL
SELECT e.first_name, e.last_name, d.dept_name, s.salary
FROM Employees e
JOIN Dept_Emp de ON e.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no
JOIN Salaries s ON e.emp_no = s.emp_no
WHERE de.to_date = '9999-01-01' AND s.to_date = '9999-01-01';
28. Find the names of all employees who work in the 'Sales' department.
code
SQL
SELECT e.first_name, e.last_name
FROM Employees e
JOIN Dept_Emp de ON e.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no
WHERE d.dept_name = 'Sales';
29. List all titles held by employees in the 'Development' department.
code
SQL
SELECT DISTINCT t.title
FROM Titles t
JOIN Dept_Emp de ON t.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no
WHERE d.dept_name = 'Development';
30. Find the total number of employees for each title.
code
SQL
SELECT title, COUNT(emp_no) AS employee_count
FROM Titles
GROUP BY title
ORDER BY employee_count DESC;
Subquery Queries
31. Find employees who have the same birth date as employee number 101010.
code
SQL
SELECT * FROM Employees
WHERE birth_date = (SELECT birth_date FROM Employees WHERE emp_no = 101010);
32. Retrieve the titles of employees who were hired on the same day as employee number 10044.
code
SQL
SELECT title FROM Titles
WHERE emp_no IN (SELECT emp_no FROM Employees WHERE hire_date = (SELECT hire_date FROM Employees WHERE emp_no = 10044));
33. Find all employees whose salary is higher than the average salary.
code
SQL
SELECT e.emp_no, e.first_name, e.last_name
FROM Employees e
JOIN Salaries s ON e.emp_no = s.emp_no
WHERE s.salary > (SELECT AVG(salary) FROM Salaries);
34. List all employees who are currently managers.
code
SQL
SELECT * FROM Employees
WHERE emp_no IN (SELECT emp_no FROM Titles WHERE title = 'Manager' AND to_date = '9999-01-01');
35. Find the names of employees who have the highest salary.
code
SQL
SELECT e.first_name, e.last_name
FROM Employees e
JOIN Salaries s ON e.emp_no = s.emp_no
WHERE s.salary = (SELECT MAX(salary) FROM Salaries);
36. Get the department name for the employee with the most recent hire date.
code
SQL
SELECT d.dept_name
FROM Departments d
JOIN Dept_Emp de ON d.dept_no = de.dept_no
WHERE de.emp_no = (SELECT emp_no FROM Employees ORDER BY hire_date DESC LIMIT 1);
37. List all employees in the 'Marketing' department hired after the department's average hire date.
code
SQL
SELECT * FROM Employees e
WHERE e.emp_no IN (SELECT emp_no FROM Dept_Emp WHERE dept_no = 'd001') -- Assuming Marketing is d001
AND e.hire_date > (
    SELECT AVG(e_inner.hire_date)
    FROM Employees e_inner
    JOIN Dept_Emp de_inner ON e_inner.emp_no = de_inner.emp_no
    WHERE de_inner.dept_no = 'd001'
);
38. Find employees who have never been a 'Manager'.
code
SQL
SELECT * FROM Employees
WHERE emp_no NOT IN (SELECT emp_no FROM Titles WHERE title = 'Manager');
39. Find the current salary of employees who have also worked in the 'Production' department at some point.
code
SQL
SELECT s.salary
FROM Salaries s
WHERE s.to_date = '9999-01-01'
AND s.emp_no IN (
    SELECT emp_no
    FROM Dept_Emp de
    JOIN Departments d ON de.dept_no = d.dept_no
    WHERE d.dept_name = 'Production'
);
40. Find the job title that has the highest average salary.
code
SQL
SELECT t.title, AVG(s.salary) AS avg_sal
FROM Titles t
JOIN Salaries s ON t.emp_no = s.emp_no
GROUP BY t.title
ORDER BY avg_sal DESC
LIMIT 1;```

---

### Advanced and Miscellaneous Queries

**41. Find the number of employees hired each year.**
```sql
SELECT YEAR(hire_date) AS hire_year, COUNT(emp_no) AS number_of_hires
FROM Employees
GROUP BY hire_year
ORDER BY hire_year;
42. Rank employees in the 'Sales' department by their salary.
code
SQL
SELECT e.first_name, e.last_name, s.salary,
       RANK() OVER (ORDER BY s.salary DESC) AS salary_rank
FROM Employees e
JOIN Salaries s ON e.emp_no = s.emp_no
JOIN Dept_Emp de ON e.emp_no = de.emp_no
WHERE de.dept_no = 'd007' AND s.to_date = '9999-01-01'; -- Assuming Sales is d007
43. Find the salary progression for a specific employee (e.g., emp_no = 10001).
code
SQL
SELECT salary, from_date, to_date
FROM Salaries
WHERE emp_no = 10001
ORDER BY from_date;
44. Calculate the tenure of each employee in years.
code
SQL
SELECT emp_no, first_name, last_name,
       DATEDIFF(CURDATE(), hire_date) / 365 AS tenure_in_years
FROM Employees;
45. Categorize employees based on their salary: 'High', 'Medium', 'Low'.
code
SQL
SELECT emp_no, salary,
       CASE
           WHEN salary > 100000 THEN 'High'
           WHEN salary BETWEEN 60000 AND 100000 THEN 'Medium'
           ELSE 'Low'
       END AS salary_category
FROM Salaries
WHERE to_date = '9999-01-01';
46. Find the average salary of employees hired in each decade.
code
SQL
SELECT (FLOOR(YEAR(e.hire_date) / 10)) * 10 AS decade, AVG(s.salary) AS avg_salary
FROM Employees e
JOIN Salaries s ON e.emp_no = s.emp_no
GROUP BY decade
ORDER BY decade;
47. Find the department with the highest average employee tenure.
code
SQL
SELECT d.dept_name, AVG(DATEDIFF(de.to_date, de.from_date)) AS avg_tenure_days
FROM Dept_Emp de
JOIN Departments d ON de.dept_no = d.dept_no
GROUP BY d.dept_name
ORDER BY avg_tenure_days DESC
LIMIT 1;```

**48. Find the employee with the second-highest salary in each department.**
```sql
WITH RankedSalaries AS (
    SELECT e.emp_no, d.dept_name, s.salary,
           DENSE_RANK() OVER (PARTITION BY d.dept_name ORDER BY s.salary DESC) as rank_num
    FROM Employees e
    JOIN Salaries s ON e.emp_no = s.emp_no
    JOIN Dept_Emp de ON e.emp_no = de.emp_no
    JOIN Departments d ON de.dept_no = d.dept_no
    WHERE s.to_date = '9999-01-01' AND de.to_date = '9999-01-01'
)
SELECT emp_no, dept_name, salary
FROM RankedSalaries
WHERE rank_num = 2;
49. Find the year-over-year growth in the number of new hires.
code
SQL
WITH YearlyHires AS (
    SELECT YEAR(hire_date) AS hire_year, COUNT(emp_no) AS num_hires
    FROM Employees
    GROUP BY hire_year
)
SELECT
    yh1.hire_year,
    yh1.num_hires,
    LAG(yh1.num_hires, 1, 0) OVER (ORDER BY yh1.hire_year) AS previous_year_hires,
    (yh1.num_hires - LAG(yh1.num_hires, 1, 0) OVER (ORDER BY yh1.hire_year)) AS growth
FROM YearlyHires yh1;
50. Calculate the moving average of salaries over time.
code
SQL
SELECT from_date, salary,
       AVG(salary) OVER (ORDER BY from_date ROWS BETWEEN 30 PRECEDING AND CURRENT ROW) AS moving_avg_salary
FROM Salaries
ORDER BY from_date;
10 More Important JOINs for the Employees Database
Here are 10 more practical and insightful JOIN queries that demonstrate different techniques.
1. The "All-In-One" Current Employee Status (Multi-Table INNER JOIN)
This is a powerful query for generating a complete, current snapshot of every employee, joining all relevant tables.
Query:
code
SQL
SELECT
    e.emp_no,
    e.first_name,
    e.last_name,
    d.dept_name,
    s.salary,
    t.title,
    e.hire_date
FROM Employees AS e
JOIN Dept_Emp AS de ON e.emp_no = de.emp_no
JOIN Departments AS d ON de.dept_no = d.dept_no
JOIN Salaries AS s ON e.emp_no = s.emp_no
JOIN Titles AS t ON e.emp_no = t.emp_no
WHERE
    de.to_date = '9999-01-01'
    AND s.to_date = '9999-01-01'
    AND t.to_date = '9999-01-01';
Explanation: This query links an employee to their current department, current salary, and current title by joining five tables. The WHERE clause is crucial, filtering each of the time-sensitive tables (Dept_Emp, Salaries, Titles) to only include the records that are still active.
2. Finding Employees Who Have Changed Departments (JOIN with Aggregation)
This query identifies employees who have a history in more than one department, which can be useful for identifying versatile or long-term employees.
Query:
code
SQL
SELECT
    e.emp_no,
    e.first_name,
    e.last_name,
    COUNT(de.dept_no) AS number_of_departments
FROM Employees AS e
JOIN Dept_Emp AS de ON e.emp_no = de.emp_no
GROUP BY
    e.emp_no, e.first_name, e.last_name
HAVING
    COUNT(de.dept_no) > 1;
Explanation: This joins Employees with Dept_Emp and then groups by employee. The HAVING clause filters these groups, keeping only those where the count of department entries is greater than one.
3. Finding Departments With No Current Employees (LEFT JOIN)
This is a data integrity check to find if there are any departments that are listed but are currently empty.
Query:
code
SQL
SELECT
    d.dept_no,
    d.dept_name
FROM Departments AS d
LEFT JOIN Dept_Emp AS de ON d.dept_no = de.dept_no AND de.to_date = '9999-01-01'
WHERE
    de.emp_no IS NULL;
Explanation: A LEFT JOIN is used to select all departments and tries to match them with current employee assignments. If a department has no current employees, the columns from Dept_Emp will be NULL, and the WHERE clause isolates these specific departments.
4. Comparing an Employee's Salary to their Department's Average (JOIN with a Subquery)
This complex query provides context for an employee's salary by showing how it compares to the average salary within their own department.
Query:
code
SQL
SELECT
    e.emp_no,
    d.dept_name,
    s.salary AS employee_salary,
    dept_avg.avg_salary AS department_average_salary
FROM Employees AS e
JOIN Salaries AS s ON e.emp_no = s.emp_no
JOIN Dept_Emp AS de ON e.emp_no = de.emp_no
JOIN Departments AS d ON de.dept_no = d.dept_no
JOIN (
    SELECT de.dept_no, AVG(s.salary) as avg_salary
    FROM Salaries s
    JOIN Dept_Emp de ON s.emp_no = de.emp_no
    WHERE s.to_date = '9999-01-01' AND de.to_date = '9999-01-01'
    GROUP BY de.dept_no
) AS dept_avg ON d.dept_no = dept_avg.dept_no
WHERE
    s.to_date = '9999-01-01' AND de.to_date = '9999-01-01';
Explanation: The core of this query is the subquery aliased as dept_avg. This subquery first calculates the average salary for each department. The outer query then joins this result back to the main employee information, allowing for a direct comparison in the final output.
5. Finding Pairs of Employees in the Same Department (SELF JOIN)
A SELF JOIN is perfect for comparing rows within the same table. This query finds pairs of employees who currently work in the same department.
Query:
code
SQL
SELECT
    CONCAT(e1.first_name, ' ', e1.last_name) AS employee_1,
    CONCAT(e2.first_name, ' ', e2.last_name) AS employee_2,
    d.dept_name
FROM Employees AS e1
JOIN Dept_Emp AS de1 ON e1.emp_no = de1.emp_no
JOIN Dept_Emp AS de2 ON de1.dept_no = de2.dept_no
JOIN Employees AS e2 ON de2.emp_no = e2.emp_no
JOIN Departments AS d ON de1.dept_no = d.dept_no
WHERE
    de1.to_date = '9999-01-01'
    AND de2.to_date = '9999-01-01'
    AND e1.emp_no < e2.emp_no;
Explanation: We join Dept_Emp to itself (de1 and de2) on the department number. This creates pairs of all employees in the same department. We then join back to the Employees table twice to get their names. The condition e1.emp_no < e2.emp_no is crucial to prevent duplicate pairs (e.g., showing 'John-Jane' and 'Jane-John') and to stop employees from being paired with themselves.
6. Listing Employees Hired Before Their Current Manager (Complex JOIN)
This insightful query identifies employees who have been at the company longer than their current manager.
Query:
code
SQL
WITH CurrentManagers AS (
    SELECT de.dept_no, e.emp_no, e.hire_date AS manager_hire_date
    FROM Employees e
    JOIN Dept_Emp de ON e.emp_no = de.emp_no
    JOIN Titles t ON e.emp_no = t.emp_no
    WHERE t.title = 'Manager' AND t.to_date = '9999-01-01' AND de.to_date = '9999-01-01'
)
SELECT
    e.emp_no,
    e.first_name,
    e.last_name,
    e.hire_date AS employee_hire_date,
    d.dept_name,
    cm.manager_hire_date
FROM Employees e
JOIN Dept_Emp de ON e.emp_no = de.emp_no
JOIN Departments d ON de.dept_no = d.dept_no
JOIN CurrentManagers cm ON de.dept_no = cm.dept_no
WHERE
    de.to_date = '9999-01-01'
    AND e.hire_date < cm.manager_hire_date;
Explanation: We first create a Common Table Expression (CTE) called CurrentManagers to easily identify each department's current manager and their hire date. Then, we join all current employees to this CTE on their department number and use a WHERE clause to filter for employees whose hire date is earlier than their manager's.
7. Cross-Departmental Title Analysis (JOIN with GROUP BY)
Find out which job titles exist in which departments. This is great for understanding organizational structure.
Query:
code
SQL
SELECT
    d.dept_name,
    t.title,
    COUNT(t.emp_no) AS number_of_employees_with_title
FROM Titles AS t
JOIN Dept_Emp AS de ON t.emp_no = de.emp_no
JOIN Departments AS d ON de.dept_no = d.dept_no
WHERE
    t.to_date = '9999-01-01'
    AND de.to_date = '9999-01-01'
GROUP BY
    d.dept_name,
    t.title
ORDER BY
    d.dept_name,
    number_of_employees_with_title DESC;
Explanation: This query joins the three tables to link titles to departments for current employees. The GROUP BY clause then aggregates this data, counting how many employees hold a specific title within a specific department.
8. Employees Who Have Never Gotten a Raise (LEFT JOIN)
This query assumes a raise is reflected by having more than one entry in the Salaries table.
Query:
code
SQL
SELECT
    e.emp_no,
    e.first_name,
    e.last_name
FROM Employees AS e
JOIN Salaries AS s ON e.emp_no = s.emp_no
GROUP BY
    e.emp_no, e.first_name, e.last_name
HAVING
    COUNT(s.from_date) = 1;
Explanation: Although this uses GROUP BY and HAVING, the key concept is identifying a group based on the number of matches found after a JOIN. By grouping all salary entries for each employee, we can use HAVING COUNT(...) = 1 to find those who have only ever had one salary record.
9. Showing Department Name Changes for Employees (SELF JOIN on Junction Table)
This is an advanced query to see the career path of an employee moving between departments.
Query:
code
SQL
SELECT
    e.emp_no,
    e.first_name,
    d1.dept_name AS previous_department,
    d2.dept_name AS current_department,
    de2.from_date AS date_of_change
FROM Dept_Emp AS de1
JOIN Dept_Emp AS de2 ON de1.emp_no = de2.emp_no AND de1.to_date = de2.from_date
JOIN Employees AS e ON de1.emp_no = e.emp_no
JOIN Departments AS d1 ON de1.dept_no = d1.dept_no
JOIN Departments AS d2 ON de2.dept_no = d2.dept_no
ORDER BY
    e.emp_no;
Explanation: The key is self-joining Dept_Emp where the to_date of one record is the same as the from_date of the next record for the same employee. This effectively finds consecutive department assignments, clearly showing the transition from one to another.
10. UNION vs JOIN: Employees Hired or Managed in the 90s
This demonstrates how a JOIN is often better than a UNION for combining different attributes of the same entity. Let's find employees who were either hired in the 90s or are currently managed by someone who was hired in the 90s.
Query (JOIN approach):
code
SQL
WITH Managers90s AS (
    SELECT de.dept_no
    FROM Employees e
    JOIN Dept_Emp de ON e.emp_no = de.emp_no
    JOIN Titles t ON e.emp_no = t.emp_no
    WHERE t.title = 'Manager'
    AND t.to_date = '9999-01-01'
    AND YEAR(e.hire_date) BETWEEN 1990 AND 1999
)
SELECT DISTINCT e.emp_no, e.first_name, e.last_name
FROM Employees e
LEFT JOIN Dept_Emp de ON e.emp_no = de.emp_no
LEFT JOIN Managers90s m90 ON de.dept_no = m90.dept_no
WHERE
    de.to_date = '9999-01-01'
    AND (YEAR(e.hire_date) BETWEEN 1990 AND 1999 OR m90.dept_no IS NOT NULL);
Explanation: This query is more efficient. We create a CTE of departments managed by people hired in the 90s. Then, we select all employees who either meet the hire-date criteria themselves OR who currently work in one of the departments identified in the CTE. This avoids scanning tables multiple times as a UNION would.
