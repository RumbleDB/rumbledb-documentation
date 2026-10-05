# Advanced configuration

## RumbleDB's specific configuration

In RumbleDB 3.0, configure a session with `.rumbleConfig(path, value)` when creating it:

```python
from jsoniq import RumbleSession

rumble = (
    RumbleSession.builder
    .rumbleConfig("runtime.resultsSizeCap", 1000)
    .rumbleConfig("debug.printIteratorTree", True)
    .getOrCreate()
)
```

To inspect or change an existing session, use the `RumbleConfiguration` adapter returned by `getRumbleConf()`:

```python
conf = rumble.getRumbleConf()
conf.set("runtime.resultsSizeCap", 1000)
conf.set("debug.printIteratorTree", True)

cap = conf.getInt("runtime.resultsSizeCap")
show_plan = conf.getBoolean("debug.printIteratorTree")
language = conf.getString("semantics.queryLanguage")
```

Use dot-separated configuration paths with `set(path, value)`. Pass integers for caps, `True` or `False` for booleans, and strings for language names and paths. Settings apply to subsequent queries; sequences already created keep their compilation settings. The old configuration setters are no longer supported.

`runtime.resultsSizeCap` controls how many items `first()` and notebook display retrieve (default: `10`). To increase the number of items that can be materialized with `json()` or `items()`, change the separate materialization cap (default: `100000`):

```python
conf.set("runtime.materializationCap", 1000000)
```

Setting `debug.printIteratorTree` to `True` prints the internal query plan. This can help data engineers and researchers understand type and execution mode detection and optimizations.

The [configuration reference](../rumbledb-reference/cli.md) lists the supported paths, types, and defaults. Query-source settings and CLI output destinations are primarily for command line use. In Python, pass queries to `rumble.jsoniq(...)` and retrieve or write the returned sequence using its output methods. Bind variables with `rumble.bind(...)`, `rumble.bindOne(...)`, or keyword arguments to `rumble.jsoniq(...)`; remove persistent bindings with `rumble.unbind(...)`.

## Allocating more memory

Allocate Spark memory with `.config(key, value)` when building the session. Use `.rumbleConfig(path, value)` for RumbleDB settings. These calls can be combined with other builder methods, such as `.withDelta()` and `.appName()`.

{% hint style="info" %}
Spark memory settings must be supplied before the first session is created. Subsequent calls to `getOrCreate()` reuse the existing session. In Jupyter, restart the kernel before creating the session to change these startup settings.
{% endhint %}

```python
from jsoniq import RumbleSession

rumble = (
    RumbleSession.builder
    .config("spark.driver.memory", "10g")
    .rumbleConfig("runtime.resultsSizeCap", 1000)
    .getOrCreate()
)
```
