# Using with an existing Spark installation

This method gives you more control about the Spark configuration than the experimental standalone jar, in particular you can increase the memory used, change the number of cores, and so on.

If you use Linux, Florian Kellner also kindly contributed an [installation script](https://github.com/fkellner/rumbledb-install-script) for Linux users that roughly takes care of what is described below for you.

### Install Spark

RumbleDB requires an Apache Spark installation on Linux, Mac or Windows. Important note: it needs to be the scala 2.13 version of spark as RumbleDB supports only that version.

It is straightforward to directly [download it](https://spark.apache.org/downloads.html), unpack it and put it at a location of your choosing. We recommend to pick Spark 3.5.5. Let us call this location SPARK\_HOME (it is a good idea, in fact to also define an environment variable SPARK\_HOME pointing to the absolute path of this location).

What you need to do then is to add the subdirectory "bin" within the unpacked directory to the PATH variable. On macOS this is done by adding

```
export SPARK_HOME=/path/to/spark-3.5.5-bin-hadoop3-scala2.13
export PATH=$SPARK_HOME/bin:$PATH
```

(with SPARK\_HOME appropriately set to match your unzipped Spark directory) to the file .zshrc in your home directory, then making sure to force the change with

```
. ~/.zshrc
```

in the shell. In Windows, changing the PATH variable is done in the control panel. In Linux, it is similar to macOS.

As an alternative, users who love the command line can also install Spark with a package management system instead, such as brew (on macOS) or apt-get (on Ubuntu). However, these might be less predictable than a raw download.

You can test that Spark was correctly installed with:

```
spark-submit --version
```

### Java version (important)

You need to make sure that you have Java 11 or 17 and that, if you have several versions installed, JAVA\_HOME correctly points to Java 11 or 17. Spark only supports Java 11 or 17.

Spark 3+ is documented to work with both Java 11 and Java 17. If there is an issue with the Java version, RumbleDB will inform you with an appropriate error message. You can check the Java version that is configured on your machine with:

```
java -version
```

### Download the small version of the RumbleDB jar

Like Spark, RumbleDB is just a download and no installation is required.

In order to run RumbleDB, you simply need to download one of the small .jar files from the [download page](https://github.com/RumbleDB/rumble/releases) and put it in a directory of your choice, for example, right besides your data.

If you use Spark 3.5, use rumbledb-1.24.0-for-spark-3.5.jar.

If you use Spark 4.0, use rumbledb-1.24.0-for-spark-4.0.jar.

These jars do not embed Spark, since you chose to set it up separately. They will work with your Spark installation with the spark-submit command.

Make sure to use the corresponding jar name accordingly in all our instructions in lieu of rumbledb.jar, replacing rumbledb.jar with the actual name of the jar file you downloaded.

In a shell, from the directory where the RumbleDB .jar lies, type, all on one line:

```
spark-submit rumbledb.jar repl
```

replacing rumbledb.jar with the actual name of the jar file you downloaded.

The RumbleDB shell appears:

```
    ____                  __    __     ____  ____ 
   / __ \__  ______ ___  / /_  / /__  / __ \/ __ )
  / /_/ / / / / __ `__ \/ __ \/ / _ \/ / / / __  |  The distributed JSONiq engine
 / _, _/ /_/ / / / / / / /_/ / /  __/ /_/ / /_/ /   1.24.0 "Lemon Ironwood" beta
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
