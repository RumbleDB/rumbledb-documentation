# Advanced configuration

It is possible to access RumbleDB's advanced configuration parameters with

```
conf = rumble.getRumbleConf()
```

Then, you can change the value of some parameters. For example, you can increase the number of JSON values that you can retrieve with a json() call:

```
conf.setResultSizeCap(1000)
```

We will document more tunable parameters here soon.
