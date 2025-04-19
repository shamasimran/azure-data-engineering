# 🧠 Azure Databricks Essentials

This folder contains essential commands and examples for working with Azure Databricks, including Spark basics, dataframes, Delta Lake, and notebooks.

---

## 📦 Environment Setup

```python
# Check Spark version
spark.version
spark.conf.set("key", "value")   # Set Spark config

# Create Spark session (for local use, Databricks provides it automatically)
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("example").getOrCreate()
```

---

## 🧾 Reading Data

```python
df = spark.read.csv("path", header=True, inferSchema=True)
df1 = spark.read.json("path")
df2 = spark.read.parquet("path")
df3 = spark.read.format("delta").load("path")

df.show()
diplay(df1)
```

## 💾 Writing Data

```python
df.write.csv("path")
df.write.parquet("path")
df.write.format("delta").save("path")
df.write.saveAsTable("table_name")
```

---

## 🧱 DataFrame Operations

```python
df.show() # Show results
df.printSchema() # Show schema
df.describe().show()

df.select("col1", "col2").show() # Select columns
df.filter(df.age > 30).show() # Filter rows
df.withColumn("new_col", df.col1 * 2) # Add new column
df.drop("col_name") # Drop column
df.groupBy("category").count().show() # Group by
df.groupBy("col").agg({"col2": "sum"}).show()
```

---

## 🧊 Delta Lake

### Write to Delta

```python
df.write.format("delta").save("/mnt/delta/my_table")
```

### Read from Delta

```python
df_delta = spark.read.format("delta").load("/mnt/delta/my_table")
```

### Create Delta Table (Managed)

```python
df.write.format("delta").saveAsTable("my_managed_table")
```

### Time Travel

```python
df_old = spark.read.format("delta").option("versionAsOf", 1).load("/mnt/delta/my_table")
```

---

## 🛠️ Notebooks & Magic Commands

```python
# %run another notebook
%run ./utils/helper_functions

# %fs - DBFS file system commands
%fs ls /mnt/data

# %sql - SQL queries
%sql
SELECT * FROM my_table LIMIT 10;
```

---

## 🔒 Mounting Azure Data Lake (Example)

```python
configs = {
  "fs.azure.account.key.<your-storage-account>.dfs.core.windows.net": "<your-access-key>"
}

dbutils.fs.mount(
  source = "abfss://<container>@<your-storage-account>.dfs.core.windows.net/",
  mount_point = "/mnt/data",
  extra_configs = configs
)
```

---

## 🧭 Utilities (dbutils)

```python
dbutils.help()
dbutils.help('fs')
dbutils.fs.ls("/mnt/")
dbutils.notebook.run("notebook-path", 60, {"param": "value"})
dbutils.widgets.get("param")

```

---

## 🧹 Cleaning Up

```python
# Drop a table
spark.sql("DROP TABLE IF EXISTS my_managed_table")

# Unmount a path
dbutils.fs.unmount("/mnt/data")
```

---

> 💡 More samples and tutorials can be added as notebooks or scripts inside this folder.
