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

We will document more tunable parameters here soon.
