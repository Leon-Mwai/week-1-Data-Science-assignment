# week-1-Data-Science-assignment

import sqlite3
import pandas as pd

conn = sqlite3.connect(":memory:")
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE employees (
    id INTEGER,
    name TEXT,
    age INTEGER,
    department TEXT,
    salary INTEGER
);
""")

<sqlite3.Cursor at 0x7f285e848bc0>


data = [
    (1, 'Alice', 30, 'HR', 50000),
    (2, 'Bob', 25, 'IT', 60000),
    (3, 'Charlie', 35, 'Finance', 70000),
    (4, 'Diana', 28, 'IT', 65000),
    (5, 'Ahmed', 40, 'Finance', 72000),
    (6, 'Ann', 23, 'HR', 48000),
]

cursor.executemany("INSERT INTO employees VALUES (?, ?, ?, ?, ?);", data)
conn.commit()

pd.read_sql("SELECT name, salary FROM employees WHERE department = 'IT' ORDER BY salary DESC;", conn)

name	salary
0	Diana	65000
1	Bob	60000

pd.read_sql("SELECT department, COUNT(*) AS employee_count FROM employees GROUP BY department;", conn)

department	employee_count
0	Finance	2
1	HR	2
2	IT	2

pd.read_sql("SELECT AVG(age) AS average_age FROM employees WHERE salary > 55000;", conn)

average_age
0	32.0

pd.read_sql("SELECT * FROM employees WHERE department = 'Finance' ORDER BY age ASC LIMIT 1;", conn)

	id	name	age	department	salary
0	3	Charlie	35	Finance	70000

pd.read_sql("SELECT COUNT(*) AS count_over_30 FROM employees WHERE age > 30;", conn)


count_over_30
0	2

SUM(salary) AS total_salary FROM employees GROUP BY department;", conn)

	department	total_salary
0	Finance	142000
1	HR	98000
2	IT	125000

pd.read_sql("SELECT department, AVG(salary) AS average_salary FROM employees GROUP BY department ORDER BY average_salary DESC LIMIT 1;", conn)


department	average_salary
0	Finance	71000.0
