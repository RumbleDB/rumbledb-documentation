# JSONiq coverage

RumbleDB relies on the JSONiq language.

## JSONiq reference

The complete specification can be found [here](../the-jsoniq-language/jsoniq-specification/) and on the [JSONiq.org](http://www.jsoniq.org) website. The implementation is now complete and all JSONiq features are supported including Update and Scripting.

## JSONiq tutorial

A tutorial can be found [here](https://github.com/ghislainfourny/jsoniq-tutorial). All queries in this tutorial will work with RumbleDB.

## JSONiq tutorial for Python users

A tutorial aimed at Python users can be found [here](https://github.com/ghislainfourny/jsoniq-tutorial-python).

## Nested FLWOR expressions

FLWOR expressions support nestedness, for example like so:

```
let $x := for $x in json-lines("file.json")
          where $x.field eq "foo"
          return $x
return count($x)
```

However, keep in mind that parallelization cannot be nested in Spark (there cannot be a job within a job), that is, the following will not work:

```
for $x in json-lines("file1.json")
let $z := for $y in json-lines("file2.json")
          where $y.foo eq $x.fbar
          return $y
return count($z)
```

## Expressions pushed down to Spark

Many expressions are pushed down to Spark out of the box. For example, this will work on a large file leveraging the parallelism of Spark:

```
count(json-lines("file.json")[$$.field eq "foo"].bar[].foo[[1]])
```

Examples of what is pushed down and efficiently computed:

* FLWOR expressions (as soon as a for clause is encountered, binding a variable to a sequence generated with json-lines() or parallelize())
* aggregation functions such as count
* JSON navigation expressions: object lookup (as well as keys() call), array lookup, array unboxing, filtering predicates
* predicates on positions, include use of context-dependent functions position() and last(), e.g.,
* type checking (instance of, treat as)
* many builtin function calls (head, tail, exist, etc)

```
json-lines("file.json")[position() ge 10 and position() le last() - 2]
```

We can push down even more expressions in the future, prioritized on the feedback we receive.

We also push down many expressions to DataFrames and Spark SQL (e.g. after structured-json-lines, csv-file and parquet-file calls). In particular, keys() pushes down the schema lookup if used on parquet-file() and structured-json-lines(). Likewise, count() as well as object lookup, array unboxing and array lookup is also pushed down on DataFrames.

When an expression does not support pushdown, it will materialize automatically. The materialization is capped by default at 100000 items, but this can be changed on the command line with --materialization-cap. See the [configuration page](cli.md) to see how to do it in Python or Java. The separate `--result-size` option controls the display limit (default: `10`). An error is thrown if the materialization cap is exceeded within a query.

## External global variables.

Prologs with user-defined functions and global variables are supported. Global external variables are supported (use `--variable foo=bar` on the command line to assign values to them). If the declared type is not string, then the literal supplied on the command line is cast. If the declared type is anyURI, the path supplied on the command line is also resolved against the working directory to an absolute URI. Thus, anyURI should be used to supply paths dynamically through an external variable.

Context item declarations are supported and a global context item value can be passed with the "--context-item" or "-I" parameter on the command line.

## Library modules

Library modules are supported including location hints. If the provided path is relative, it is resolved against the importing module location.

The same schemes are supported as for reading queries and data: file, hdfs, and so on. HTTP is also supported: you can import modules from the Web!

Example of library module (the file name is library-module.jq):

```
module namespace m = "https://www.example.com";

declare variable $m:x := 2;

declare function mod:func($v) {
  $m:x + $v
);
```

Example of importing module (assuming it is in the same directory):

```
import module namespace mod = "https://www.example.com" at "library-module.jq";

mod:func($mod:x)
```



### Supported types

The JSONiq type system is fully supported. Below is a complete list of JSONiq types and their support status. All builtin types are in the default type namespace, so that no prefix is needed. These types are defined in the XML Schema standard.

All types specific to XML (e.g., NOTATION, NMTOKENS, NMTOKEN, ID, IDREF, ENTITY, etc) are also supported in JSONiq.

| Type               | Status          |
| ------------------ | --------------- |
| atomic             | JSONiq 1.0 only |
| anyAtomicType      | supported       |
| anyURI             | supported       |
| base64Binary       | supported       |
| boolean            | supported       |
| byte               | supported       |
| date               | supported       |
| dateTime           | supported       |
| dateTimeStamp      | supported       |
| dayTimeDuration    | supported       |
| decimal            | supported       |
| double             | supported       |
| duration           | supported       |
| float              | supported       |
| gDay               | supported       |
| gMonth             | supported       |
| gYear              | supported       |
| gYearMonth         | supported       |
| hexBinary          | supported       |
| int                | supported       |
| integer            | supported       |
| long               | supported       |
| negativeInteger    | supported       |
| nonPositiveInteger | supported       |
| nonNegativeInteger | supported       |
| numeric            | supported       |
| positiveInteger    | supported       |
| short              | supported       |
| string             | supported       |
| time               | supported       |
| unsignedByte       | supported       |
| unsignedInt        | supported       |
| unsignedLong       | supported       |
| unsignedShort      | supported       |
| yearMonthDuration  | supported       |

## Newly supported features in RumbleDB 3.0

### Prolog

All prolog settings are supported.

### FLWOR features

Tumbling and sliding windows are supported.

### Function types

Function type syntax is supported as well as annotations (like %public, %private).

### Builtin functions

The vast majority of building functions is supported including all optional parameters and flags. There are very few exceptions such as functions invoking XSLT, which RumbleDB does not support, or dynamically loading a module.

XML-specific functions are also all available in JSONiq.

Buitin functions can now also be used with named function reference expressions (example: concat#2).

### Error variables

Error variables ($err:code, ...) for inside catch blocks are fully supported.

### Updates and scripting

Updates and scripting are fully supported, with Delta Lake and Apache Iceberg as supported persistence layers.
