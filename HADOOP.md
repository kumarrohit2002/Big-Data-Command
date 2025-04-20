
---

# 💻 Hadoop Setup & Command Reference

## ✅ Initial Setup Checklist

> Open **PowerShell as Administrator** and run the following:

```bash
1) java -version
2) hadoop
3) hadoop -version
```

### 🧹 In case of issues, format the namenode:

```bash
hdfs namenode -format
```

---

## 🚀 Starting Hadoop Services

```bash
cd C:\hadoopsetup\hadoop-3.2.4\sbin
.\start-dfs.cmd
```

> This opens 2 more CMD windows. Then, run:

```bash
./start-yarn.cmd
```

To check running background daemons:

```bash
jps
```

---

## 🔗 Useful Web Interfaces

- **NameNode UI**: [http://localhost:9870/dfshealth.html](http://localhost:9870/dfshealth.html)  
- **DataNode UI**: [http://localhost:9864/datanode.html](http://localhost:9864/datanode.html)  
- **YARN Resource Manager**: [http://localhost:8088/cluster](http://localhost:8088/cluster)

---

## 🛠 Routine Commands (To Start Hadoop)

```bash
1) cd C:\hadoopsetup\hadoop-3.2.4\sbin
2) .\start-dfs.cmd
3) ./start-yarn.cmd
4) jps
```

---

## 📂 HDFS Basic File Operations

| Action | Command |
|--------|---------|
| Create a directory | `hadoop fs -mkdir /richa` |
| List contents | `hadoop fs -ls /` |
| Create empty file | `hadoop fs -touchz /richa/demo.txt` |
| Display file | `hadoop fs -ls /richa` |
| Copy local → HDFS | `hadoop fs -put "C:\path\sample.txt" /abc` |
| View file contents | `hadoop fs -cat /abc/sample.txt` |
| Copy HDFS → local | `hadoop fs -get /abc/demo.txt "C:\path\"` |
| Copy within HDFS | `hadoop fs -cp /abc/sample.txt /richa/` |
| Move within HDFS | `hadoop fs -mv /richa/demo1.txt /abc/` |

---

## 💾 Disk Usage

| Description | Command |
|------------|---------|
| Size of each directory | `hadoop fs -du /` |
| Size of each file in dir | `hadoop fs -du /abc` |
| Total size of dir | `hadoop fs -dus /abc` |

---

## ✅ Test Commands

| Check | Command |
|-------|---------|
| Is directory? | `hadoop fs -test -d /abc ; echo $?` |
| Exists? | `hadoop fs -test -e /abc ; echo $?` |
| Is file? | `hadoop fs -test -f /abc/sample.txt` |
| Is empty? | `hadoop fs -test -z /abc/sample.txt` |

---

## 📤 Moving Files

| Task | Command |
|------|---------|
| Move local → HDFS | `hadoop fs -moveFromLocal "C:\path\testing.txt" /abc/` |
| Move HDFS → local | `hadoop fs -get ...` + `hadoop fs -rm ...` |

---

## 🔀 Merge & Append

- **Merge multiple HDFS files → local**:
  ```bash
  hadoop fs -getmerge -nl /abc/ "C:/path/mergeResult.txt"
  ```

- **Append local files → HDFS**:
  ```bash
  hadoop fs -appendToFile "C:\path\t1.txt" "C:\path\t2.txt" /richa/appendHDFS.txt
  ```

---

## 🛡️ File Integrity, FS Status & Metadata

| Action | Command |
|--------|---------|
| Check checksum | `hadoop fs -checksum /abc/sample.txt` |
| Check HDFS health | `hadoop fsck /` |
| Count files/dirs | `hadoop fs -count /richa` |

---

## 🗑️ Delete Commands

| Task | Command |
|------|---------|
| Delete file | `hadoop fs -rm /richa/test.txt` |
| Delete empty dir | `hadoop fs -rmdir /sachin` |
| Delete non-empty dir | `hadoop fs -rm -r /abc/Desktop` |

---

## 👥 Ownership & Permissions

| Task | Command |
|------|---------|
| Change group | `hadoop fs -chgrp RichaNigam /richa/sample.txt` |
| File size in bytes | `hadoop fs -stat %b /abc/sample.txt` |
| Group name | `hadoop fs -stat %g /abc/sample.txt` |
| Replication factor | `hadoop fs -stat %r /abc/sample.txt` |
| File owner | `hadoop fs -stat %u /abc/sample.txt` |
| Last modified time | `hadoop fs -stat %y /abc/sample.txt` |

---

## 📚 Help & Usage

| Info | Command |
|------|---------|
| Command usage | `hadoop fs -usage mkdir` |
| Full help | `hadoop fs -help cat` |

---

## 📖 Read File Chunks

| Task | Command |
|------|---------|
| First 1KB | `hadoop fs -head /abc/head_and_tail.txt` |
| Last 1KB | `hadoop fs -tail /abc/head_and_tail.txt` |

---

## 🧹 Trash & Permissions

| Action | Command |
|--------|---------|
| Expunge trash | `hadoop fs -expunge` |
| Change owner | `hadoop fs -chown ri /abc/file.txt` |
| Change owner & group | `hadoop fs -chown ri:richa /richa/sample.txt` |
| Change permissions | `hadoop fs -chmod 777 /abc/file.txt` |

---

## 🔁 Replication & Truncate

| Task | Command |
|------|---------|
| Set replication | `hadoop fs -setrep -w 3 /abc/sample.txt` |
| Truncate file | `hadoop fs -truncate -w 10 /abc/sample.txt` |

---

## ⚙️ File Attributes

| Action | Command |
|--------|---------|
| Set extended attribute | `hadoop fs -setfattr -n 'user.ap' -v "this is dummy file" /abc/sample.txt` |
| Get attributes | `hadoop fs -getfattr -d /abc/sample.txt` |

---

## 📈 Word Count Program (MapReduce)

> Requirements:  
> ✅ A local input file  
> ✅ JAR file: `hadoop-mapreduce-examples-3.2.4.jar`

```bash
# Step 1: Create input dir
hadoop fs -mkdir /dir1

# Step 2: Copy input file
hadoop fs -put "C:\Users\91991\OneDrive\Desktop\wordcount.txt" /dir1

# Step 3: Run job
hadoop jar "C:\hadoopsetup\hadoop-3.2.4\share\hadoop\mapreduce\hadoop-mapreduce-examples-3.2.4.jar" wordcount /dir1 /outputdir1

# Step 4: View output
hadoop fs -ls /
hadoop fs -ls /outputdir1
hadoop fs -cat /outputdir1/part-r-00000
```

---

## 🧠 Pro Tip

> To terminate any running command:
```
Ctrl + C
```

---

