# Structured command list with suitable comments  **connecting to Hadoop and Hive**

---

## 🚀 HADOOP & HIVE SETUP & USAGE (STRUCTURED)

---

### 🔧 **Step 1: Start Hadoop Services**

```powershell
# Navigate to Hadoop sbin directory
cd C:\hadoopsetup\hadoop-3.2.4\sbin

# Start HDFS (NameNode & DataNode)
.\start-dfs.cmd

# Start YARN (ResourceManager & NodeManager)
.\start-yarn.cmd

# Check all Java processes (should see NameNode, DataNode, etc.)
jps
```

---

### 🌐 **Step 2: Start Derby Network Server**

```powershell
# In a separate PowerShell window, run the Derby server
StartNetworkServer -h 0.0.0.0

# Keep this window minimized
```

---

### 🐝 **Step 3: Start Hive**

```powershell
# Navigate to Hive bin directory
cd C:\hive\apache-hive-3.1.3-bin\bin

# if not created schema then
 hive --service  schematool -dbType derby -initSchema 

# Start Hive shell
hive
```

---



## 💾 HIVE COMMANDS

---

### 📁 **Section 1: Database Management**

```sql
-- 1. Create a new database
create database if not exists abc;

-- 2. Create with comment
create database if not exists abc1 comment "This is a sample db";

-- 3. Show all databases
show databases;

-- 4. Use LIKE with wildcard
show databases like 'a*';

-- 5. Describe a database
describe database abc1;

-- 6. Drop a database
drop database abc1;
```

---

### 📊 **Section 2: Table Creation & Data Loading**

```sql
-- 7. Create a table
create table sample_hive (
  id int, 
  f_name string,
  l_name string,
  city string
)
row format delimited
fields terminated by '|'
stored as textfile;

-- 8. Load data from local file
load data local inpath "C:\Users\91991\OneDrive\Desktop\hive_sample.txt" into table sample_hive;

-- 9. View data
select * from sample_hive;

-- 10. Drop a table
drop table sample_hive;

-- 11. Show all tables
show tables;

-- 12. Switch to another database
use abc;

-- 13. Describe a table
describe sample_hive;
```

---

### 🔧 **Section 3: DDL (Data Definition Language) Commands**

```sql
-- 1.1 Create new database
create database data1;

-- 1.2 Use the database
use data1;

-- 1.3 Create a table
create table sample1 (id int);

-- 1.4 Load data into the table
load data local inpath "C:\Users\91991\OneDrive\Desktop\s1.txt" into table sample1;

-- 1.5 Select data
select * from sample1;

-- 2. Alter table name
alter table sample1 rename to sample_table1;

-- 2.1 Add new column
alter table sample_table1 add columns (name char(10));

-- 3. Describe the altered table
describe sample_table1;

-- 4. Show commands
show tables;
show databases;

-- 5. Truncate the table (delete all rows)
truncate table sample_table1;

-- 6. Drop the table
drop table sample_table1;
```

---

### 🧩 **Section 4: Complex Data Types (ARRAY, MAP, STRUCT)**

#### ✅ ARRAY

```sql
-- Create ARRAY table
create table temperature (
  sno int,
  place string,
  mytemp array<double>
)
row format delimited
fields terminated by '\t'
collection items terminated by ',';

-- Load array data
load data local inpath "C:\Users\91991\OneDrive\Desktop\array.txt" into table temperature;

-- Query array
select * from temperature;
select place, mytemp[0] from temperature;
select place, mytemp[3] from temperature;
```

#### ✅ MAP

```sql
-- Create MAP table
create table tab (
  city string,
  gender string,
  my_map map<int,int>
)
row format delimited
fields terminated by '\t'
collection items terminated by ','
map keys terminated by ':';

-- Load map data
load data local inpath "C:\Users\91991\OneDrive\Desktop\map.txt" into table tab;

-- Query map
select * from tab;
select my_map[2022] from tab where city='xy';
select my_map[2022] from tab where city='xy' and gender='male';
```

#### ✅ STRUCT

```sql
-- Create STRUCT table
create table struct (
  name string,
  city string,
  info struct<place:string,temp:float>
)
row format delimited
fields terminated by '\t'
collection items terminated by ',';

-- Load struct data
load data local inpath "C:\Users\91991\OneDrive\Desktop\struct.txt" into table struct;

-- Query struct
select * from struct;
select info.place from struct;
select info.temp from struct;
```

---

### 👁️ **Section 5: VIEWS**

```sql
-- Create a table
create table vexample (
  id int, 
  name string, 
  sal int, 
  dept string
)
row format delimited
fields terminated by '\t';

-- Load data
load data local inpath "C:\Users\91991\OneDrive\Desktop\view.txt" into table vexample;

-- View data
select * from vexample;

-- Create a view hiding salary column
create view viewone as select id, name, dept from vexample;

-- Query the view
select * from viewone;

-- Drop the view
drop view viewone;
```

---


---

## ✅ **PARTITIONING in Hive**

Partitioning divides data based on column values to allow faster access.

