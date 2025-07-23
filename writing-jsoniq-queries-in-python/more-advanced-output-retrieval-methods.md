# More advanced output retrieval methods

This section shares more techniques for advanced users who want to make the most of RumbleDB in Python.

## RumbleDB Item API

JSONiq has a rich type system, and the [conversion to JSON values](type-mapping.md) can lose type information.

An alternative consists of retrieving the sequence as a tuple of native items, which can be accessed with the [RumbleDB Item API](https://github.com/RumbleDB/rumble/blob/ccd9b91829510d7fbddf919d6d9ef7e45aa74905/src/main/java/org/rumbledb/api/Item.java).

```python
list = res.items();
for result in list:
    print(result.getStringValue())
```

## Streaming through the items

Sometimes, a sequence can be very long, and materializing it to a tuple of JSON values or a tuple of native items can fail because of the materialization cap. While it can be changed in the configuration to allow for larger tuples, this does not scale.

Another way to retrieve a sequence of arbitrary length is to use the iterator API to stream through the items one by one. If you do not keep previous values in memory, there is no limit to the sequence size than can be retrieved in this way (but it may take more time than using RDDs or DataFrames, which benefit from parallelism).

This is how to stream through the items converted to JSON one by one: &#x20;

```python
res.open();
while(res.hasNext()):
    print(res.nextJSON());
res.close();
```

This is how to stream through the native items, using the Item API:&#x20;

```python
res.open();
while (res.hasNext()):
    print(res.next().getStringValue());
res.close();
```

## Getting unstructured output as an RDD

Sometimes, it is not possible to retrieve the output sequence as a (pandas or pyspark) DataFrame because no schema could be inferred. This is notably the case if the output sequence is heterogeneous (such as a sequence of items mixing atomics, objects of various structures, arrays, etc).

And materializing or streaming may not be an option either if there are billions of items.

In this case, it is possible to obtain the output as an RDD instead. This gets an RDD of JSON values that can be processed by Python (using the [type mapping](type-mapping.md)).

The rdd() method is experimental because we had to reverse-engineer how pyspark encodes RDDs for the Java Virtual Machine (pickling).

```python
rdd = res.rdd();
print(rdd.count());
for str in rdd.take(10):
    print(str);
```
