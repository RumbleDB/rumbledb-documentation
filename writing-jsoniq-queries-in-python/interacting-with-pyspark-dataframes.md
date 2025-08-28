# Interacting with pyspark DataFrames

RumbleDB can work out of the box with pyspark DataFrames, both as input and (when the output has a schema) as output.

## Using Pyspark DataFrames with JSONiq

The power users can also interface our library with pyspark DataFrames. JSONiq sequences of items can have billions of items, and our library supports this out of the box: it can also run on clusters on AWS Elastic MapReduce for example. But your laptop is just fine, too: it will spread the computations on your cores. You can bind a DataFrame to a JSONiq variable. JSONiq will recognize this DataFrame as a sequence of object items.

Creating a data frame also similar to Spark (but using the rumble object).

```renpy
data = [("Alice", 30), ("Bob", 25), ("Charlie", 35)];
columns = ["Name", "Age"];
df = spark.createDataFrame(data, columns);
```

This is how to bind a JSONiq variable to a dataframe. You can bind as many variables as you want.

```python
rumble.bind('$a', df);
```

This is how to run a query. This is similar to spark.sql(). Since variable $a was bound to a DataFrame, it is automatically declared as an external variable and can be used in the query. In JSONiq, it is logically a sequence of objects.

```python
res = rumble.jsoniq('$a.Name');
```

You can also, instead of the bind() call, pass the pyspark DataFrame directly in jsoniq() with an extra named parameter:

```python
res = rumble.jsoniq('$a.Name', a=df);
```

There are several ways to collect the outputs, depending on the user needs but also on the query supplied. The following method returns a list containing one or several of "DataFrame", "RDD", "PUL", "Local".

If DataFrame is in the list, df() can be invoked.

If RDD is in the list, rdd() can be invoked.

If Local is the list, items() or json() can be invokved, as well as the local iterator API.

```python
modes = res.availableOutputs();
for mode in modes:
    print(mode)
```

## Manipulating DataFrames with SQL and JSONiq

If the output of the JSONiq query is structured (i.e., RumbleDB was able to detect a schema), then we can extract a regular data frame that can be further processed with spark.sql() or rumble.jsoniq().

```python
df = res.df();
df.show();
```

We are continuously working on the detection of schemas and RumbleDB will get better at it with them. JSONiq is a very powerful language and can also produce heterogeneous output "by design". Then you need to use rdd() instead of df(), or to collect the list of JSON values (see further down). Remember that availableOutputs() tells you what is at your disposal.

A DataFrame output by JSONiq can be reused as input to a Spark SQL query.

(Remember that rumble is a wrapper around a SparkSession object, so you can use rumble.sql() just like spark.sql())\


```python
df.createTempView("myview")
df2 = spark.sql("SELECT * FROM myview").toDF("name");
df2.show();
```

A DataFrame output by Spark SQL can be reused as input to a JSONiq query.

```python
rumble.bind('$b', df2);
seq2 = rumble.jsoniq("for $i in 1 to 5 return $b");
df3 = seq2.df();
df3.show();
```

And a DataFrame output by JSONiq can be reused as input to another JSONiq query.

```python
rumble.bind('$b', df3);
seq3 = rumble.jsoniq("$b[position() lt 3]");
df4 = seq3.df();
df4.show();
```

