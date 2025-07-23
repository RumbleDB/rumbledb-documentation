# Write back to the disk (or data lake)

Generally, it is possible to write output by to disk using the pandas DataFrame API, the pyspark DataFrame API, or Python's library to write JSON values to disk.

For convenience, we provide a way to also directly do so with the sequence object output by the query.

it is possible to write the output to a file locally or on a cluster. The API is similar to that of Spark dataframes. Note that it creates a directory and stores the (potentially very large) output in a sharded directory. RumbleDB was already tested with up to 64 AWS machines and 100s of TBs of data.

Of course the examples below are so small that it makes more sense to process the results locally with Python, but this shows how GBs or TBs of data obtained from JSONiq can be written back to disk.

```python
seq = rumble.jsoniq("$a.Name");
seq.write().mode("overwrite").json("outputjson");
seq.write().mode("overwrite").parquet("outputparquet");

seq = rumble.jsoniq("1+1");
seq.write().mode("overwrite").text("outputtext");
```
