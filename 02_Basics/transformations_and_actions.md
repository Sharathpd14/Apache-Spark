# 🚀 Understanding Spark Operations: Transformations & Actions

When working with distributed data in Spark, operations fall into two main categories:


## 🔄 Transformations - The Art of Shaping Data 
Transformations modify a Spark **DataFrame** without altering the original data. Instead, they return a **new** DataFrame, maintaining **immutability** in Spark. [More...](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)

### 📌 Key Characteristics of Transformations  
- **Immutable** – The original dataset remains unchanged.  
- **Lazy Evaluation** – Transformations are not executed immediately; they are only computed when an **action** is triggered.  
- **Creates a New Dataset** – The transformed output is stored in a separate DataFrame.  

### ✨ Common Transformations  
- `select()` – Retrieves specific columns from the dataset.  
- `filter()` – Returns a subset of data based on given conditions.  
- `map()` – Applies a function to each element in the dataset.  
- `groupBy()` – Groups data based on specified attributes.  

### 📝 Example  
```python
df_filtered = df.filter(df["country"] == "India")
```

## ⚡ Actions - The Power of Execution   

### 🚀 What Are Actions?  
In Apache Spark, **actions** are operations that **trigger the execution** of all previously recorded transformations. Unlike transformations, which are **lazy** and only build a lineage, actions **force computation** and return results. [More...](https://spark.apache.org/docs/latest/rdd-programming-guide.html#actions)

### 🔍 How Actions Work  
1️⃣ **Transformations are recorded but not executed**.  
2️⃣ **An action is called**, triggering Spark to execute all preceding transformations.  
3️⃣ **Results are returned** to the driver or written to storage.  

### 📝 Example  
```python
from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder.appName("Example").getOrCreate()

# Sample DataFrame
data = [(1, "Alice", 25), (2, "Bob", 17), (3, "Charlie", 30)]
df = spark.createDataFrame(data, ["id", "name", "age"])

# 🚀 Transformation (Lazy)
filtered_df = df.filter(df.age > 18)  # Doesn't execute yet

# ⚡ Action (Triggers execution)
filtered_df.show()  # Now Spark processes and shows results

```
![Image](https://github.com/user-attachments/assets/42f86eba-dc1c-42f6-abaa-467aacd2ac4d)
Image Source: [InfinitePy](https://infinitepy.com/p/main-transformations-actions-available-apache-spark-dataframe-overview-practical-examples)
## 🚀 Lazy Evaluation in Apache Spark  

### 🧐 What is Lazy Evaluation?  
In Apache Spark, **all transformations are evaluated lazily**. This means their results are **not computed immediately**. Instead, Spark records them as a **lineage** (a history of transformations applied to the data).  

### 🔍 How Does Lazy Evaluation Work?  
- Transformations are **not executed** right away.  
- Spark **remembers** the sequence of operations as a **lineage graph**.  
- When an **action** (e.g., `.count()`, `.collect()`) is triggered, Spark **optimizes** and executes the transformations efficiently.  
- Spark can **rearrange, coalesce, and optimize transformations** into stages for better performance.  

### ⚡ Why is Lazy Evaluation Important?  
✅ **Optimized Execution** – Spark groups transformations and executes them more efficiently.  
✅ **Fault Tolerance** – The recorded lineage allows Spark to **recompute lost data** if needed.  
✅ **Resource Efficiency** – Only required computations are performed, reducing memory usage.  

## 📝 Example  
```python
df_filtered = df.filter(df["age"] > 30)  # Transformation (lazy)
df_selected = df_filtered.select("name", "age")  # Another transformation (lazy)

df_selected.show()  # Action (triggers execution)
```

## ⚡ Transformations vs. Actions in Apache Spark  

### 🔄 Transformations  

## 🔄 Transformations  

| Transformation  | Description | Example Usage |
|---------------|-------------|--------------|
| `map()`       | Applies a function to each element. | `rdd.map(lambda x: x * 2)` |
| `join()`      | Performs an inner join between two RDDs. | `rdd1.join(rdd2)` |
| `union()`     | Combines two RDDs. | `rdd1.union(rdd2)` |
| `distinct()`  | Removes duplicate elements. | `rdd.distinct()` |
| `mapPartitions()` | Applies a function to each partition. | `rdd.mapPartitions(func)` |
| `flatMap()`   | Flattens the result into a single collection. | `rdd.flatMap(lambda x: x.split(" "))` |
| `intersection()` | Returns common elements between RDDs. | `rdd1.intersection(rdd2)` |
| `pipe()`      | Transforms an RDD using an external command. | `rdd.pipe("grep ERROR")` |
| `repartition()` | Changes the number of partitions (**full shuffle**). | `rdd.repartition(10)` |
| `coalesce()`  | Reduces partitions **without shuffle**. | `rdd.coalesce(5)` |
| `cartesian()` | Returns all possible pairs (Cartesian product). | `rdd1.cartesian(rdd2)` |
| `cogroup()`   | Groups values from multiple RDDs by key. | `rdd1.cogroup(rdd2)` |
| `filter()`    | Filters elements based on a condition. | `rdd.filter(lambda x: x > 10)` |
| `sample()`    | Returns a **random subset** of an RDD. | `rdd.sample(False, 0.1)` |
| `sortByKey()` | Sorts an RDD by key values. | `rdd.sortByKey()` |
| `groupByKey()` | Groups values with the **same key**. | `rdd.groupByKey()` |
| `reduceByKey()` | Merges values of the same key using a function. | `rdd.reduceByKey(lambda a, b: a + b)` |
| `aggregateByKey()` | Allows different aggregation for local/global computations. | `rdd.aggregateByKey(0, lambda a, b: a + b, lambda a, b: a + b)` |
| `mapPartitionsWithIndex()` | Applies a function to each partition with partition index. | `rdd.mapPartitionsWithIndex(func)` |
| `repartitionAndSortWithinPartitions()` | Repartitions and sorts within partitions. | `rdd.repartitionAndSortWithinPartitions(4)` |

## ⚡ Actions  

| Action         | Description | Example Usage |
|---------------|-------------|--------------|
| `reduce()`    | Aggregates elements using a function. | `rdd.reduce(lambda a, b: a + b)` |
| `take(n)`     | Returns the first `n` elements. | `rdd.take(5)` |
| `collect()`   | Brings all elements to the **driver**. | `rdd.collect()` |
| `takeSample(withReplacement, num, seed)` | Returns a **random sample** of elements. | `rdd.takeSample(False, 3, 42)` |
| `count()`     | Returns the **number of elements**. | `rdd.count()` |
| `takeOrdered(n)` | Returns first `n` elements in **sorted order**. | `rdd.takeOrdered(5)` |
| `countByKey()` | Returns a **key-wise count**. | `rdd.countByKey()` |
| `first()`     | Returns the **first element**. | `rdd.first()` |
| `foreach(f)`  | Applies a function to each element **without returning anything**. | `rdd.foreach(print)` |
| `saveAsTextFile(path)` | Saves RDD contents as a **text file**. | `rdd.saveAsTextFile("output")` |
| `saveAsSequenceFile(path)` | Saves RDD as **Hadoop Sequence File**. | `rdd.saveAsSequenceFile("output")` |
| `saveAsObjectFile(path)` | Stores RDD elements as **serialized Java objects**. | `rdd.saveAsObjectFile("output")` |


## 📌 Key Differences  
- **Transformations** are **lazy** and only build an **execution plan**.  
- **Actions** **trigger** execution and return results.  

🔗 **Learn More**: [RDD Programming Guide](https://spark.apache.org/docs/latest/)  


## Repo-Chatbot  

### 🚀 **Try our AI chatbot for Spark-related questions!**  

👉 [Click here to Chat](https://repo-chatbot.streamlit.app/)  


