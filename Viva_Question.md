**Viva Questions and Answers for ETP Exam: Big Data and Hadoop Ecosystem**

---

| Feature              | **Hadoop**                              | **Hive**                                    | **HBase**                                     | **Pig**                                                |
| -------------------- | --------------------------------------- | ------------------------------------------- | --------------------------------------------- | ------------------------------------------------------ |
| **Type**             | Framework                               | Data warehouse infrastructure               | NoSQL database (Column-oriented)              | Scripting platform                                     |
| **Main Component**   | HDFS (storage) + MapReduce (processing) | Built on top of Hadoop                      | Built on top of HDFS                          | Built on top of Hadoop                                 |
| **Data Type**        | Batch processing of large data sets     | Structured data (tables, similar to SQL)    | Semi-structured / Unstructured (key-value)    | Semi-structured                                        |
| **Language Used**    | Java                                    | HiveQL (similar to SQL)                     | Java APIs                                     | Pig Latin (data flow language)                         |
| **Use Case**         | Large-scale data processing             | Data summarization, ad-hoc querying         | Real-time read/write access to large datasets | ETL (Extract, Transform, Load) operations              |
| **Query Language**   | None (programmatic Java-based)          | HiveQL                                      | No traditional query language, uses Java API  | Pig Latin                                              |
| **Execution Engine** | MapReduce                               | Converts HiveQL to MapReduce                | Internally uses HDFS and HBase engine         | Converts Pig Latin to MapReduce                        |
| **Latency**          | High (batch processing)                 | High (not for real-time queries)            | Low (near real-time access)                   | High (batch-based, like MapReduce)                     |
| **Schema**           | Schema on read                          | Schema on read                              | Schema-less (column families defined)         | Schema on read                                         |
| **Ideal For**        | Batch processing and storage            | Data analysis with familiar SQL-like syntax | Random, real-time access to big data          | Complex data transformations without writing Java code |


### **1. Introduction to Hadoop and Big Data**

**Q1. What is Big Data? Mention its types with examples.**
**A:** Big Data refers to large volumes of data—structured, semi-structured, or unstructured—that cannot be processed using traditional systems. Types include:

* Structured (e.g., relational databases)
* Semi-structured (e.g., XML, JSON)
* Unstructured (e.g., images, videos, emails)

**Q2. What are the key V's of Big Data?**
**A:** The key V's are:

* Volume: Quantity of data
* Velocity: Speed of data generation
* Variety: Different data types
* Veracity: Uncertainty in data
* Value: Insights gained from data

**Q3. What is Hadoop? Why is it used?**
**A:** Hadoop is an open-source Big Data framework that allows distributed storage and parallel processing of large datasets using simple programming models.

**Q4. What are the components of Hadoop?**
**A:** Hadoop has two core components:

* HDFS (Hadoop Distributed File System)
* MapReduce (Processing Framework)
  Additional: YARN, Common utilities

**Q5. How is Hadoop installed?**
**A:** Hadoop can be installed on a single-node or multi-node cluster. Installation involves setting JAVA\_HOME, configuring XML files (`core-site.xml`, `hdfs-site.xml`), formatting the NameNode, and starting daemons.

---

### **2. Hadoop Architecture**

**Q1. Explain the architecture of Hadoop.**
**A:** Hadoop follows a Master-Slave architecture:

* HDFS: NameNode (Master), DataNodes (Slaves)
* MapReduce: JobTracker (Master), TaskTrackers (Slaves)

**Q2. What is HDFS?**
**A:** HDFS is a distributed file system that stores data across multiple machines, ensuring fault tolerance and high throughput.

**Q3. Define NameNode and DataNode.**
**A:**

* NameNode: Manages file system metadata.
* DataNode: Stores actual data blocks.

**Q4. What is MapReduce?**
**A:** MapReduce is a programming model for processing large datasets. It involves:

* Map: Processing input data into key-value pairs
* Reduce: Aggregating results

