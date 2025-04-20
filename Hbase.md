# HBase setup and command reference guide


---

## 🚀 **Starting Hadoop and HBase**

### ✅ **Start Hadoop**
```bash
cd C:\hadoopsetup\hadoop-3.2.4\sbin
start-all.cmd
jps
# Should display: NameNode, DataNode, ResourceManager, NodeManager, etc.
```

### ✅ **Start HBase**
```bash
cd C:\hbasesetup\hbase-1.4.9\bin
start-hbase.cmd
jps
# Should show: HMaster (the HBase Master daemon)
```

---

## 🐚 **Access HBase Shell**
```bash
hbase shell
```

---

## 📘 **HBase Shell Commands with Comments**

| Command | Description |
|--------|-------------|
| `status` | Shows cluster summary |
| `status 'simple'` | Shows simplified status |
| `status 'detailed'` | Shows detailed info about region servers |
| `version` | Displays HBase version |
| `table_help` | Shows table-related commands |
| `whoami` | Displays the HBase shell user |

---

### 🛠️ **Table Operations**
```bash
create 'sample_table', 'colfam'         # Creates table with column family
list                                     # Lists all tables
describe 'sample_table'                 # Schema of the table
disable 'sample_table'                  # Disables table
enable 'sample_table'                   # Enables table
drop 'sample_table'                     # Deletes a disabled table
```

---

### 🧱 **Altering Tables**
```bash
create 'test_table','colfam1'
describe 'test_table'

alter 'test_table',{NAME => 'colfam2'}                   # Add column family
alter 'test_table',{NAME => 'colfam2',METHOD => 'delete'}# Delete column family

alter 'test_table',{NAME => 'colfam1', VERSIONS => 2}    # Set number of versions
alter 'test_table', READONLY => true                     # Make table read-only
alter 'test_table', MAX_FILESIZE => '1234566'            # Max HFile size
alter_status 'test_table'                                # Last alter operation status
```

---

### 🧭 **Namespace Operations**
```bash
create_namespace 'ns1'               # Create a new namespace
create 'ns1:rt1','cf'                # Create table under namespace
list 'ns1:rt1'                       # List specific table
describe_namespace 'ns1'            # Metadata about namespace
list_namespace                      # Lists all namespaces
```

---

### 📝 **Data Operations**
```bash
# Insert Data
create 'f', 'f1'
put 'f', 1, 'f1:name', 'Richa'
put 'f', 1, 'f1:city', 'XYZ'
put 'f', 1, 'f1:id', '10'
put 'f', 2, 'f1:address', 'abcdefg'

# Read Data
scan 'f'                                 # Scans all rows
get 'f','1'                              # Gets full row with key '1'
get 'f','2'                              # Gets full row with key '2'
get 'f','1', {COLUMN => 'f1:name'}       # Get specific column
get 'f','1', {COLUMN => ['f1:name','f1:city']} # Multiple columns
```

---

### 🧹 **Deleting Data**
```bash
delete 'mytable', 1, 'colfam1:Creator'   # Delete a specific cell
deleteall 'mytable', '1'                 # Delete all cells of a row
```

---

### 🔢 **Counting Rows**
```bash
count 'mytable'      # Returns number of rows in 'mytable'
```

---
