# Writing JSONiq queries in Python

You can use RumbleDB from within Python programmes by running

```bash
pip install jsoniq
```

_Important note_: since the jsoniq package depends on pyspark 4, Java 17 or Java 21 is a requirement. If another version of Java is installed, the execution of a Python program attempting to create a RumbleSession will lead to an error message on stderr that contains explanations.

### High-level information on the library

A RumbleSession is a wrapper around a SparkSession that additionally makes sure the RumbleDB environment is in scope.

JSONiq queries are invoked with rumble.jsoniq() in a way similar to the way Spark SQL queries are invoked with spark.sql().

JSONiq variables can be bound to lists of JSON values (str, int, float, True, False, None, dict, list) or to Pyspark DataFrames. A JSONiq query can use as many variables as needed (for example, it can join between different collections).

It will later also be possible to read tables registered in the Hive metastore, similar to spark.sql(). Alternatively, the JSONiq query can also read many files of many different formats from many places (local drive, HTTP, S3, HDFS, ...) directly with simple [builtin function calls](Input.md) such as json-lines(), text-file(), parquet-file(), csv-file(), etc.

The resulting sequence of items can be retrieved as a list of JSON values, as a Pyspark DataFrame, or, for advanced users, as an RDD or with a streaming iteration over the items using the [RumbleDB Item API](https://github.com/RumbleDB/rumble/blob/master/src/main/java/org/rumbledb/api/Item.java).

It is also possible to write the sequence of items to the local disk, to HDFS, to S3, etc in a way similar to how DataFrames are written back by Pyspark.

The design goal is that it is possible to chain DataFrames between JSONiq and Spark SQL queries seamlessly. For example, JSONiq can be used to clean up very messy data and turn it into a clean DataFrame, which can then be processed with Spark SQL, spark.ml, etc.

Any feedback or error reports are very welcome.

## Your first program

The syntax to start a session is similar to that of Spark. A RumbleSession is a SparkSession that additionally knows about RumbleDB. All attributes and methods of SparkSession are also available on RumbleSession.

Even though RumbleDB uses Spark internally, it can be used without any knowledge of Spark.

Executing a query is done with rumble.jsoniq() like so. A query returns a sequence\
of items, here the sequence with just the integer item 2.

```python
from jsoniq import RumbleSession

rumble = RumbleSession.builder.getOrCreate();

items = rumble.jsoniq('1+1')
python_list = items.json()
print(python_list)
```

### Type mapping

Any expression in JSONiq returns a sequence of items. Any variable in JSONiq is bound to a sequence of items. Items can be objects, arrays, or atomic values (strings, integers, booleans, nulls, dates, binary, durations, doubles, decimal numbers, etc). A sequence of items can be a sequence of just one item, but it can also be empty, or it can be as large as to contain millions, billions or even trillions of items. Obviously, for sequence longer than a billion items, it is a better idea to use a cluster than a laptop. A relational table (or more generally a data frame) corresponds to a sequence of object items sharing the same schema. However, sequences of items are more general than tables or data frames and support heterogeneity seamlessly.

When passing Python values to JSONiq or getting them from a JSONiq queries, the mapping to and from Python is as follows:

| Python | JSONiq            |
| ------ | ----------------- |
| tuple  | sequence of items |
| dict   | object            |
| list   | array             |
| str    | string            |
| int    | integer           |
| bool   | boolean           |
| None   | null              |

Furthermore, other JSONiq types will be mapped to string literals. Users who want to preserve JSONiq types can use the Item API instead.

JSONiq is very powerful and expressive. You will find tutorials as well as a reference on JSONiq.org.

## More complex, standalone queries

```python
seq = rumble.jsoniq("""

let $stores :=
[
  { "store number" : 1, "state" : "MA" },
  { "store number" : 2, "state" : "MA" },
  { "store number" : 3, "state" : "CA" },
  { "store number" : 4, "state" : "CA" }
]
let $sales := [
   { "product" : "broiler", "store number" : 1, "quantity" : 20  },
   { "product" : "toaster", "store number" : 2, "quantity" : 100 },
   { "product" : "toaster", "store number" : 2, "quantity" : 50 },
   { "product" : "toaster", "store number" : 3, "quantity" : 50 },
   { "product" : "blender", "store number" : 3, "quantity" : 100 },
   { "product" : "blender", "store number" : 3, "quantity" : 150 },
   { "product" : "socks", "store number" : 1, "quantity" : 500 },
   { "product" : "socks", "store number" : 2, "quantity" : 10 },
   { "product" : "shirt", "store number" : 3, "quantity" : 10 }
]
let $join :=
  for $store in $stores[], $sale in $sales[]
  where $store."store number" = $sale."store number"
  return {
    "nb" : $store."store number",
    "state" : $store.state,
    "sold" : $sale.product
  }
return [$join]
""");

print(seq.json());

seq = rumble.jsoniq("""
for $product in json-lines("http://rumbledb.org/samples/products-small.json", 10)
group by $store-number := $product.store-number
order by $store-number ascending
return {
    "store" : $store-number,
    "products" : [ distinct-values($product.product) ]
}
""");
print(seq.json());




```

## Binding JSONiq variables to Python values

\
It is possible to bind a JSONiq variable to a tuple of native Python values and then use it in a query. JSONiq, variables are bound to sequences of items, just like the results of JSONiq queries are sequence of items. A Python tuple will be seamlessly converted to a sequence of items by the library.  Currently we only support strs, ints, floats, booleans, None, and (recursively) lists and dicts. But if you need more (like date, bytes, etc) we will add them without any problem. JSONiq has a rich type system.

```python
rumble.bind('$c', (1,2,3,4, 5, 6))
print(rumble.jsoniq("""
for $v in $c
let $parity := $v mod 2
group by $parity
return { switch($parity)
         case 0 return "even"
         case 1 return "odd"
         default return "?" : $v
}
""").json())

rumble.bind('$c', ([1,2,3],[4,5,6]))
print(rumble.jsoniq("""
for $i in $c
return [
  for $j in $i
  return { "foo" : $j }
]
""").json())

rumble.bind('$c', ({"foo":[1,2,3]},{"foo":[4,{"bar":[1,False, None]},6]}))
print(rumble.jsoniq('{ "results" : $c.foo[[2]] }').json())
```

It is possible to bind only one value. The it must be provided as a singleton tuple. This is because in JSONiq, an item is the same a sequence of one item.

```python
rumble.bind('$c', (42,))
print(rumble.jsoniq('for $i in 1 to $c return $i*$i').json())
```

For convenience and code readability, you can also use bindOne().

```python
rumble.bindOne('$c', 42)
print(rumble.jsoniq('for $i in 1 to $c return $i*$i').json())
```

## Ways to get and process the output of a JSONiq query

There are several ways to get back the output of the JSONiq query. There are many examples of use further down this page.

| Method                                 | Description                                                                                                                                                                                            | Requirement in availableOutputs()                                                                |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| availableOutputs()                     | Returns a list that helps you understand which output methods you can call. The strings in this list can be Local, RDD, DataFrame, or PUL.                                                             |                                                                                                  |
| json()                                 | Returns the results as a tuple containing dicts, lists, strs, ints, floats, booleans, Nones.                                                                                                           | Local, for lists below the materialization cap (can be increased in the RumbleDB configuration). |
| df()                                   | Returns the results as a pyspark data frame                                                                                                                                                            | DataFrame                                                                                        |
| pdf()                                  | Returns the results as a pandas data frame                                                                                                                                                             | DataFrame                                                                                        |
| rdd()                                  | Returns the results as an RDD containing dicts, lists, strs, ints, floats, booleans, Nones (experimental)                                                                                              | RDD                                                                                              |
| items()                                | Returns the results as a list containing Java Item objects that can be queried with the RumbleDB Item API. Will contain more information and more accurate typing.                                     | Local, for lists below the materialization cap (can be increased in the RumbleDB configuration). |
| open(), hasNext(), nextJSON(), close() | Allows streaming (with no limitation of length) through individuals items as dicts, lists, strs, ints, floats, booleans, Nones.                                                                        | Local                                                                                            |
| open(), hasNext(), next(), close()     | Allows streaming (with no limitation of length) through individuals items as Java Item objects that can be queried with the RumbleDB Item API. Will contain more information and more accurate typing. | Local                                                                                            |
| applyUpdates()                         | Persists the Pending Update List produced by the query (to the Delta Lake or a table registered in the Hive metastore).                                                                                | PUL                                                                                              |

## Binding JSONiq variables to pandas DataFrames

bind() also accepts pandas dataframes

```python
data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [30,25,35]};
pdf = pd.DataFrame(data);

rumble.bind('$a',pdf);
seq = rumble.jsoniq('$a.Name')
```

## Getting the results as a pandas DataFrame

It is also possible to get the results back as a pandas dataframe with pdf() (if the output has a schema, which you can check by calling availableOutputs() and seeing if "DataFrame" is in the returned list).

```
print(seq.pdf())
```

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



## Local access

This materializes the rows as items. The items are accessed with the RumbleDB Item API.

```python
list = res.items();
for result in list:
    print(result.getStringValue())
```

This streams through the items one by one

```python
res.open();
while (res.hasNext()):
    print(res.next().getStringValue());
res.close();
```

\
Native Python/JSON Access for bypassing the Item API (but losing on the richer JSONiq type system)
--------------------------------------------------------------------------------------------------

This method directly gets the result as JSON (dict, list, strings, ints, etc).

```python
jlist = res.json();
for str in jlist:
    print(str);
```

This streams through the JSON values one by one.

```python
res.open();
while(res.hasNext()):
    print(res.nextJSON());
res.close();
```

This gets an RDD of JSON values that can be processed by Python

```python
rdd = res.rdd();
print(rdd.count());
for str in rdd.take(10):
    print(str);
```

## Write back to the disk (or data lake)

&#x20;It is also possible to write the output to a file locally or on a cluster. The API is similar to that of Spark dataframes. Note that it creates a directory and stores the (potentially very large) output in a sharded directory. RumbleDB was already tested with up to 64 AWS machines and 100s of TBs of data.

Of course the examples below are so small that it makes more sense to process the results locally with Python, but this shows how GBs or TBs of data obtained from JSONiq can be written back to disk.

```python
seq = rumble.jsoniq("$a.Name");
seq.write().mode("overwrite").json("outputjson");
seq.write().mode("overwrite").parquet("outputparquet");

seq = rumble.jsoniq("1+1");
seq.write().mode("overwrite").text("outputtext");
```
