# 🔄 Wide vs. Narrow Transformations in Apache Spark  

In Apache Spark, transformations are **lazy**, meaning they are not executed immediately. Instead, Spark records them in a *lineage graph* and optimizes execution. Transformations are categorized into two types: **narrow** and **wide** transformations.  

But why does this matter? 🤔  

Understanding the difference helps in writing **efficient** Spark applications by **reducing data shuffling**, improving **performance**, and **minimizing memory overhead**. Let’s break it down! 🚀  

---

## 🟢 Narrow Transformations – The Fast Lane 🏎️  

A **narrow transformation** means that each partition of the output depends on **a single partition** of the input. This allows Spark to **process data in parallel without shuffling**, making it **faster and more efficient**.  

### ✅ Characteristics:  
✔️ No data movement between partitions.  
✔️ Faster execution due to no shuffle operation.  
✔️ More memory efficient.  

### ✨ Examples of Narrow Transformations:  

| Transformation | Description | Example |
|---------------|-------------|---------|
| `map()` | Applies a function to each row. | `df.rdd.map(lambda x: x * 2)` |
| `filter()` | Selects rows based on a condition. | `df.filter(df.age > 18)` |
| `flatMap()` | Similar to `map()` but flattens the output. | `df.rdd.flatMap(lambda x: x.split(" "))` |
| `mapPartitions()` | Processes each partition independently. | `df.rdd.mapPartitions(lambda x: [sum(x)])` |

🛠️ **Example Code:**  
```python
rdd = spark.sparkContext.parallelize([1, 2, 3, 4])
mapped_rdd = rdd.map(lambda x: x * 10)  # Narrow transformation
filtered_rdd = mapped_rdd.filter(lambda x: x > 10)  # Another narrow transformation
filtered_rdd.collect()  # [20, 30, 40]
```

#### 🔍 What Happened?

- `map()` applied a function to each element (**no shuffle**).
- `filter()` kept only numbers greater than 10 (**still no shuffle**).
- Everything executed within the same partition! 🚀


## 🔴 Wide Transformations – The Slow Lane 🛑

A wide transformation requires data shuffling across partitions, which slows down execution because Spark needs to redistribute the data across the cluster.

### ❌ Characteristics:
- ❗ Requires data exchange between partitions (**network shuffle**).
- ❗ Higher memory and disk usage.
- ❗ Can slow down performance significantly.

### ⚡ Examples of Wide Transformations:
| Transformation  | Description  | Example  |
|---|---|---|
| `groupByKey()`  | Groups values by key (**expensive shuffle**).  | `rdd.groupByKey()`  |
| `reduceByKey()`  | Aggregates values, but optimizes shuffle.  | `rdd.reduceByKey(lambda a, b: a + b)`  |
| `sortByKey()`  | Sorts data across partitions (**full shuffle**).  | `rdd.sortByKey()`  |
| `join()`  | Joins two datasets (**shuffle required**).  | `df1.join(df2, "id")`  |

### 🛠️ Example Code:
```python
rdd = spark.sparkContext.parallelize([(1, "a"), (2, "b"), (1, "c"), (2, "d")])
grouped_rdd = rdd.groupByKey()  # Wide transformation - shuffle happens here!
grouped_rdd.collect()
```

#### 🔍 What Happened?
- `groupByKey()` forced Spark to redistribute data across nodes.
- Data was shuffled across partitions (**expensive operation**).

🔥 **Alternative?** Use `reduceByKey()` instead of `groupByKey()` for better performance!

![Image](https://github.com/user-attachments/assets/fa456a70-8ce3-41c7-9003-a787d106e4d2)

Image Source [Data Engineer Things](https://blog.det.life/i-spent-8-hours-learning-the-details-of-the-apache-spark-scheduling-process-26816f805658)

## 🚀 How to Optimize Spark Transformations?

- Prefer **narrow transformations** whenever possible.
- Avoid `groupByKey()` and use `reduceByKey()` instead.
- Use **caching (`persist()`)** to store intermediate results when needed.
- **Partition your data wisely** to minimize shuffle operations.

### 🚀 Avoid `groupByKey()` and Use `reduceByKey()` Instead

#### 🔥 Why is `groupByKey()` Inefficient?

At first glance, `groupByKey()` might seem like the obvious choice to group data based on keys. However, it's highly inefficient and can cause serious performance issues in Spark applications. Here’s why:

- **Causes a Full Shuffle** – All key-value pairs are shuffled across the network, leading to high data movement and increased execution time.
- **Memory Overhead** – Since `groupByKey()` collects all values for a key before processing, it uses excessive memory and can lead to **OutOfMemory (OOM) errors**.
- **Unnecessary Data Transfer** – It transfers all values for a key across the network instead of aggregating them beforehand.

#### ❌ Example of `groupByKey()` (Bad Practice)
```python
rdd = spark.sparkContext.parallelize([("A", 1), ("B", 2), ("A", 3), ("B", 4)])

# Inefficient approach using groupByKey()
grouped_rdd = rdd.groupByKey()

# Collecting results
for key, values in grouped_rdd.collect():
    print(key, list(values))
```

#### 🔍 What’s Wrong Here?

- The **entire dataset is shuffled** across the network, causing high latency.
- Spark **stores all values in memory**, leading to high memory consumption.

### ✅ The Better Alternative: `reduceByKey()`

Instead of `groupByKey()`, use `reduceByKey()`, which performs aggregation locally before shuffling. This reduces data transfer, making it much faster and more memory-efficient.

#### ⚡ How `reduceByKey()` Solves the Problem?

✔️ Performs **partial aggregation** before data is shuffled.  
✔️ **Reduces memory overhead** by computing values incrementally.  
✔️ **Minimizes network traffic**, making it significantly faster.

#### ✅ Example of `reduceByKey()` (Optimized Approach)
```python
rdd = spark.sparkContext.parallelize([("A", 1), ("B", 2), ("A", 3), ("B", 4)])

# Efficient approach using reduceByKey()
optimized_rdd = rdd.reduceByKey(lambda a, b: a + b)

# Collecting results
optimized_rdd.collect()  # Output: [('A', 4), ('B', 6)]
```

#### 🚀 Why is `reduceByKey()` Better?

- The **partial sum of values is computed locally**, reducing the data sent across the network.
- **Less data movement** = **faster execution** 🚀.
- **Lower memory usage**, reducing the risk of crashes.



## 🎯 Final Thoughts

| Feature  | Narrow Transformations  | Wide Transformations  |
|---|---|---|
| **Data Movement**  | No shuffle  | Requires shuffle  |
| **Execution Speed**  | Faster 🚀  | Slower 🐌  |
| **Parallel Processing**  | High  | Limited  |
| **Memory & CPU Usage**  | Low  | High  |

## 🔗 Learn More: [Wide and Narrow Dependencies in Apache Spark](https://www.geeksforgeeks.org/wide-and-narrow-dependencies-in-apache-spark/)
