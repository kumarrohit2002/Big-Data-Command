



# 🚀 Big Data Command Reference & Setup Guide

This repository serves as a comprehensive reference and step-by-step setup guide for Big Data technologies including **Hadoop**, **Hive**, and **HBase**.

Whether you're just setting up your system for the first time or revisiting command-line utilities, this repository provides you with a handy and complete collection of commands, troubleshooting steps, and execution examples for key components of the Hadoop ecosystem.

---

## 📁 Detailed Notes & Commands

| Tool   | Commands & Notes                                                                 |
|--------|-----------------------------------------------------------------------------------|
| 🐘 Hadoop | [HADOOP.md](https://github.com/kumarrohit2002/Big-Data-Command/blob/main/HADOOP.md) |
| 🐝 Hive   | [HIVE.md](https://github.com/kumarrohit2002/Big-Data-Command/blob/main/HIVE.md)     |
| 📦 HBase  | [Hbase.md](https://github.com/kumarrohit2002/Big-Data-Command/blob/main/Hbase.md)   |

---

## ⚙️ Hadoop Installation & Execution Guide (PowerShell)

> 💡 Open PowerShell as Administrator before executing any commands.

### ✅ Check if everything is installed correctly

```bash
java -version
Hadoop
Hadoop -version
```

### 🔧 Format NameNode (if needed)

```bash
hdfs namenode -format
```

### ▶️ Start Hadoop Services

```bash
cd C:\hadoopsetup\hadoop-3.2.4\sbin
.\start-dfs.cmd
./start-yarn.cmd
```

> This opens new CMD windows. To verify background services:

```bash
jps
```

### 🔗 Important Web Interfaces

- [NameNode UI](http://localhost:9870/dfshealth.html)
- [DataNode UI](http://localhost:9864/datanode.html)
- [YARN Cluster UI](http://localhost:8088/cluster)

---

## 🧰 Useful Commands: Quick Glance

Below are some examples of what you’ll find in the detailed docs:

- Create & manage directories/files in HDFS
- Copy files to/from local ↔️ HDFS
- Disk usage, status, and file information
- Ownership, permissions, replication
- Word Count problem using MapReduce JAR
- Append, Merge, Checksum, Truncate operations
- File & directory testing
- Trash expunge and cleanup
- And much more…

For all 30+ commands and examples, see the [HADOOP.md](https://github.com/kumarrohit2002/Big-Data-Command/blob/main/HADOOP.md) file.

---

## 🔥 Sample Execution: Word Count Program

1. Prepare a text file locally
2. Run:

```bash
hadoop fs -mkdir /dir1
hadoop fs -put "C:\Users\91991\OneDrive\Desktop\wordcount.txt" /dir1
hadoop jar "C:\hadoopsetup\hadoop-3.2.4\share\hadoop\mapreduce\hadoop-mapreduce-examples-3.2.4.jar" wordcount /dir1 /outputdir1
hadoop fs -cat /outputdir1/part-r-00000
```

---

## 🧾 Author

**Rohit Kumar**  
Big Data Enthusiast | MERN Stack Developer  
GitHub: [@kumarrohit2002](https://github.com/kumarrohit2002)

---

> ⭐ Star this repo if you find it helpful. Contributions & feedback welcome!
```
