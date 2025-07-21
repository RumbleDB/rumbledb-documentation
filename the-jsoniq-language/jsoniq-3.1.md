# JSONiq 3.1

JSONiq 3.1 aligns JSONiq more closely with XQuery 3.1, which is now a W3C recommendation, but keeping what makes it JSONiq, the flagship feature being the ability to copy-paste JSON into a JSONiq query.

RumbleDB is slowly deploying the use of JSONiq 3.1 but it will take some time.

## JSONiq 3.1 compared to XQuery 3.1

### Context item

In JSONiq 3.1, the context item is obtained through \$$ and not through a dot.

### Escaping in strings

String literals use JSON escaping instead of XML escaping (backslash, not ampersand).

### Map constructors

In map (object) constructors, the "map" keyword in front is optional.

### Constraints on XPath

A name test must be prefixed with ./

### True and false literals

true and false exist as literals and do not have to be obtained through function calls (true(), false()).

### Navigation

The dot . and double square brackets \[\[ ]] act as syntactic sugars for ? lookup.&#x20;
