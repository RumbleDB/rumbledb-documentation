# Configuration parameters

This page documents the RumbleDB 3 command line and configuration parameters. Configuration settings are also available through the Java `org.rumbledb.api.RumbleConfiguration` class and the Python session configuration API.

## Command line modes

Select a mode with the first argument:

| Command | Availability | Semantics |
| --- | --- | --- |
| `run` | RumbleDB 2 and 3 | Executes a query supplied as a string or read from a file. |
| `repl` | RumbleDB 2 and 3 | Starts the interactive shell. |
| `serve` | RumbleDB 2 only; dropped in RumbleDB 3 | Starts the legacy HTTP server. |

For RumbleDB 3:

```sh
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run file.jq -o output-dir -P 1
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run -q '1+1'
spark-submit rumbledb-3.0.0-for-spark-4.0.jar repl --result-size 10
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run --help
```

`run` requires exactly one query source: `--query`, `--query-path`, or a positional query file. `repl` does not take a query source. The other options below are shared by `run` and `repl`, except where their purpose is specific to one mode.

Boolean CLI options are flags: use `--static-typing` to enable static typing and `--no-static-typing` to disable it. All boolean options below support the corresponding `--no-...` form. Do not pass `yes` or `no` as separate arguments to these flags. Serialization options such as `--output-format-option indent=yes` have their own value syntax.

`--help` (shortcut `-h`) displays help for the selected command, or for the launcher when used without a command. It has no configuration key.

## Updating commands from RumbleDB 2

These examples use the Spark 4.0 jar. For another Spark version, use the matching `rumbledb-3.0.0-for-spark-<version>.jar`. With the standalone jar, replace `spark-submit rumbledb-3.0.0-for-spark-4.0.jar` with `java -jar rumbledb-3.0.0-standalone.jar`.

* Select `run` or `repl` explicitly. The old `--shell` and `--server` options are no longer accepted; `serve` is unavailable in RumbleDB 3.
* Output options now use `--output-format-option name=value`, and variable bindings use `--variable name=value` or `--variable-from-file name=path`. Repeat the option for each entry. The former colon-based option names are no longer accepted.
* Boolean options take no separate `yes` or `no` argument. For example, use `--overwrite` or `--no-overwrite`.
* Use `--result-size` to change the display limit (default: `10`). `--materialization-cap` / `-c` controls materialization during execution (default: `100000`).

For example, bind a variable and configure serialization:

```sh
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run \
    -q 'declare variable $foo external; $foo' \
    --variable 'foo=hello world' \
    -f serialize-each-item --output-format-option method=json \
    --output-format-option indent=yes
```

## Configuration from Python and Java

The **Python/Java configuration key** column gives the exact, case-sensitive string path used by the API. For Python, configure a session when creating it:

```python
from jsoniq import RumbleSession

rumble = (
    RumbleSession.builder
    .rumbleConfig("runtime.resultsSizeCap", 1000)
    .rumbleConfig("debug.printIteratorTree", True)
    .getOrCreate()
)
```

Use `.rumbleConfig(path, value)` for RumbleDB settings and `.config(key, value)` for Spark settings, such as `spark.driver.memory`.

To inspect or change an existing session's configuration:

```python
conf = rumble.getRumbleConf()
conf.set("runtime.resultsSizeCap", 1000)
conf.set("debug.printIteratorTree", True)

cap = conf.getInt("runtime.resultsSizeCap")
show_plan = conf.getBoolean("debug.printIteratorTree")
language = conf.getString("semantics.queryLanguage")
```

Changes apply to subsequent queries. Already-created sequences retain the configuration with which they were compiled. Python values must have the appropriate type: use integers for caps, `True` or `False` for booleans, and strings for language names and paths. Legacy setters such as `setResultSizeCap()` and `setPrintIteratorTree()` were removed in RumbleDB 3; use `set(path, value)` instead.

In Java, use the configuration builder:

```java
RumbleConfiguration configuration = RumbleConfiguration.builder()
    .with("runtime.resultsSizeCap", 1000)
    .with("debug.printIteratorTree", true)
    .build();
```

Import `org.rumbledb.api.RumbleConfiguration`. The configuration is immutable; `toBuilder()` creates a builder initialized with its current values. Read settings with `get(path)`, `getInt(path)`, `getBoolean(path)`, or `getString(path)`.

