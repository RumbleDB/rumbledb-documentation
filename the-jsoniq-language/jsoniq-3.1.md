# JSONiq 3.1

JSONiq 3.1 aligns JSONiq more closely with XQuery 3.1, which is now a W3C recommendation, but keeping what makes it JSONiq: the flagship feature being the ability to copy-paste JSON into a JSONiq query and with a navigation syntax that appeals to the JSON community.

JSONiq 3.1 does not require a distinct data model (JDM) since XQuery 3.1 support maps and arrays. As a result, JSONiq 3.1 objects are the same as XQuery 3.1 maps and JSONiq 3.1 arrays are the same as XQuery 3.1 arrays.

JSONiq 3.1 does not require a separate serialization mechanism, since XQuery 3.1 supports the JSON output method.

JSONiq 3.1 benefits from all the map and object builtin functions defined in XQuery 3.1.

JSONiq 3.1 is fully interoperable with XQuery 3.1 and can execute on the same virtual machine (similar to Scala and Java).

As a result, the specification for JSONiq 3.1 is even more minimal than that of JSONiq 1.0. This makes it easy to support for any existing XQuery engine to step into the JSON community.

RumbleDB is slowly deploying the use of JSONiq 3.1 but it will take some time as we make sure to sweep in all corners.

## How JSONiq 3.1 amends XQuery 3.1

### Context item

In JSONiq 3.1, the context item is obtained through \$$ and not through a dot.

### Escaping in strings

String literals use JSON escaping instead of XML escaping (backslash, not ampersand).

### Map constructors

In map (object) constructors, the "map" keyword in front is optional.

### Constraints on XPath

A name test must be prefixed with \$$/ and cannot stand on its own.

### True, null, and false literals

true and false exist as literals and do not have to be obtained through function calls (true(), false()).

null exists as a literal and stands for the empty sequence.

### Navigation

The dot . and double square brackets \[\[ ]] act as syntactic sugars for ? lookup.&#x20;

## How JSONiq 3.1 differs from JSONiq 1.0

The data model standardized by the W3C working group is more generic and allows for atomic object keys that are not necessarily strings (dates, etc). Also, an object value or an array value can be a sequence of items and does not need to be a single item. The particular case in which object keys are strings and values are single items (or empty) corresponds to the JSON use.

Null does not exist as its own type in JSONiq 3.1, instead it is mapped to the empty sequence.

There are other minor changes in semantics that correspond to the alignment with XQuery 3.1 such as Effective Boolean Values, comparison, etc.
