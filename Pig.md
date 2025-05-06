
---

# 🐷 Apache Pig Setup & Usage Guide (Windows + PowerShell)

Apache Pig is a high-level platform for creating MapReduce programs used with Hadoop. It uses a language called **Pig Latin**.

📌 **Note:**

* Use **PowerShell**, not CMD.
* Always use **forward slashes (`/`)**, not backslashes (`\`), in file paths to avoid errors.

---

## 📥 Installation

### 🔗 Download Link

👉 [https://dlcdn.apache.org/pig/latest/](https://dlcdn.apache.org/pig/latest/)

---

### 🛠️ Step-by-Step Installation

#### ✅ Step 1: Create Setup Folder

* Create a folder in `C:/` drive (e.g., `C:/pigsetup`)
* Extract the downloaded `pig-0.17.0` ZIP inside it

#### ✅ Step 2: Edit Environment Variables

* Open "Edit the system environment variables" > "Environment Variables"
* Under **System variables**, click `New`:

```
Variable name: PIG_HOME  
Variable value: C:\pigsetup\pig-0.17.0\pig-0.17.0
```

* Then edit the `Path` variable:

  * Add: `%PIG_HOME%\bin`

🔁 This ensures your terminal (PowerShell) can run Pig commands from any location.

#### ✅ Step 3: Edit pig.cmd

Go to: `C:\pigsetup\pig-0.17.0\pig-0.17.0\bin`
Open `pig.cmd` and find this line:

```bat
set HADOOP_BIN_PATH=%HADOOP_HOME%\bin
```

🔄 Replace it with:

```bat
set HADOOP_BIN_PATH=%HADOOP_HOME%\libexec
```

✔️ This fixes a compatibility issue between Pig and newer Hadoop versions.

---

## ✅ Verify Installation

Open PowerShell:

```powershell
pig -version
```

➡️ If installed correctly, it will display the version info.

---

## 🧪 Running Pig

### 🖥️ Local Mode

```powershell
pig -x local
```

This starts the **Grunt shell** (Pig interactive shell) in local mode — for small file testing on your system (not HDFS).

### ☁️ HDFS Mode (requires Hadoop running)

1. Start Hadoop services:

```powershell
start-all.cmd
```

2. In a new terminal:

```powershell
pig
```

3. Run HDFS commands like:

```pig
fs -ls /
```

---

# 🔄 Basic Pig Commands with Explanation

## 📂 LOAD & STORE

### 🧾 Load a Text File

```pig
data = LOAD 'C:/path/to/test.txt' USING PigStorage(',') AS (n1:int, n2:int, n3:int, n4:int);
```

🔹 **What it does:** Loads a CSV file using comma `,` as delimiter
🔹 **Why:** Converts raw text into a Pig relation with schema for processing

### 🖨️ Dump Data

```pig
DUMP data;
```

🔹 Prints loaded data to console (similar to `SELECT *` in SQL)

### 💾 Store Output

```pig
STORE data INTO 'C:/pigsetup/output/' USING PigStorage('|');
```

🔹 **What it does:** Writes the output data into files using pipe `|` delimiter
🔹 **Why:** To export processed data

---

## 🔍 DESCRIBE, FILTER, DISTINCT, FOREACH
### data1
```data1
1,John,CSE,85
2,Alice,MATH,92
3,Bob,CSE,67
4,Eve,PHY,78
5,Mike,MATH,56
6,Lisa,CSE,88
7,Tom,ECE,45
8,Sam,CSE,90
9,Nina,PHY,60
10,Alex,ECE,82
```

### load data
```pig
data1 = LOAD 'student_data.txt' USING PigStorage(',') AS (id:int, name:chararray, dept:chararray, marks:int);
```

### 📜 Describe schema

```pig
DESCRIBE data1;
```

### 🔍 Filter students with marks > 80

```pig
high_achievers = FILTER data1 BY marks > 80;
```

🔹 Why: To select top-performing students

### 🧑‍🎓 Filter by department

```pig
cs_students = FILTER data1 BY dept == 'Computer Science';
```

### 📘 Distinct departments

```pig
unique_departments = DISTINCT (FOREACH data1 GENERATE dept);
```

🔹 Why: To list unique department names only

### 💡 Remove duplicate records

```pig
unique_students = DISTINCT data1;
```

---

## 🔢 Sorting

```pig
sorted_data = ORDER data1 BY marks ASC;
```

🔹 **Why:** Sorts students by marks (ascending)

---

## 🔍 Group & CoGroup

### 📁 Group by name

```pig
grouped_sales = GROUP sales BY name;
```

🔹 Like SQL's `GROUP BY`

### 🔗 CoGroup for joins

```pig
cogrouped = COGROUP students BY id, marks BY id;
```

🔹 Like SQL JOIN + GROUP BY

---

## 🎯 Union, Split, Limit

```pig
SPLIT students INTO passed IF score >= 50, failed IF score < 50;
```

🔹 Split one dataset into multiple based on condition

```pig
top_passed = LIMIT passed 3;
```

🔹 Select top 3 passed students

```pig
sample_combined = UNION top_passed, top_failed;
```

🔹 Merge passed and failed samples together

---

## 🧠 Eval Functions (Aggregation)

```pig
grouped = GROUP students ALL;
```

### 📊 AVG, SUM, MIN, MAX, COUNT

```pig
avg_result = FOREACH grouped GENERATE AVG(students.mark);
sum_result = FOREACH grouped GENERATE SUM(students.mark);
min_max_result = FOREACH grouped GENERATE MIN(students.mark), MAX(students.mark);
count_result = FOREACH grouped GENERATE COUNT(students);
```

---

## 🧩 String Functions

```pig
fullnames = FOREACH names GENERATE CONCAT(first, '-', last);
```

🔹 Join first and last names with `-`

```pig
flagged = FOREACH names GENERATE first, (first IN ('John', 'Alice') ? 'Yes' : 'No') AS is_present;
```

🔹 Check if the name is in the list

---

## ✂️ Tokenize & Size

```pig
tokens = FOREACH lines GENERATE TOKENIZE(line);
```

🔹 Breaks sentence into words

```pig
word_count = FOREACH tokens GENERATE SIZE($0);
```

🔹 Count number of words in each line

---

## 📌 Conclusion

This setup and command reference gives you everything needed to get started with **Apache Pig** on **Windows using PowerShell**.

You can now:

* Process local or HDFS data
* Write SQL-like queries in Pig Latin
* Aggregate, transform, and export results

---