The tables below use Python notation for defaults (`True`, `False`, and `None`). `None` means that no value is configured. Query-source settings and CLI output destinations are primarily for command line use; in Python, pass queries to `rumble.jsoniq(...)` and retrieve or write the returned sequence using its output methods.

## Query source

These CLI options apply to `run` only.

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--query` | `-q` | `input.query` | string | `None` | Query supplied directly as a string, for example `1+1`. |
| `--query-path` | Positional query file | `input.queryPath` | string | `None` | Query file to read from a supported file system or URL, for example `file:///folder/file.jq`. |

## Runtime

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--result-size` | — | `runtime.resultsSizeCap` | integer | `10` | Maximum number of items to display on screen or retrieve through Python's `first()` method. Python's `json()` uses the separate materialization cap. |
| `--materialization-cap` | `-c` | `runtime.materializationCap` | integer | `100000` | Maximum number of items to materialize from large distributed sequences during execution, for example when collecting an RDD or DataFrame into an array. Also used by Java's full-list retrieval. This is separate from the result display cap. |
| `--native-sql-predicates` | — | `runtime.useNativeSQLPredicates` | boolean | `True` | Enables native SQL predicates when possible. |
| `--data-frame-execution-mode-detection` | — | `runtime.detectDataFrameExecutionMode` | boolean | `True` | Enables DataFrame execution mode detection for higher-order functions. |
| `--parallel-execution` | — | `runtime.useParallelExecution` | boolean | `True` | Enables parallel execution when possible. |
| `--data-frame-execution` | — | `runtime.useDataFrameExecution` | boolean | `True` | Enables DataFrame execution when possible. |
| `--native-execution` | — | `runtime.useNativeExecution` | boolean | `True` | Enables native Spark SQL execution when possible. |
| `--apply-updates` | — | `runtime.shouldApplyUpdates` | boolean | `False` | Applies the pending update list returned by an updating query. |

## Output

Output destinations, execution logs, and shell filters are primarily CLI settings. A configuration key does not imply that Python query retrieval automatically writes to the configured destination.

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--output-path` | `-o` | `output.outputPath` | string | `None` | Output destination. `serialize` writes one file containing the serialized string. `serialize-each-item` writes newline-separated serialized items to partition files, or one file with `-P 1`. Other formats write partition files (local JSON with one partition can be a single file). Without a destination, the CLI displays results on standard output. |
| `--output-format` | `-f` | `output.outputFormat` | string | `None` (`serialize-each-item` for JSONiq; `serialize` for XQuery) | Output file format: `json`, `csv`, `avro`, `parquet`, or another supported Spark format. The special format `serialize` serializes the entire result sequence to one string using the configured serialization method. `serialize-each-item` serializes each item independently, with newlines between the resulting strings. Only `serialize` and `serialize-each-item` work without `--output-path`. Spark file formats, including `json`, require an output path. Formats other than `json`, `serialize`, and `serialize-each-item` also require a sequence representable as a DataFrame (structured objects or supported atomic values); `annotate()` can supply a schema. |
| `--output-format-option` | — | `output.serializationParameters` (see below) | `name=value` on the CLI; object in the API | `None` | Repeatable serialization or Spark writer option, for example `--output-format-option indent=yes --output-format-option compression=gzip`. |
| `--overwrite` | `-O` | `output.allowOverwrite` | boolean | `False` | Allows overwriting an existing CLI output path; otherwise an existing destination raises an error. |
| `--number-of-output-partitions` | `-P` | `output.numberOfOutputPartitions` | integer | `-1` | Positive values request that many output partitions. `-1` leaves the partition count unspecified. `serialize` always writes one string and rejects values greater than `1`. |
| `--log-path` | — | `output.logPath` | string | `None` | Destination for CLI execution timing and profiler information. This is separate from diagnostic logging levels. |
| `--shell-filter` | — | `output.shellFilter` | string | `None` | Command used to post-process interactive shell output through standard input, for example `jq .`. |

The output format and serialization method are distinct. `--output-format csv` selects the CSV file writer; setting `--output-format-option method=xml` does not change that writer. Both `serialize` and `serialize-each-item` accept serialization methods such as `xml`, `json`, `text`, `adaptive`, or the RumbleDB extension `xml-json-hybrid`. The default output format and method follow the query language (including version declarations and file extensions):

