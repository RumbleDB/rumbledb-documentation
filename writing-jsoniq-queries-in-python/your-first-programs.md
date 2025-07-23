# Your first programs

The syntax to start a session is similar to that of Spark. A RumbleSession is a SparkSession that additionally knows about RumbleDB. All attributes and methods of SparkSession are also available on RumbleSession.

Even though RumbleDB uses Spark internally, it can be used without any knowledge of Spark.

Executing a query is done with rumble.jsoniq() like so.

```python
from jsoniq import RumbleSession

rumble = RumbleSession.builder.getOrCreate();

items = rumble.jsoniq('1+1')
python_tup = items.json()
print(python_tup)
```

A query returns a sequence of items, here the sequence with just the integer item 2.

There are several ways to retrieve the results of the query. Calling the json() is just one of them. It retrieves the sequence of as a tuple of JSON values that Python can process. The detailed [type mapping for this is described here](type-mapping.md). Other methods for [retrieving the output of a query are described here](ways-to-get-and-process-the-output-of-a-jsoniq-query.md).

## More complex, standalone queries

Below are a few examples showing what is possible with JSONiq. You can [learn JSONiq with our interactive tutorial](https://colab.research.google.com/github/RumbleDB/rumble/blob/master/RumbleSandbox.ipynb). You will also find [a full language reference here](../the-jsoniq-language/jsoniq-specification.md).

For complex queries, you can use Python's ability to spread strings over multiple lines, and with no need to escape special characters.

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
