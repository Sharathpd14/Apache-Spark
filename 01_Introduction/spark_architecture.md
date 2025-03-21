# Apache Spark Architecture

## Spark Architecture and Components

Apache Spark's architecture is like a well-coordinated team, where each component plays a crucial role. These components work together seamlessly, ensuring efficient, distributed data processing while handling large-scale computations with speed and reliability.

### **1. Driver Program**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The **driver program** is the entry point of a Spark application. It plays a crucial role in managing the execution of Spark jobs.

#### ✨ **Responsibilities**

&nbsp;&nbsp;&nbsp;✅ **Converting transformations into a Directed Acyclic Graph (DAG):**  
  The driver translates user-defined operations into a DAG, which represents the execution flow.

&nbsp;&nbsp;&nbsp;✅ **Allocating tasks to executors:**  
  It schedules and distributes tasks across worker nodes to execute computations efficiently.

&nbsp;&nbsp;&nbsp;✅ **Monitoring job execution:**  
  The driver tracks the progress of tasks, handles failures, and retries tasks if necessary.

&nbsp;&nbsp;&nbsp;✅ **Collecting results:**  
  Once computations are complete, the driver gathers the processed data from executors.

📌 **How It Works**
  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Initialization**  
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸 User submits a Spark application (`spark-submit`).  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸 Driver requests resources from the **Cluster Manager**.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Job Execution**  
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
     🔸 Converts transformations into a **Directed Acyclic Graph (DAG)**.  
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸 **Schedules tasks** and assigns them to executors.  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  🔸 Executors process tasks in **parallel** and return results.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3. Monitoring & Completion**  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Driver monitors execution and **retries failed tasks**.  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Collects final results and **terminates the application**. 

&nbsp;

#### **2. Cluster Manager**  

Spark can run on different **cluster managers** to distribute and manage resources across nodes. These include:  

- **Standalone Mode** – Default cluster manager provided by Spark.  
- **Apache Mesos** – A general-purpose cluster manager.  
- **Hadoop YARN** – Widely used in Hadoop ecosystems.  
- **Kubernetes** – A container-based cluster manager for Spark applications.  

The **Cluster Manager** allocates resources (executors) to the **Driver Program**, ensuring smooth execution.

&nbsp;

&nbsp;&nbsp;**3. Executors**  

Executors are the **worker nodes** in a Spark cluster responsible for executing assigned tasks.  

#### ✨ **Key Responsibilities**  
&nbsp;&nbsp;✅ **Run tasks** and return results to the driver.  
&nbsp;&nbsp;✅ **Store intermediate data** in memory or disk for fault tolerance.  
&nbsp;&nbsp;✅ **Communicate with the driver** and shuffle data between nodes for distributed processing.  

Executors **run in parallel** across multiple nodes, making Spark a powerful distributed system.

&nbsp;

&nbsp;&nbsp;**4. Tasks**  

Tasks are the **smallest units of execution** in Spark, representing operations applied to partitions of data.  

📌 **How Tasks Work?**  
- A Spark **job** is divided into **stages** based on transformations.  
- Each **stage** consists of multiple **tasks**.
-  Each task process 1 **partition**.  
- Tasks run **independently and in parallel** across executors.  

Efficient task execution ensures high performance and scalability. 🚀

&nbsp;


&nbsp;&nbsp;**5. DAG Scheduler**  

The **DAG Scheduler** translates user-defined computations into a **Directed Acyclic Graph (DAG)** and optimizes execution.

📌 **How DAG Works**


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. High-Level Transformations**  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Spark operations like `map()`, `filter()`, and `groupBy()` create a **DAG of RDDs**.  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  These transformations are **lazy**, meaning execution starts only when an action (e.g., `collect()`, `count()`) is triggered.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Series of Stages**  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  The DAG is **divided into stages** based on **shuffle dependencies**.  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Each stage represents a sequence of transformations that **do not require data shuffling**.  

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3. Parallel Task Execution**  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Each **stage consists of multiple tasks** that operate on data partitions.  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 🔸  Tasks within a stage run **in parallel** across executors.  
  
 

A well-structured DAG ensures that Spark optimally executes tasks while minimizing recomputation.

