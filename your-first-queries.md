# Your first queries

This section assumes that you have installed RumbleDB with one of the proposed ways, and guides you through your first queries.

## Create some data set

Create, in the same directory as RumbleDB to keep it simple, a file data.json and put the following content inside. This is a small list of JSON objects in the JSON Lines format.

```
{ "product" : "broiler", "store number" : 1, "quantity" : 20  }
{ "product" : "toaster", "store number" : 2, "quantity" : 100 }
{ "product" : "toaster", "store number" : 2, "quantity" : 50 }
{ "product" : "toaster", "store number" : 3, "quantity" : 50 }
{ "product" : "blender", "store number" : 3, "quantity" : 100 }
{ "product" : "blender", "store number" : 3, "quantity" : 150 }
{ "product" : "socks", "store number" : 1, "quantity" : 500 }
{ "product" : "socks", "store number" : 2, "quantity" : 10 }
{ "product" : "shirt", "store number" : 3, "quantity" : 10 }
```

If you want to later try a bigger version of this data, you can also download a larger version with 100,000 objects from [here](https://rumbledb.org/samples/products-small.json). Wait, no, in fact you do not even need to download it: you can simply replace the file path in the queries below with "https://rumbledb.org/samples/products-small.json" and it will just work! RumbleDB feels just at home on the Web.

RumbleDB also scales without any problems to datasets that have millions or (on a cluster) billions of objects, although of course, for billions of objects HDFS or S3 are a better idea than the Web to store your data, for obvious reasons.

In the JSON Lines format that this simple dataset uses, you just need to make sure you have one object on each line (this is different from a plain JSON file, which has a single JSON value and can be indented). Of course, RumbleDB can read plain JSON files, too (with json-doc()), but below we will show you how to read JSON Line files, which is how JSON data scales.

## Running simple queries locally

Depending on your installation method, the JSONiq queries will go to:

* A cell in a jupyter notebook and with the %%jsoniq magic: a simple click is sufficient to execute.
* The shell: type the query, and finish by pressing Enter twice.
* In a Python program, inside a rumble.jsoniq() call of which you can exploit the output with more Python code.
* A JSONiq query file, which you can execute with the RumbleDB CLI interface.

Either way, the meaning of the queries is the same.

```
"Hello, World"
```

or

```
 1 + 1
 
```

or

```
 (3 * 4) div 5
 
```

The above queries do not actually use Spark. Spark is used when the I/O workload can be parallelized. The following query should output the file created above.

```
 json-lines("data.json")
 
```

json-lines() reads its input in parallel, and thus will also work on your machine with MB or GB files (for TB files, a cluster will be preferable). You should specify a minimum number of partitions, here 10 (note that this is a bit ridiculous for our tiny example, but it is very relevant for larger files), as locally no parallelization will happen if you do not specify this number.

```
for $i in json-lines("data.json", 10)
return $i
```

The above creates a very simple Spark job and executes it. More complex queries will create several Spark jobs. But you will not see anything of it: this is all done behind the scenes. If you are curious, you can go to [localhost:4040](http://localhost:4040) in your browser while your query is running (it will not be available once the job is complete) and look at what is going on behind the scenes.

Data can be filtered with the where clause. Again, below the hood, a Spark transformation will be used:

```
for $i in json-lines("data.json", 10)
where $i.quantity gt 99
return $i
```

RumbleDB also supports grouping and aggregation, like so:

```
for $i in json-lines("data.json", 10)
let $quantity := $i.quantity
group by $product := $i.product
return { "product" : $product, "total-quantity" : sum($quantity) }
```

RumbleDB also supports ordering. Note that clauses (where, let, group by, order by) can appear in any order. The only constraint is that the first clause should be a for or a let clause.

```
for $i in json-lines("data.json", 10)
let $quantity := $i.quantity
group by $product := $i.product
let $sum := sum($quantity)
order by $sum descending
return { "product" : $product, "total-quantity" : $sum }
```

Finally, RumbleDB can also parallelize data provided within the query, exactly like Sparks' parallelize() creation:

```
for $i in parallelize((
 { "product" : "broiler", "store number" : 1, "quantity" : 20  },
 { "product" : "toaster", "store number" : 2, "quantity" : 100 },
 { "product" : "toaster", "store number" : 2, "quantity" : 50 },
 { "product" : "toaster", "store number" : 3, "quantity" : 50 },
 { "product" : "blender", "store number" : 3, "quantity" : 100 },
 { "product" : "blender", "store number" : 3, "quantity" : 150 },
 { "product" : "socks", "store number" : 1, "quantity" : 500 },
 { "product" : "socks", "store number" : 2, "quantity" : 10 },
 { "product" : "shirt", "store number" : 3, "quantity" : 10 }
), 10)
let $quantity := $i.quantity
group by $product := $i.product
let $sum := sum($quantity)
order by $sum descending
return { "product" : $product, "total-quantity" : $sum }
```

Mind the double parenthesis, as parallelize is a unary function to which we pass a sequence of objects.
