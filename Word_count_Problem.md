
---

# 📝 Hadoop WordCount Using Eclipse (Java 1.8 + Hadoop 3.2.4)

This project demonstrates how to run a basic Hadoop **MapReduce** program to count words in a text file using **Eclipse IDE** and **Java 1.8**. Perfect for beginners learning Hadoop.

---

## ✅ Prerequisites

* [Java JDK 1.8](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
* [Hadoop 3.2.4](https://hadoop.apache.org/releases.html)
* [Eclipse IDE for Java Developers](https://www.eclipse.org/downloads/)
* Hadoop configured on your system with HDFS, YARN, and environment variables properly set

---

## 🔧 Project Setup in Eclipse

### Step-by-Step Instructions

1. Download & install the **latest Eclipse IDE**
2. Launch Eclipse → Create a new Java project:

   ```
   File → New → Java Project → Project Name: WordCountProject
   ```

   ✅ Ensure you select **JRE version 1.8**
3. Right-click on `src` → New → Class → Name it `WordCount` → Finish
4. Delete the generated code in the class
5. Paste the **WordCount.java** source code (given below)

---

## 💻 WordCount.java – Full Code

```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  // Mapper class
  public static class TokenizerMapper extends Mapper<Object, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one); // Emit <word, 1>
      }
    }
  }

  // Reducer class
  public static class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get(); // Sum up values for each word
      }
      result.set(sum);
      context.write(key, result); // Emit <word, total count>
    }
  }

  // Main method: sets up the job
  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));  // Input path
    FileOutputFormat.setOutputPath(job, new Path(args[1])); // Output path
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
```

---

## 🧠 Explanation of the Code

### 🔹 Imports

We import classes from `java.io`, `java.util`, and Hadoop's `mapreduce` and `io` packages.

### 🔹 Mapper Class: `TokenizerMapper`

* **Input Key**: Offset of the line in the file (ignored)
* **Input Value**: Line of text (as `Text`)
* **Output Key**: Each word found (as `Text`)
* **Output Value**: Count `1` (as `IntWritable`)

**Logic**:

* Breaks the input line into words using a `StringTokenizer`
* For each word, it emits a key-value pair: `<word, 1>`

---

### 🔹 Reducer Class: `IntSumReducer`

* **Input Key**: A word (from mapper output)
* **Input Value**: Iterable of counts (1s)
* **Output Key/Value**: `<word, totalCount>`

**Logic**:

* For each word, it loops through the values (1s) and calculates the total frequency
* Emits `<word, count>`

---

### 🔹 Main Method

* Sets up the job configuration
* Specifies the mapper, reducer, and combiner classes
* Defines input and output paths (from `args[0]`, `args[1]`)
* Launches the job using `job.waitForCompletion(true)`

---

## 📚 Adding Hadoop Libraries in Eclipse

1. Right-click the project → `Build Path → Configure Build Path`
2. Go to `Libraries` tab → Click **Add External JARs**

### Add:

* From:
  `C:\hadoopsetup\hadoop-3.2.4\share\hadoop\common`
  ➕ Add **4 core jars**

* From:
  `C:\hadoopsetup\hadoop-3.2.4\share\hadoop\mapreduce`
  ➕ Add **15 mapreduce jars**

✅ Apply and close to resolve all errors

---

## 📦 Export to JAR File

1. Right-click the project → `Export → Java → JAR file` → Next
2. Select the source folder and classpath
3. Provide JAR file name (e.g., `abc.jar`)
4. Define the main class: `WordCount`
5. Click **Finish**

---

## 🧪 Run the WordCount Program on Hadoop

1. Create an input file:

```bash
echo "hello world hello hadoop" > input.txt
```

2. Start Hadoop services:

```bash
start-dfs.sh
start-yarn.sh
```

3. Upload input file to HDFS:

```bash
hdfs dfs -mkdir /input
hdfs dfs -put input.txt /input
```

4. Run the program:

```bash
hadoop jar /path/to/abc.jar /input /output
```

5. View the output:

```bash
hdfs dfs -cat /output/part-r-00000
```

---

## ✅ Sample Output

```
hadoop    1
hello     2
world     1
```

---

## 📌 Cleanup Tip

If `/output` already exists in HDFS, delete it:

```bash
hdfs dfs -rm -r /output
```

---

## 📂 Project Folder Structure

```
WordCountProject/
├── src/
│   └── WordCount.java
├── lib/
│   └── [Hadoop external jars]
└── abc.jar
```

---