### ➤ Create a Table
```sql
CREATE TABLE states (
  state STRING,
  city STRING,
  pin BIGINT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

### ➤ Load Data into Table
```sql
LOAD DATA LOCAL INPATH 'C:/Users/Dell/Desktop/LPU 23241/INT 312/PPTS/hive/partitioning.txt' 
INTO TABLE states;
```

### ➤ Check Data
```sql
DESCRIBE states;
SELECT * FROM states;
```

### ➤ Create Partitioned Table
```sql
CREATE TABLE part (
  city STRING,
  pin BIGINT
)
PARTITIONED BY (state STRING);
```

### ➤ Enable Dynamic Partitioning
```sql
SET hive.exec.dynamic.partition.mode=nonstrict;
```

### ➤ Insert Data into Partitioned Table
```sql
INSERT OVERWRITE TABLE part PARTITION(state)
SELECT city, pin, state FROM states;
```

### ➤ Check in HDFS (in terminal)
```bash
hadoop fs -ls /user/hive/warehouse/part
hadoop fs -ls "/user/hive/warehouse/part/state=Haryana"
hadoop fs -cat "/user/hive/warehouse/part/state=Haryana/000000_0"
```

---

## ✅ **BUCKETING in Hive**

Bucketing distributes data into files (buckets) based on hash function.

### ➤ Create Table for Bucketing
```sql
CREATE TABLE bucket (
  id INT,
  name STRING,
  salary FLOAT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

### ➤ Load Data
```sql
LOAD DATA LOCAL INPATH "C:/Users/91991/OneDrive/Desktop/bucket.txt" 
INTO TABLE bucket;
```

### ➤ Enable Bucketing
```sql
SET hive.enforce.bucketing=true;
```

### ➤ Create Bucketed Table
```sql
CREATE TABLE small_bucket (
  id INT,
  name STRING,
  salary FLOAT
)
CLUSTERED BY (id) INTO 3 BUCKETS
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

### ➤ Insert Data
```sql
INSERT OVERWRITE TABLE small_bucket 
SELECT * FROM bucket;
```

### ➤ Check in HDFS
```bash
hadoop fs -ls /user/hive/warehouse/small_bucket
hadoop fs -cat /user/hive/warehouse/small_bucket/000000_0
hadoop fs -cat /user/hive/warehouse/small_bucket/000001_0
hadoop fs -cat /user/hive/warehouse/small_bucket/000002_0
```

---

## ✅ **JOINS in Hive**

Before starting:
```sql
SET hive.auto.convert.join=false;
```

### ➤ Create Students & Marks Tables
```sql
CREATE TABLE students (
  id INT,
  name STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';

CREATE TABLE marks (
  student_id INT,
  mark INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

### ➤ Load Data
```sql
LOAD DATA LOCAL INPATH "C:/Users/91991/OneDrive/Desktop/Students.csv" 
INTO TABLE students;

LOAD DATA LOCAL INPATH "C:/Users/91991/OneDrive/Desktop/Marks.csv" 
INTO TABLE marks;
```

### ➤ Perform Joins

#### 🔸 Inner Join
```sql
SELECT s.id, s.name, m.mark
FROM students s
JOIN marks m ON s.id = m.student_id;
```

#### 🔸 Left Join
```sql
SELECT s.id, s.name, m.mark
FROM students s
LEFT OUTER JOIN marks m ON s.id = m.student_id;
```

#### 🔸 Right Join
```sql
SELECT s.id, s.name, m.mark
FROM students s
RIGHT OUTER JOIN marks m ON s.id = m.student_id;
```

#### 🔸 Full Outer Join
```sql
SELECT s.id, s.name, m.mark
FROM students s
FULL OUTER JOIN marks m ON s.id = m.student_id;
```

---

## ✅ **OPERATORS in Hive**

### ➤ Arithmetic Operators
| Operator | Description |
|---------|-------------|
| A + B | Addition |
| A - B | Subtraction |
| A * B | Multiplication |
| A / B | Division |
| A % B | Modulo |
| A \| B | Bitwise OR |
| A & B | Bitwise AND |
| A ^ B | Bitwise XOR |
| ~A | Bitwise NOT |

Example:
```sql
SELECT cgpa + 1.0 FROM students;
```

---

### ➤ Relational Operators
| Operator | Description |
|----------|-------------|
| A = B | Equal |
| A <> B / A != B | Not Equal |
| A < B | Less than |
| A > B | Greater than |
| A <= B | Less than or equal |
| A >= B | Greater than or equal |
| A IS NULL | Check null |
| A IS NOT NULL | Check not null |

Example:
```sql
SELECT regno FROM students WHERE cgpa > 5.0;
```

---

## ✅ **FUNCTIONS in Hive**

### ➤ Aggregate Functions
```sql
SELECT COUNT(*) FROM students;
SELECT MAX(cgpa) FROM students;
SELECT AVG(cgpa) FROM students;
SELECT SUM(cgpa) FROM students;
```

---

### ➤ String Functions
| Function | Description |
|----------|-------------|
| LENGTH(str) | String length |
| REVERSE(str) | Reverse string |
| CONCAT(str1, str2, ...) | Concatenate |
| SUBSTR(str, start [, length]) | Substring |
| UPPER(str) | To uppercase |
| LOWER(str) | To lowercase |
| TRIM(str) | Trim spaces |
| LTRIM(str) | Left trim |
| RTRIM(str) | Right trim |

Example:
```sql
SELECT regno, UPPER(fname) FROM students;
```

---

## ✅ **GROUP BY, HAVING, ORDER BY, SORT BY**

### ➤ GROUP BY
```sql
SELECT section, AVG(cgpa) 
FROM student 
GROUP BY section;
```

### ➤ HAVING
```sql
SELECT section, AVG(cgpa) 
FROM student 
GROUP BY section 
HAVING AVG(cgpa) > 5.0;
```

### ➤ ORDER BY
```sql
SELECT regno, name, cgpa 
FROM student 
ORDER BY cgpa; -- ascending

SELECT regno, name, cgpa 
FROM student 
ORDER BY cgpa DESC; -- descending
```

### ➤ SORT BY
```sql
SELECT * 
FROM student 
SORT BY cgpa DESC;
```

---