**Q5. What are JobTracker and TaskTracker?**
**A:**

* JobTracker: Manages and schedules jobs
* TaskTracker: Executes tasks assigned by JobTracker

---

### **3. MapReduce and YARN**

**Q1. What is the MapReduce programming model?**
**A:** A model for processing large data sets with two main tasks:

* Mapper: Splits data into key-value pairs
* Reducer: Aggregates/interprets key-value pairs

**Q2. Limitations of MapReduce v1?**
**A:**

* Limited scalability
* Single point of failure (JobTracker)
* No support for non-MapReduce applications

**Q3. Basic Java classes in MapReduce?**
**A:**

* Mapper class (extends Mapper<>)
* Reducer class (extends Reducer<>)
* Driver class (uses Job class to configure and submit the job)

**Q4. What is YARN?**
**A:** YARN (Yet Another Resource Negotiator) is Hadoop's resource management layer. It separates resource management and job scheduling.

**Q5. Compare MR1 with MR2/YARN.**
**A:**

* MR1: JobTracker handles all
* MR2/YARN: ResourceManager and ApplicationMaster handle scheduling and execution

---

### **4. Apache Hive**

**Q1. What is Hive?**
**A:** Hive is a data warehouse tool on top of Hadoop that allows querying data using SQL-like language called HiveQL.

**Q2. Hive installation steps?**
**A:**

* Install Hadoop
* Download Hive binary
* Set HIVE\_HOME and PATH
* Configure `hive-site.xml`

**Q3. Hive data types?**
**A:**

* Primitive: INT, STRING, BOOLEAN
* Complex: ARRAY, MAP, STRUCT

**Q4. Bucketing vs Partitioning in Hive?**
**A:**

* Partitioning divides data based on column values
* Bucketing divides data into fixed number of files (buckets) based on hash function

**Q5. Common HiveQL operations?**
**A:** CREATE TABLE, SELECT, LOAD DATA, INSERT, DELETE, ALTER, DROP

---

### **5. Apache HBase**

**Q1. What is HBase?**
**A:** HBase is a distributed, column-oriented NoSQL database built on top of HDFS for real-time read/write access.

**Q2. HBase data model?**
**A:**

* Table > Row > Column Family > Column Qualifier > Cell

**Q3. Architecture of HBase?**
**A:** Includes HMaster, RegionServers, ZooKeeper for coordination

**Q4. HBase Java API usage?**
**A:** Involves creating configuration, establishing connection, using `Table` object to put/get/delete data

**Q5. HBase filtering examples?**
**A:**

* PrefixFilter: Matches row keys with specific prefix
* SingleColumnValueFilter: Filters rows based on column value

**Q6. What is TTL in HBase?**
**A:** Time-To-Live defines lifespan of cell data. Expired data is automatically deleted.

---

### **6. Apache Pig**

**Q1. What is Pig?**
**A:** Pig is a high-level platform for creating MapReduce programs using a language called Pig Latin.

**Q2. What is Pig Latin?**
**A:** A data flow language used in Pig for processing and analyzing large data sets.

**Q3. Load and Store operations?**
**A:**

* LOAD: Reads data into Pig
* STORE: Writes data to HDFS

**Q4. Diagnostic operators in Pig?**
**A:** Operators like DUMP, DESCRIBE, EXPLAIN help in debugging and understanding data

**Q5. Grouping and Joining?**
**A:**

* GROUP: Groups data based on key
* JOIN: Joins two or more datasets

**Q6. Filtering and Sorting in Pig?**
**A:**

* FILTER: Filters records based on conditions
* ORDER: Sorts data

**Q7. Built-in functions in Pig?**
**A:** COUNT, AVG, SUM, MAX, MIN, CONCAT, TOKENIZE

**Q8. Handling NULL values?**
**A:** Pig skips null values during computation; can use `IS NOT NULL` in FILTER

**Q9. What is Schema-less loading?**
**A:** Pig allows loading data without specifying schema, treating all fields as bytearray

---