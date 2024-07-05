# **PySpark Style Guide**

[IN PROGRESS]

PySpark provides you with access to Python language bindings to the Apache Spark engine.

Here we outline the best practices you should follow when writing PySpark code.

Python linters already exist. This document focusses on PySpark,  not on general Python code formatting.


## **Imports**

**Never** do a ```from library import *``` statement. This makes tracking down the source of any methods/functions used incredibly difficult, thus making any refactoring, debugging, or understanding tough and time-consuming.

Instead, be explicit


```python
from pyspark.sql import functions as F
```

Now, anytime a function from ```pyspark.sql``` is used, we will clearly know.

For example ```F.col("newCol")```

Put **all imports at the top of your script/notebook**.


## **Functions**

To encourage writing **modular code**, and to adhere to the **DRY principle** where possible, functions are preferred to endless inline code.

This provides the added benefits of reusability, testability, and readability. Not to mention debugging benefits.

All functions should, at the very least, have a minimal doc string explaining what the function does.

### **Type & Return Hints**

All functions should include Type & Return hints.

```python
from pyspark.sql import functions as F
from pyspark.sql import DataFrame

def with_ingestion_time(df: DataFrame, new_col_name: str = "ingestionTime") -> DataFrame:
    """Add timestamp col"""
    return df.withColumn(new_col_name, F.current_timestamp())

def with_explode_column(df: DataFrame, column_to_explode: str) -> DataFrame:
    """Explode an array column"""
    return df.withColumn(column_to_explode, F.explode(column_to_explode))
```

### **Chaining**

When applying functions to a DataFrame, use the ```transform``` method.

```python
refinedDf = (
        df
        .transform(with_ingestion_time, new_col_name="ingestionTime")
        .transform(with_explode_column, column_to_explode="userDetails")
    )
```
This provides supreme readability and clarity for that is being applied to a DataFrame.

Name new DataFrames with a representative variable name. **Never** do this:

```python

df1 = with_ingestion_time(df)
df2 = with_explode_column(df1)
df3...
df4...
dfN...
```

### **User Defined Functions**

These should be avoided where possible as they aren't accessible by the Spark Compiler.