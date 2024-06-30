

```markdown
# SQL Server Practice Queries

Welcome to my SQL Server Practice Queries repository. This project contains a comprehensive set of SQL queries and scripts that I have practiced to enhance my skills in data management, analysis, and manipulation using SQL Server. Below, you will find detailed information about the project, including the structure, usage instructions, and examples of the queries implemented.


## Introduction

This repository showcases various SQL Server queries that demonstrate different aspects of SQL, including table creation, data manipulation, and complex queries. The goal is to provide a collection of practical examples that can be used for learning and reference purposes.

## Features

- Creation of tables with sample data.
- Complex queries to filter, aggregate, and manipulate data.
- Examples of subqueries, joins, common table expressions (CTEs), and window functions.
- Scripts to create and populate calendar dimension tables.

## Getting Started

### Prerequisites

To run the scripts in this repository, you will need:
- SQL Server installed on your machine.
- SQL Server Management Studio (SSMS) or any other SQL client tool.

### Installation

1. Clone the repository to your local machine using:
   ```sh
   git clone https://github.com/yourusername/sql-server-practice-queries.git
   ```
2. Open the SQL scripts in your SQL client tool.

## Usage

### Table Creation and Data Insertion

The `emp` table is created and populated with sample data as shown below:

```sql
CREATE TABLE emp (
    emp_id INT,
    emp_name VARCHAR(20),
    department_id INT,
    salary INT,
    manager_id INT,
    emp_age INT
);

INSERT INTO emp VALUES
(1, 'Ankit', 100, 10000, 4, 39),
(2, 'Mohit', 100, 15000, 5, 48),
(3, 'Vikas', 100, 10000, 4, 37),
(4, 'Rohit', 100, 5000, 2, 16),
(5, 'Mudit', 200, 12000, 6, 55),
(6, 'Agam', 200, 12000, 2, 14),
(7, 'Sanjay', 200, 9000, 2, 13),
(8, 'Ashish', 200, 5000, 2, 12),
(9, 'Mukesh', 300, 6000, 6, 51),
(10, 'Rakesh', 300, 7000, 6, 50);
```

### Query Examples

1. **Query to get employees with salary above department average:**
   ```sql
   SELECT * FROM emp e1
   WHERE salary > (SELECT AVG(e2.salary) 
                   FROM emp e2 
                   WHERE e1.department_id = e2.department_id);
   ```

2. **Query to get employees with specific departments:**
   ```sql
   SELECT * FROM emp
   WHERE department_id IN (100, 200);
   ```

3. **Creating and selecting from a calendar dimension table:**
   ```sql
   WITH cte AS (
       SELECT CAST('2000-01-01' AS DATE) AS cal_date
       UNION ALL
       SELECT DATEADD(DAY, 1, cal_date)
       FROM cte
       WHERE cal_date < '2050-12-31'
   )
   SELECT ROW_NUMBER() OVER (ORDER BY cal_date) AS id, *
   INTO calender_dim
   FROM cte
   OPTION (MAXRECURSION 0);
   ```

4. **Query to get records from `calender_dim` with relation to today:**
   ```sql
   WITH today AS (
       SELECT * FROM calender_dim WHERE cal_date = CAST(GETDATE() AS DATE)
   ), 
   cal AS (
       SELECT c.*, t.cal_year AS todays_year
       FROM calender_dim c
       CROSS JOIN today t
       WHERE c.cal_year BETWEEN t.cal_year - 2 AND t.cal_year
   )
   SELECT 'Records' AS Title, *
   FROM cal;
   ```

