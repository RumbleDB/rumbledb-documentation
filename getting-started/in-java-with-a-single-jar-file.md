# In Java with a single jar file

## Java version (important)

You need to make sure that you have Java 11 or 17 and that, if you have several versions installed, JAVA\_HOME correctly points to Java 11 or 17.

RumbleDB works with both Java 11 and Java 17. You can check the Java version that is configured on your machine with:

```
java -version
```

If you do not have Java, you can download version 11 or 17 from [AdoptOpenJDK](https://adoptopenjdk.net/).

Do make sure it is not Java 8, which will not work.

## Download RumbleDB

RumbleDB is just a download and no installation is required.

In order to run RumbleDB, you simply need to download rumbledb-1.24.0-standalone.jar from the [download page](https://github.com/RumbleDB/rumble/releases) and put it in a directory of your choice, for example, right besides your data.

Make sure to use the corresponding jar name accordingly in all our instructions in lieu of rumbledb.jar.

You can test that it works with:

```
java -jar rumbledb-2.0.0-standalone.jar run -q '1+1'
```

or launch a JSONiq shell with:

```
java -jar rumbledb-2.0.0-standalone.jar repl
```

If you run out of memory, you can set allocate more memory to Java with an additional Java parameter, e.g., -Xmx10g

The RumbleDB shell appears:

```
    ____                  __    __     ____  ____ 
   / __ \__  ______ ___  / /_  / /__  / __ \/ __ )
  / /_/ / / / / __ `__ \/ __ \/ / _ \/ / / / __  |  The distributed JSONiq engine
 / _, _/ /_/ / / / / / / /_/ / /  __/ /_/ / /_/ /   2.0.0 "Lemon Ironwood" beta
/_/ |_|\__,_/_/ /_/ /_/_.___/_/\___/_____/_____/  


Master: local[*]
Item Display Limit: 200
Output Path: -
Log Path: -
Query Path : -

rumble$
```

You can now start typing simple queries like the following few examples. Press _three times_ the return key to execute a query.

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
