# Bucketing in Apache Spark: A Smarter Way to Optimize Performance

When dealing with large datasets in Apache Spark, partitioning is a great way to optimize performance. However, excessive partitioning can lead to too many small files, which negatively impacts efficiency. This is where **bucketing** comes in!

Bucketing helps group data into a fixed number of evenly distributed **buckets** based on a specific column, improving shuffle efficiency and query performance.


## What is Bucketing in Spark?

Bucketing is a technique that divides data into a **fixed number of buckets** based on a **hashing function** applied to a column. Unlike partitioning, where files are stored in separate folders, **bucketing stores data in evenly distributed files inside a single directory**.

This approach helps optimize query performance by **reducing shuffle operations** in joins and aggregations.
![Image](https://github.com/user-attachments/assets/eea45fd1-5fb5-4571-aa0c-c6f770969287)
Image Source [Clairvoyant Blog](https://blog.clairvoyantsoft.com/bucketing-in-spark-878d2e02140f)

## How is Bucketing Different from Partitioning?

| Feature          | Partitioning                              | Bucketing                                      |
|-----------------|-----------------------------------------|----------------------------------------------|
| **How it works** | Divides data into physical directories | Divides data into a fixed number of files  |
| **Storage structure** | Creates separate folders for each partition value | Stores all buckets in a single folder |
| **Column selection** | Based on high-cardinality columns (like year) | Based on frequently joined columns (like user_id) |
| **Shuffle optimization** | Reduces data scan | Reduces shuffle during joins |

🚀 **Best Use Case**: When performing **frequent joins on large datasets**, bucketing significantly improves performance!


## How Bucketing Works in Spark
To implement bucketing, we use the .bucketBy() function while writing data.
#### Example: Writing a Bucketed Table in Spark

```python
from pyspark.sql import SparkSession

# Initialize Spark Session
spark = SparkSession.builder.appName("BucketingExample").getOrCreate()

# Sample DataFrame
data = [
    (1, "John", "USA"),
    (2, "Alice", "India"),
    (3, "Bob", "Canada"),
    (4, "Charlie", "USA"),
    (5, "David", "India"),
    (6, "Eve", "Canada")
]

columns = ["id", "name", "country"]
df = spark.createDataFrame(data, columns)

# Write data into bucketed tables (5 buckets based on 'id')
df.write.bucketBy(5, "id").sortBy("id").format("parquet").saveAsTable("bucketed_table")
```

#### How It Works?

- The id column is used to distribute data into 5 buckets using hashing.
- The sortBy("id") ensures data within each bucket is sorted, improving performance for range queries.
- Joins on the id column are much faster because Spark already knows where to find specific data.


## Performance Benefits of Bucketing

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. **Faster Joins:** Since data is pre-shuffled and sorted, Spark skips unnecessary shuffle operations during join queries.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. **Reduced Shuffle Overhead:** Bucketing ensures related data is stored in the same bucket, reducing data movement across nodes.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. **Optimized Aggregations:** GroupBy and Aggregation operations are much faster when data is pre-bucketed.

#### Example: Optimized Join with Bucketing

```python
df1 = spark.table("bucketed_table")
df2 = spark.table("another_bucketed_table")

# Spark knows both tables are bucketed, so it skips shuffle
result = df1.join(df2, "id")
```
👉 Since both tables are bucketed on id, Spark avoids expensive shuffling, making joins faster!

&nbsp;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bucketing is an efficient optimization technique in Apache Spark that helps improve join performance, reduce shuffle overhead, and optimize query execution. Unlike partitioning, bucketing distributes data into a fixed number of buckets, making operations faster and more efficient.

## Key Takeaways:
✅ Bucketing is ideal for frequently joined columns.  
✅ It helps minimize shuffle and boost query speed.  
✅ Works best when used with large datasets in distributed environments.  

By leveraging bucketing and partitioning together, you can significantly improve Spark’s performance and make your big data workflows run much faster! 🚀
