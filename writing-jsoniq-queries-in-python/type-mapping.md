# Type mapping

Any expression in JSONiq returns a sequence of items. Any variable in JSONiq is bound to a sequence of items. Items can be objects, arrays, or atomic values (strings, integers, booleans, nulls, dates, binary, durations, doubles, decimal numbers, etc). A sequence of items can be a sequence of just one item, but it can also be empty, or it can be as large as to contain millions, billions or even trillions of items. Obviously, for sequence longer than a billion items, it is a better idea to use a cluster than a laptop. A relational table (or more generally a data frame) corresponds to a sequence of object items sharing the same schema. However, sequences of items are more general than tables or data frames and support heterogeneity seamlessly.

When passing Python values to JSONiq or getting them from a JSONiq queries, the mapping to and from Python is as follows:

| Python | JSONiq            |
| ------ | ----------------- |
| tuple  | sequence of items |
| dict   | object            |
| list   | array             |
| str    | string            |
| int    | integer           |
| bool   | boolean           |
| None   | null              |

Furthermore, other JSONiq types will be mapped to string literals. Users who want to preserve JSONiq types can use the Item API instead.

JSONiq is very powerful and expressive. You will find tutorials as well as a reference on JSONiq.org.
