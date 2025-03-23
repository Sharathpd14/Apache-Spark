# Spark SQL Queries: Writing Efficient and Scalable Queries

Spark SQL allows you to interact with structured data using SQL queries while benefiting from Spark’s parallel processing capabilities. In this blog, we’ll cover Spark SQL queries, from basic selections to advanced optimizations.

## 🛠️ Setting Up Spark SQL

Before writing queries, let’s create a SparkSession (the entry point for Spark SQL).

```python
from pyspark.sql import SparkSession

# Create a Spark session
spark = SparkSession.builder.appName("SparkSQLQueries").getOrCreate()
```


## 🔍 Querying Data Using Spark SQL

### Loading Data into Spark SQL

We need data to query! Let’s load a JSON file and create a temporary SQL table.

```python
df = spark.read.json("employees.json")

# Register as a temporary SQL table
df.createOrReplaceTempView("employees")
```

## 📌 Basic SQL Queries in Spark SQL

### ✅ Selecting Data

### SQL:
```sql
SELECT * FROM employees;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT * FROM employees").show()
```
### ✅ Filtering Data (WHERE Clause)

### SQL:
```sql
SELECT name, salary FROM employees WHERE salary > 50000;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT name, salary FROM employees WHERE salary > 50000").show()
```
### ✅ Sorting Data (ORDER BY)

### SQL:
```sql
SELECT name, salary FROM employees ORDER BY salary DESC;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT name, salary FROM employees ORDER BY salary DESC").show()
```
### ✅ Limiting Results

### SQL:
```sql
SELECT * FROM employees LIMIT 5;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT * FROM employees LIMIT 5").show()
```
### 📌 Aggregations & Grouping

### ✅ Counting Rows

#### SQL:
```sql
SELECT COUNT(*) FROM employees;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT COUNT(*) FROM employees").show()
```
### ✅ Grouping Data (GROUP BY)

#### SQL:
```sql
SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department;
```

💡 **Python Equivalent:**
```python
spark.sql("SELECT department, AVG(salary) AS avg_salary FROM employees GROUP BY department").show()
```
