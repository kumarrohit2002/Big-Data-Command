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
cd C:\hbasesetup\hbase-1.4.9-bin\hbase-1.4.9\bin
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
put 'f', 1, 'f1:name', 'Rohit'
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

# **Apache HBase Java API Reference & Shell Equivalent Operations**

## **Overview:**
HBase shell operations have equivalent Java APIs. This document provides a detailed understanding of how to interact with HBase using Java, covering common operations such as **table creation**, **data insertion**, **data retrieval**, **deletion**, and **filtering**.

---

## 🔧 **General Notes**

| Operation             | HBase Shell                           | Java Equivalent Class/Method      |
|-----------------------|----------------------------------------|-----------------------------------|
| Create Table          | `create`                               | `HTableDescriptor`, `HColumnDescriptor`, `Admin.createTable()` |
| Insert Data           | `put`                                  | `Put` class, `addColumn()`       |
| Retrieve Data         | `get`                                  | `Get`, `Result`, `getValue()`    |
| Delete Data           | `delete`                               | `Delete`, `addColumn()`          |
| Filter Data           | `scan` with filters                    | `Scan`, `RowFilter`, `Filter`    |

---

## 🛠️ **PROGRAM 1: Create an HBase Table Using Java API**

**Shell Equivalent:**
```bash
create 'my_table', 'colfam1', 'colfam2'
```

**Steps in Java:**
1. Specify table name and column families
2. Connect to HBase
3. Use `HBaseAdmin` to create table

**Java Code:**
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.HColumnDescriptor;
import org.apache.hadoop.hbase.HTableDescriptor;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;

import java.io.IOException;

public class createTable {
    public static void main(String[] args) throws IOException {
        Configuration conf = HBaseConfiguration.create();
        Connection connection = ConnectionFactory.createConnection(conf);
        Admin admin = connection.getAdmin();

        HTableDescriptor tableName = new HTableDescriptor(TableName.valueOf("my_table"));
        tableName.addFamily(new HColumnDescriptor("colfam1"));
        tableName.addFamily(new HColumnDescriptor("colfam2"));

        if (!admin.tableExists(tableName.getTableName())) {
            System.out.print("Creating Table...");
            admin.createTable(tableName);
            System.out.println("Done");
        }
    }
}
```

---

## 📝 **PROGRAM 2: Insert Column for a Single Row ID**

**Shell Equivalent:**
```bash
put 'my_table', '1', 'colfam1:id', '32410'
put 'my_table', '1', 'colfam1:name', 'Richa'
put 'my_table', '1', 'colfam1:country', 'India'
```

**Java Code:**
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;
import org.apache.hadoop.hbase.util.Bytes;

import java.io.IOException;

public class insertData {
    public static void main(String[] args) throws IOException {
        Configuration conf = HBaseConfiguration.create();
        Connection connection = ConnectionFactory.createConnection(conf);

        Table table = connection.getTable(TableName.valueOf("my_table"));

        Put put = new Put(Bytes.toBytes("1"));
        put.addColumn(Bytes.toBytes("colfam1"), Bytes.toBytes("id"), Bytes.toBytes("32410"));
        put.addColumn(Bytes.toBytes("colfam1"), Bytes.toBytes("name"), Bytes.toBytes("Richa"));
        put.addColumn(Bytes.toBytes("colfam1"), Bytes.toBytes("country"), Bytes.toBytes("India"));

        table.put(put);
        System.out.print("Data inserted into the table");
    }
}
```

---

## 📥 **PROGRAM 3: Retrieve Data for a Single Row ID Using `Get()`**

**Shell Equivalent:**
```bash
get 'my_table', '1'
```

**Java Code:**
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;
import org.apache.hadoop.hbase.util.Bytes;

import java.io.IOException;

public class getData {
    public static void main(String[] args) throws IOException {
        Configuration conf = HBaseConfiguration.create();
        Connection connection = ConnectionFactory.createConnection(conf);

        Table table = connection.getTable(TableName.valueOf("my_table"));
        Get g = new Get(Bytes.toBytes("1"));
        Result result = table.get(g);

        String id = Bytes.toString(result.getValue(Bytes.toBytes("colfam1"), Bytes.toBytes("id")));
        String name = Bytes.toString(result.getValue(Bytes.toBytes("colfam1"), Bytes.toBytes("name")));
        String country = Bytes.toString(result.getValue(Bytes.toBytes("colfam1"), Bytes.toBytes("country")));

        System.out.println("id: " + id + " name: " + name + " country: " + country);
    }
}
```

---

## ❌ **PROGRAM 4: Delete Operation in HBase Table**

**Shell Equivalent:**
```bash
delete 'my_table', '1', 'colfam1:Channel'
```

**Java Code:**
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;
import org.apache.hadoop.hbase.util.Bytes;

import java.io.IOException;

public class deleteData {
    public static void main(String[] args) throws IOException {
        Configuration conf = HBaseConfiguration.create();
        Connection connection = ConnectionFactory.createConnection(conf);

        Table table = connection.getTable(TableName.valueOf("my_table"));

        Delete delete = new Delete(Bytes.toBytes("1"));
        delete.addColumn(Bytes.toBytes("colfam1"), Bytes.toBytes("Channel"));

        table.delete(delete);
        table.close();
    }
}
```

---

## 🔍 **PROGRAM 5: Filtering Data in Apache HBase Using Java API**

**Shell Equivalent:**
```bash
scan 'my_table', {FILTER => "RowFilter(=, 'binary:1')"}
```

**Java Code:**
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;
import org.apache.hadoop.hbase.filter.*;
import org.apache.hadoop.hbase.util.Bytes;

import java.io.IOException;

public class filterData {
    public static void main(String[] args) throws IOException {
        Configuration conf = HBaseConfiguration.create();
        Connection connection = ConnectionFactory.createConnection(conf);

        Table table = connection.getTable(TableName.valueOf("my_table"));

        Filter filter = new RowFilter(CompareFilter.CompareOp.EQUAL,
                new BinaryComparator(Bytes.toBytes("1")));

        Scan userScan = new Scan();
        userScan.setFilter(filter);

        ResultScanner userScanResult = table.getScanner(userScan);
        for (Result res : userScanResult) {
            System.out.println(res);
        }
        userScanResult.close();
    }
}
```

---

## 📌 **Key Takeaways:**

- All HBase shell commands can be translated into Java API calls.
- Use **`Bytes`** class to convert data to byte arrays.
- **`Connection`**, **`Admin`**, **`Table`**, **`Put`**, **`Get`**, **`Delete`**, **`Scan`**, and **`Filter`** are central classes in Java HBase client API.
- Java API gives programmatic control for production applications and integration.

---
