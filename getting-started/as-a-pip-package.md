# As a pip package

You can use RumbleDB from within Python programmes by running

```bash
pip install jsoniq
```

## Java version

_Important note_: since the jsoniq package depends on pyspark 4, Java 17 or Java 21 is a requirement. If another version of Java is installed, the execution of a Python program attempting to create a RumbleSession will lead to an error message on stderr that contains explanations.

You can control your Java version with:

```bash
java -version
```

Information about how this package is used can be found [in this section](../writing-jsoniq-queries-in-python/).

## Common issue: colliding Spark version

Some users who have already configured a Spark installation on their machine may encounter a version issue if SPARK\_HOME points to this alternate installation, and it is a different version of Spark (e.g., 3.5 or 3.4). The jsoniq package requires Spark 4.0.

If this happens, RumbleDB should output an informative error message. They are two ways to fix such conflicts:

* The easiest is remove the SPARK\_HOME environment variable completely. This will have RumbleDB fall back to the Spark 4.0 installation that ships with its pyspark dependency.
* Or you can instead change the value of SPARK\_HOME to point to a Spark 4.0 installation, if you have one. This would be for more advanced users who know what they are doing.
