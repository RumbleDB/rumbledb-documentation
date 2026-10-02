# RumbleDB 3.0 "Coast Redwood"

RumbleDB is a querying engine that allows you to query your large, messy datasets with ease and productivity. It covers the entire data pipeline: clean up, structure, normalize, validate, convert to an efficient binary format, and feed it right into Machine Learning estimators and models

RumbleDB supports two twin languages: JSONiq and XQuery. They are very similar. XQuery is a standard of the W3C, while JSONiq is a similar language that was adapted to be more appealing to the JSON community.

RumbleDB supports JSON-like datasets including JSON, JSON Lines, Parquet, Avro, SVM, CSV, ROOT as well as text files, as well as XML and YAML, of any size from kB to at least the two-digit TB range (we have not found the limit yet).

RumbleDB also supports the most recent data lakehouse formats such as the Delta Lake (directly as a file, or via Apache Hive) and Apache Iceberg, via the declarative JSONiq Update Facility as well as side effects with the JSONiq Scripting Extension.

RumbleDB is both good at handling small amounts of data on your laptop (in which case it simply runs locally and efficiently in a single-thread) as well as large amounts of data by spreading computations on your laptop cores, or onto a large cluster (in which case it leverages Spark automagically).

RumbleDB can also be used to easily and efficiently convert data from a format to another, including from JSON to Parquet thanks to JSound validation. It is a real Swiss army knife 🇨🇭 for your data!&#x20;

It runs on many local or distributed filesystems such as HDFS, S3, Azure blob storage, Google Cloud, and HTTP (read-only)—and of course your local drive as well. You can use any of these file systems to store your datasets, but also to store and share your queries and functions as library modules with other users, worldwide or within your institution, who can import them with just one line of code. You can also output the results of your query or the log to these filesystems (as long as you have write access).

With RumbleDB, queries can be written in the tailor-made and expressive JSONiq language. Users can write their queries declaratively and start with just a few lines. No need for complex JSON parsing machinery as JSONiq supports the JSON data model natively, as well as XML.

XQuery aficionados can opt to use XQuery 3.1 instead. Both JSONiq and XQuery run on the same virtual machine, share the same internal representation and the same runtime, similar to Java and Scala. They can even import modules from each other.

The core of RumbleDB lies in JSONiq's and XQuery's FLWOR expressions, the semantics of which map beautifully to DataFrames and Spark SQL. Likewise expression semantics is seamlessly translated to transformations on RDDs or DataFrames, depending on whether a structure is recognized or not. Transformations are not exposed as function calls, but are completely hidden behind JSONiq queries, giving the user the simplicity of an SQL-like language and the flexibility needed to query heterogeneous, tree-like data that does not fit in DataFrames. If you heard about Google's SQL Pipe Syntax, this is similar—but even more intuitive, seamless, flexible, and it even predates it.

This documentation provides you with instructions on how to get started, examples of data sets and queries that can be executed locally or on a cluster, links to JSONiq reference and tutorials, notes on the function library implemented so far, and instructions on how to compile RumbleDB from scratch.

As of October 2026, RumbleDB passes more than 95% of the X3C QT3 test suite, which contains around 31,800 tests. It is thus interoperable with other engines.

We welcome bug reports in the GitHub issues section, as well as questions on writing JSONiq or XQuery queries on StackOverflow. Do not hesitate to ask us if you need any support.
