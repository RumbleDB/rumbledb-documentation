# Command line (java -jar)

## Java version (important)

You need to make sure that you have Java 17 or 21 and that, if you have several versions installed, JAVA\_HOME correctly points to Java 17 or 21.

RumbleDB works with Java 17 and Java 21. You can check the Java version that is configured on your machine with:

```
java -version
```

If you do not have Java, you can download version 17 or 21 from [AdoptOpenJDK](https://adoptopenjdk.net/).

Java 8 and Java 11 cannot run RumbleDB 3.0.

## Download RumbleDB

RumbleDB is just a download and no installation is required.

In order to run RumbleDB, you simply need to download rumbledb-3.0.0-standalone.jar from the [download page](https://github.com/RumbleDB/rumble/releases) and put it in a directory of your choice, for example, right besides your data.

Use the standalone jar with `java -jar`; use a matching thin jar with `spark-submit` as described on the next page.

You can test that it works with:

```
java -jar rumbledb-3.0.0-standalone.jar run -q '1+1'
```

or launch a JSONiq shell with:

```
java -jar rumbledb-3.0.0-standalone.jar repl
```

If you run out of memory, allocate more memory with a JVM option before `-jar`, for example:

```sh
java -Xmx10g -jar rumbledb-3.0.0-standalone.jar repl
```

The RumbleDB shell appears:

```
    ____                  __    __     ____  ____ 
   / __ \__  ______ ___  / /_  / /__  / __ \/ __ )
  / /_/ / / / / __ `__ \/ __ \/ / _ \/ / / / __  |  The distributed JSONiq engine
 / _, _/ /_/ / / / / / / /_/ / /  __/ /_/ / /_/ /   3.0.0 "Coast Redwood" beta
/_/ |_|\__,_/_/ /_/ /_/_.___/_/\___/_____/_____/  

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

Javadoc

If you plan to add the jar to your Java environment to use RumbleDB in your Java programs, the JavaDoc documentation can be found [here](https://rumbledb.org/docs/latest/api/). The entry point is the class org.rumbledb.api.Rumble.
