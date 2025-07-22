# As a pip package

You can use RumbleDB from within Python programmes by running

```bash
pip install jsoniq
```

_Important note_: since the jsoniq package depends on pyspark 4, Java 17 or Java 21 is a requirement. If another version of Java is installed, the execution of a Python program attempting to create a RumbleSession will lead to an error message on stderr that contains explanations.

You can control your Java version with:

```bash
java -version
```

Information about how this package is used can be found [in this section](../writing-jsoniq-queries-in-python.md).
