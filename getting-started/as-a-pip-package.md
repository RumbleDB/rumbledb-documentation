# In Python as a pip package

You can use RumbleDB from within Python programmes by running

```bash
pip install jsoniq
```

Information about how this package is used in Python code can be found [in this section](../writing-jsoniq-queries-in-python/).

**There is no need to install Spark**, because our jsoniq package automatically installs the pyspark package as well, and uses it.

## Java version

_Important note_: since the jsoniq package depends on pyspark 4, Java 17 or Java 21 is a requirement. If another version of Java is installed, the execution of a Python program attempting to create a RumbleSession will lead to an error message on stderr that contains explanations.

You can control your Java version with:

```bash
java -version
```

## Common issue: colliding Spark version

The latest version of the jsoniq pip package should no longer collide with an existing Spark installation, because by default as of version 3 it ignores SPARK\_HOME. You can override this behavior to force using SPARK\_HOME by adding `.withBundledSpark(False)` in the chain of calls creating the RumbleDB session.

Advanced users who do so may encounter a version issue if SPARK\_HOME points to this alternate installation, and it is a different version of Spark (e.g., 3.5 or 3.4). The jsoniq package requires Spark 4.0.

If this happens, RumbleDB should output an informative error message. They are two ways to fix such conflicts:

* The easiest is not override the default behavior in the first place. This will have RumbleDB fall back to the Spark 4.0 installation that ships with its pyspark dependency.
* Or you can instead change the value of SPARK\_HOME to point to a Spark 4.0 installation, if you have one. This would be for more advanced users who know what they are doing.

If you have another working Spark installation on your machine, you can see which version it is with

```
spark-submit --version
```

The above command is of course expected not to work for first-time users who only installed the jsoniq package and never installed Spark additionally on their machine.
