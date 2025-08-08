# Writing JSONiq queries in Python

You can use RumbleDB from within Python programmes by running

```bash
pip install jsoniq
```

## Java version

_Important note_: since the jsoniq package depends on pyspark 4, Java 17 or Java 21 is a requirement. If another version of Java is installed, the execution of a Python program attempting to create a RumbleSession will lead to an error message on stderr that contains explanations.

You can control your Java version with:

```bash
java -version
```

Information about how this package is used can be found [in this section](./).

## Common issue: colliding Spark version

Some users who have already configured a Spark installation on their machine may encounter a version issue if SPARK\_HOME points to this alternate installation, and it is a different version of Spark (e.g., 3.5 or 3.4). The jsoniq package requires Spark 4.0.

If this happens, RumbleDB should output an informative error message. They are two ways to fix such conflicts:

* The easiest is remove the SPARK\_HOME environment variable completely. This will have RumbleDB fall back to the Spark 4.0 installation that ships with its pyspark dependency.
* Or you can instead change the value of SPARK\_HOME to point to a Spark 4.0 installation, if you have one. This would be for more advanced users who know what they are doing.

If you have another working Spark installation on your machine, you can see which version it is with

```
spark-submit --version
```

The above command is of course expected not to work for first-time users who only installed the jsoniq package and never installed Spark additionally on their machine.

### High-level information on the library

A RumbleSession is a wrapper around a SparkSession that additionally makes sure the RumbleDB environment is in scope.

JSONiq queries are invoked with rumble.jsoniq() in a way similar to the way Spark SQL queries are invoked with spark.sql().

JSONiq variables can be bound to lists of JSON values (str, int, float, True, False, None, dict, list) or to Pyspark DataFrames. A JSONiq query can use as many variables as needed (for example, it can join between different collections).

It will later also be possible to read tables registered in the Hive metastore, similar to spark.sql(). Alternatively, the JSONiq query can also read many files of many different formats from many places (local drive, HTTP, S3, HDFS, ...) directly with simple [builtin function calls](../rumbledb-reference/input.md) such as json-lines(), text-file(), parquet-file(), csv-file(), etc.

The resulting sequence of items can be retrieved as a list of JSON values, as a Pyspark DataFrame, or, for advanced users, as an RDD or with a streaming iteration over the items using the [RumbleDB Item API](https://github.com/RumbleDB/rumble/blob/master/src/main/java/org/rumbledb/api/Item.java).

It is also possible to write the sequence of items to the local disk, to HDFS, to S3, etc in a way similar to how DataFrames are written back by Pyspark.

The design goal is that it is possible to chain DataFrames between JSONiq and Spark SQL queries seamlessly. For example, JSONiq can be used to clean up very messy data and turn it into a clean DataFrame, which can then be processed with Spark SQL, spark.ml, etc.

Any feedback or error reports are very welcome.