| Query language | Default output format | Default method |
| --- | --- | --- |
| JSONiq | `serialize-each-item` | `xml-json-hybrid` |
| XQuery | `serialize` | `xml` |

Explicit format and method options override these defaults independently. The serialization parameter `item-separator` is absent by default in both languages; W3C sequence normalization inserts spaces between adjacent atomic values when serializing a whole sequence. Newlines between independently serialized items come from the `serialize-each-item` output format, rather than from this parameter. To serialize an entire sequence with a chosen method:

```sh
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run -q '1, [2, 3], 4' -o result.txt -f serialize \
    --output-format-option method=text --output-format-option 'item-separator=|'
```

This writes exactly `1|2|3|4`, without an extra trailing newline. Serialization follows sequence normalization: for example, text/XML methods flatten arrays, while JSON serialization requires at most one top-level item (use an array to serialize multiple JSON values together). Query serialization declarations take precedence over CLI serialization defaults. File encoding follows the `encoding` serialization parameter, whose default is `UTF-8`.

`serialize` materializes the whole sequence subject to `runtime.materializationCap`; `runtime.resultsSizeCap` does not truncate it. Without `--output-path`, `serialize` displays the serialized string. `serialize-each-item` displays items subject to the result-size cap, but writes all items when an output path is given. It supports multiple output partitions without collecting the whole result sequence. Its file output honors the configured encoding and terminates each serialized item with a newline. A serialized item can itself contain newlines, so this is not necessarily one physical line per item. The `item-separator` parameter does not control the newline between serialized items; it can still affect normalization within an item, such as an array serialized with the text method. All Spark file formats, including JSON, CSV, Avro, and Parquet, require an output path. For JSON on standard output, select `-f serialize-each-item --output-format-option method=json`, or use `-f serialize --output-format-option method=json` for a single JSON value.

For example, to write multiple independent JSON values:

```sh
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run -q '1, 2, 3' -o results.jsonl -P 1 -f serialize-each-item \
    --output-format-option method=json --output-format-option 'item-separator=|'
```

This writes `1`, `2`, and `3` on separate lines. The configured `|` does not replace the newlines. In contrast, `-f serialize` with method `json` rejects this sequence because JSON serialization requires at most one top-level item.

`--output-format-option` does not map to an arbitrary `output.serializationParameters.foo` key. CLI option names are converted into the serialization object's fields. For example, `indent` maps to `output.serializationParameters.indent`, `indent-spaces` maps to `output.serializationParameters.indentSpaces`, and Spark writer options such as `compression` belong to `output.serializationParameters.sparkOptions`. API values use native types rather than CLI strings:

```python
conf.set("output.serializationParameters.indent", True)
conf.set("output.serializationParameters.indentSpaces", 2)
conf.set("output.serializationParameters.sparkOptions", {"compression": "gzip"})
```

## Diagnostics

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--print-iterator-tree` | — | `debug.printIteratorTree` | boolean | `False` | Prints the expression tree and runtime iterator tree. |
| `--show-error-info` | `-v` | `debug.showErrorInfo` | boolean | `False` | Displays detailed error information and exception stacks for debugging or bug reports. |
| `--debug` | — | `debug.logging` | boolean | `False` | Enables the engine's debug output. Diagnostic logging levels are configured separately below. |
| `--log-level` | — | `debug.logLevel` | string | `None` (CLI normally uses `warn`) | CLI diagnostic logging level: `off`, `fatal`, `error`, `warn`, `info`, `debug`, `trace`, or `all`. |
| `--spark-log-level` | — | `debug.sparkLogLevel` | string | `"off"` | CLI Spark/Hadoop logging level; accepts the same levels as `--log-level`. |

When `--log-level` is omitted, `--debug`, `--print-iterator-tree`, or `--print-inferred-types` enables the diagnostic logging needed to display the requested information. An explicit `--log-level` takes precedence. Diagnostic output goes to standard error so it does not mix with query results.

## Static analysis

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--static-typing` | `-t` | `analysis.enableStaticTyping` | boolean | `False` | Activates static type analysis, annotating expressions with inferred types and enabling additional optimizations. Experimental. |
| `--print-inferred-types` | — | `analysis.printInferredTypes` | boolean | `False` | Prints inferred types during analysis. |
| `--check-return-types-of-builtin-functions` | — | `analysis.checkReturnTypeOfBuiltinFunctions` | boolean | `False` | Checks the return types of built-in functions. |

