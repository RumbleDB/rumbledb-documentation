# JSONiq 3.1

JSONiq 3.1 is an initiative of the RumbleDB team that aligns JSONiq more closely with XQuery 3.1, which has now become a W3C recommendation, but keeping what makes it JSONiq: the flagship feature being the ability to copy-paste JSON into a JSONiq query and with a navigation syntax conducive to Petabyte-scale querying and that appeals to the JSON community.

JSONiq 3.1 does not require a distinct data model (JDM) since XQuery 3.1 support maps and arrays. As a result, JSONiq 3.1 objects are the same as XQuery 3.1 maps and JSONiq 3.1 arrays are the same as XQuery 3.1 arrays.

JSONiq 3.1 does not require a separate serialization mechanism, since XQuery 3.1 supports the JSON output method.

JSONiq 3.1 benefits from all the map and object builtin functions defined in XQuery 3.1.

JSONiq 3.1 is fully interoperable with XQuery 3.1 and can execute on the same virtual machine (similar to Scala and Java). Concretely, XQuery and JSONiq modules can be imported and used in the same (XQuery or JSONiq) query.

This also paves the way for JSONiq 4.0 which will also be aligned with XQuery 4.0 as much as is technically possible.

As a result, the specification for JSONiq 3.1 is even more minimal than that of JSONiq 1.0. This makes it easy to support for any existing XQuery engine to step into the JSON community.

RumbleDB is slowly deploying the use of JSONiq 3.1 but it will take some time as we make sure to sweep in all corners.

## How JSONiq 3.1 amends XQuery 3.1

### Context item

In JSONiq 3.1, the context item is obtained through \$$ and not through a dot.

{% hint style="info" %}
The dot is used for JSONiq's large-scale object lookup syntax.
{% endhint %}

### No dot in names

The . character is disallowed in variable names, function names and QName/NCName literals.

{% hint style="info" %}
The dot is used for JSONiq's large-scale object lookup syntax.
{% endhint %}

### Escaping in strings

String literals use JSON escaping instead of XML escaping (backslash, not ampersand).

{% hint style="info" %}
JSON-like escaping is more natural to the JSON community. This allows in particular copy-pasting (nested) JSON content directly into the query without modification.
{% endhint %}

### Map constructors

In map (object) constructors, the "map" keyword in front is optional.

{% hint style="info" %}
This is forward compatible with XQuery 4.0. This allows copy-pasting (nested) JSON content directly into the query without modification.
{% endhint %}

### Constraints on XPath

A name test on any of the element names true, false, or null must be prefixed with \$$/ and cannot stand on its own.

{% hint style="info" %}
This allows for the parsing of true, false and null literals, allowing copy-pasting JSON content directly into the query without modification.
{% endhint %}

### True, null, and false literals

true and false exist as literals and do not have to be obtained through function calls (true(), false()).

null exists as a literal representing a special item. It is distinct from the empty sequence.

{% hint style="info" %}
The null literal is forward compatible with XQuery 4.0's semantics for json-doc() and parse-doc(), which enable the representation of JSON null literals as a special QName item rather than an empty sequence with the "null" parameter.
{% endhint %}

### Array constructors

The \[ ... ] array constructor has the semantics of XQuery 3.1's array { ... } constructor, in the sense that each item in the child expression's sequence becomes a member of the array.

{% hint style="info" %}
This behavior is familiar to the JSONiq community and is preserved.
{% endhint %}

### Navigation

The dot ., the double square brackets \[\[ ]], and the array unboxing syntax \[] are retained and follow the semantics of JSONiq 1.0. They come in addition to the XQuery 3.1's ? postfix and unary lookup syntax.

{% hint style="info" %}
The difference with ? lookup syntax is that . \[] and \[\[ ]] are semantically optimized for working flexibly on large-scale sequences of items, e.g., billions of items.&#x20;
{% endhint %}

## How JSONiq 3.1 differs from JSONiq 1.0

The data model standardized by the W3C working group is more generic and allows for atomic object keys that are not necessarily strings (dates, etc). Also, an object value or an array value can be a sequence of items and does not need to be a single item. The particular case in which object keys are strings and values are single items (or empty) corresponds to the JSON use.

Unquoted keys are not allowed in JSONiq 3.1 and are considered element name tests.

There are other minor changes in semantics that correspond to the alignment with XQuery 3.1 such as Effective Boolean Values, comparison, etc.
