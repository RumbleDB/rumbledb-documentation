# Writing queries directly in Jupyter notebook cells

The Python edition of RumbleDB comes out of the box with a JSONiq magic.&#x20;

If you are in a Jupyter notebook and have installed the jsoniq pip package, you can activate the jsoniq magic with:

```
%load_ext jsoniqmagic
```

Then, you can run JSONiq in standalone cells and see the results:

```
%%jsoniq
{"foobar":1} 
```

Of course, you can still continue to use rumble.jsoniq() calls and process the outputs as you see fit.

An example of the magic in action is available in our [upgraded online sandbox](https://colab.research.google.com/github/RumbleDB/rumble/blob/master/RumbleSandbox.ipynb).

Note: This is a different magic than the magic that works with the RumbleDB HTTP server. It is more modern and running a server is no longer needed with this different magic. It suffices to install the jsoniq Python package.&#x20;

## Change the behavior to output DataFrames

By default, the output will be in the form of serialized JSON values. If the output is structured, then you can change this default behavior to show it in the form of a DataFrame instead.

For a pandas DataFrame:

```notebook-python
%%jsoniq -pdf
for $i in 1 to 10000000
return { "foobar" : $i}
```

For a pyspark DataFrame:

```
%%jsoniq -df
for $i in 1 to 10000000
return { "foobar" : $i}
```

Note that it will not work in all cases. If the output is not fully structured or RumbleDB is unable to infer a DataFrame schema, you can specify the schema yourself.

```
%%jsoniq -pdf
declare type local:mytype as {
    "product" : "string",
    "store-number" : "int",
    "quantity" : "decimal"
};
validate type local:mytype* { 
    for $product in json-lines("http://rumbledb.org/samples/products-small.json", 10)
    where $product.quantity ge 995
    return $product
}
```

It is possible to measure the response time with the -t parameter:

```
%%jsoniq -t
for $i in 1 to 10000000
return { "foobar" : $i}
```