## Optimizations

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--function-inlining` | — | `optimization.useFunctionInlining` | boolean | `True` | Enables inlining of non-recursive functions. |
| `--tail-call-optimization` | — | `optimization.useTailCallOptimization` | boolean | `True` | Enables tail call optimization. |
| `--optimize-general-comparison-to-value-comparison` | — | `optimization.optimizeGeneralComparisonToValueComparison` | boolean | `True` | Rewrites general comparisons as value comparisons when applicable. |
| `--optimize-steps` | — | `optimization.optimizeSteps` | boolean | `True` | Enables XPath step optimizations, which may affect stability of document order. |
| `--optimize-steps-experimental` | — | `optimization.optimizeStepsExperimental` | boolean | `False` | Enables experimental step optimizations that skip uniqueness checks or sorting in some cases; correctness is not yet verified. |
| `--optimize-parent-pointers` | — | `optimization.optimizeParentPointers` | boolean | `True` | Removes parent pointers when no steps requiring them are detected statically. |

## Language and semantics

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--default-language` | — | `semantics.queryLanguage` | string | `"jsoniq10"` | Default query language: `jsoniq10`, `jsoniq31`, or `xquery31`. |
| `--xml-version` | — | `semantics.xmlVersion` | string | `"1.1"` | XML version: `1.0` or `1.1`. |
| `--dates-with-timezone` | — | `semantics.datesWithTimeZone` | boolean | `False` | Enables timezone support for `xs:date`. |
| `--lax-json-null-validation` | — | `semantics.laxJSONNullValidation` | boolean | `True` | Allows JSON nulls and absent values to be conflated when validating nillable object fields. |
| `--static-base-uri` | — | `semantics.staticBaseUri` | string | `None` | Static base URI, for example `../data/`. Overrides the module location; a declaration in the query takes precedence. |

The misspelled `--lax-json-null-valication` remains a supported alias for `--lax-json-null-validation`. Use the correctly spelled option in new commands.

## Date and time formatting

These defaults are used by date/time formatting functions when no explicit place, calendar, or language is supplied.

| CLI option | Shortcut | Python/Java configuration key | Type | Default | Semantics |
| --- | --- | --- | --- | --- | --- |
| `--default-formatting-place` | — | `formatting.defaultFormattingPlace` | string | `None` | Formatting timezone, for example `Europe/Zurich` or `UTC`. When unset, formatting preserves the value's timezone. |
| `--default-formatting-calendar` | — | `formatting.defaultFormattingCalendar` | string | `"ISO"` | Formatting calendar; use a supported calendar identifier. |
| `--default-formatting-language` | — | `formatting.defaultFormattingLanguage` | string | `"en"` | Formatting language; use a supported language identifier. |

## External variables and context item

Bindings are separate from configuration and have no Python/Java configuration string key. The query must declare the corresponding external variable (for example, `declare variable $foo external;`) or context item (`declare context item external;`).

| CLI option | Shortcut | Python/Java configuration key | Example value | Semantics |
| --- | --- | --- | --- | --- |
| `--variable` | — | N/A (binding API) | `foo=bar` | Binds external variable `$foo` to a lexical value. Repeat the option for different variables. |
| `--variable-from-file` | — | N/A (binding API) | `foo=data.json` | Binds external variable `$foo` from a file. A variable cannot also be supplied with `--variable`. |
| `--context-item` | `-I` | N/A (binding API) | `bar` | Binds the global context item `$$` to a lexical value. Mutually exclusive with `--context-item-input`. |
| `--context-item-input` | `-i` | N/A (binding API) | `data.json` or `-` | Reads the context item from a file, or from standard input when the path is `-`. |
| `--context-item-input-format` | — | N/A (binding API) | `json` or `text` | Format for context item file or standard input parsing. Default: `json`. |

For example:

```sh
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run -q 'declare variable $foo external; $foo' --variable foo=bar
spark-submit rumbledb-3.0.0-for-spark-4.0.jar run -q 'declare context item external; $$' -i data.json
```

In Python, use `rumble.bind(...)`, `rumble.bindOne(...)`, or keyword arguments to `rumble.jsoniq(...)` for variables; do not try to configure variable values through `conf.set(...)`. See [Binding JSONiq variables to Python values](../writing-jsoniq-queries-in-python/binding-jsoniq-variables-to-python-values.md).
