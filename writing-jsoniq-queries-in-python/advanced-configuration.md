# Advanced configuration

It is possible to access RumbleDB's advanced configuration parameters with

```
conf = rumble.getRumbleConf()
```

Then, you can change the value of some parameters. For example, you can increase the number of JSON values that you can retrieve with a json() call:

```
conf.setResultSizeCap(1000)
```

You can also configure RumbleDB to output verbose information about the internal query plan, type and mode detection, and optimizations. This can be of interest to data engineers or researchers to understand how RumbleDB works.

```
conf.setPrintIteratorTree(True)
```

The complete API for configuring RumbleDB is accessible in our [JavaDoc](https://rumbledb.org/docs/latest/api/org/rumbledb/config/RumbleRuntimeConfiguration.html) pages. These methods are also callable in Python.

Warning: some of the configuration methods do not make sense in Python and are specific to the command line edition of RumbleDB (such as setting the query content or an output path and input/output format). Also, setting external variables in Python should not be done via the configuration, but with the bind() and unbind() functions or extra parameters in jsoniq() calls.
