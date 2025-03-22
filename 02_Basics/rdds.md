# Resilient Distributed Dataset (RDD) in Apache Spark
Resilient Distributed Dataset (RDD) is the fundamental data structure in Apache Spark. It is an immutable, distributed collection of objects that enables parallel processing across a cluster. RDDs provide fault tolerance and scalability, making them a powerful abstraction for big data processing. [More](https://spark.apache.org/docs/latest/rdd-programming-guide.html)

![Image](https://github.com/user-attachments/assets/375a23f7-c4d5-405f-8f1d-25bb51ec677c)
Image Source: [NashTech](https://blog.nashtechglobal.com/things-to-know-about-spark-rdd/)

### Characteristics of RDD
&nbsp;&nbsp;&nbsp;**1. Immutable**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Once created, RDDs cannot be changed; transformations create new RDDs.

&nbsp;&nbsp;&nbsp;**2. Distributed**
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data is partitioned across multiple nodes in the cluster.

&nbsp;&nbsp;&nbsp;**3. Fault-Tolerant**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Uses lineage (DAG) to recompute lost partitions.

&nbsp;&nbsp;&nbsp;**4. Lazy Evaluation**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Transformations are not executed until an action is called.

&nbsp;&nbsp;&nbsp;**5. Partitioned**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Data is split into logical partitions for parallel processing.

### RDD Operations

RDD supports two types of operations:

### 1. Transformations (Lazy Execution)

Transformations create new RDDs from existing ones and are only executed when an action is called. [know more](https://github.com/Sharathpd14/Apache-Spark/blob/main/02_Basics/transformations_and_actions)

### 2. Actions (Trigger Execution)

Actions trigger execution of transformations and return values. [know more](https://github.com/Sharathpd14/Apache-Spark/blob/main/02_Basics/transformations_and_actions)

## Creating RDDs

### 1. From an Existing Collection (Parallelized)
```python
from pyspark.sql import SparkSession

# Create a SparkSession
spark = SparkSession.builder \
    .appName("StringRDD") \
    .getOrCreate()

# String to create an RDD from
my_string = "This is a sample string to create an RDD."

# Create an RDD from the string
string_rdd = spark.sparkContext.parallelize([my_string])

# Print the RDD contents
print(string_rdd.collect())
```

### 2. From External Datasets (Files, HDFS, S3, etc.)
```python
# Read the text file as an RDD
rdd = spark.sparkContext.textFile("sample.txt")

# Print the RDD contents
print(rdd.collect())
```

### 3. From Transformations
```python
# Create a new RDD by multiplying each element of the existing RDD by 2
rdd_second = rdd.map(lambda x: x * 2)

# Print the RDD contents
print(rdd2.collect())
```

&nbsp;

RDDs provide a low-level abstraction for distributed computing in Spark. While RDDs offer fine-grained control over operations, higher-level APIs like DataFrames and Datasets are often preferred for better optimization and ease of use. However, understanding RDDs is essential for advanced Spark applications and optimizations.


## Repo-Chatbot  

### 🚀 **Try our AI chatbot for Spark-related questions!**  

👉 [Click here to Chat](https://repo-chatbot.streamlit.app/)  

