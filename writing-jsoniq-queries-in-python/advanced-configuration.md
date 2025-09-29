# Advanced configuration

## RumbleDB's specific configuration

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

## Allocating more memory

If you get an out-of-memory error, it is possible to allocate memory when you build the Rumble session with a config() call. This is exactly the same way it is done when building a Spark session. The config() call can of course be used in combination with any other method calls that are part of the builder chain (withDelta(), appName(), config(), etc).

{% hint style="info" %}
This will only have an effect when done the first time the session is created. Spark and Java are not able to adjust the memory automatically after the session has been created. Subsequent calls of getOrCreate() do not create a new session, they only get the existing one.

In Jupyter, you can restart the Kernel before you create the session, to force a new session.&#x20;
{% endhint %}

For example:

```
rumble = RumbleSession.builder
.config("spark.driver.memory", "10g")
.getOrCreate()
```
