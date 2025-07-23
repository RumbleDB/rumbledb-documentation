# The JSONiq data model

JSONiq is a query language that was specifically designed for querying JSON, although its data model is powerful enough to handle more similar formats.

As stated on json.org, JSON is a "lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate."

A JSON document is made of the following building blocks: objects, arrays, strings, numbers, booleans and nulls.

JSONiq manipulates sequences of these building blocks, which are called items. Hence, a JSONiq value is a sequence of items.

Any JSONiq expression takes and returns sequences of items.

Comma-separated JSON-like building blocks is all you need to begin building your own sequences. You can mix and match, as JSONiq supports heterogeneous sequences seamlessly.

### A sequence

```

"foo", 2, true, { "foo", "bar" }, null, [ 1, 2, 3 ]
      
```

Result:foo 2 true foo bar null \[ 1, 2, 3 ]

Sequences are flat and cannot be nested. This makes streaming possible, which is very powerful.

### Sequences are flat

```

( ("foo", 2), ( (true, 4, null), 6 ) )
      
```

Result:foo 2 true 4 null 6

A sequence can be empty. The empty sequence can be constructed with empty parentheses.

### The empty sequence

```

()
      
```

Result:

A sequence of just one item is considered the same as just this item. Whenever we say that an expression returns or takes one item, we really mean that it takes a singleton sequence of one item.

### A sequence of one item

```

("foo")
      
```

Result:foo

JSONiq classifies the items mentioned above in three categories:

* Atomic items: strings, numbers, booleans and nulls, but also many other supported atomic values such as dates, binary, etc.
* Structured items: objects and arrays.
* Function items: items that can take parameters and, upon evaluation, return sequences of items.

The JSONiq data model follows the [W3C specification](https://www.w3.org/TR/xpath-datamodel-30/#sequences), but, in core JSONiq, does not include XML nodes, and includes instead JSON objects and arrays. Engines are free, however, to optionally support XML nodes in addition to JSON objects and arrays.

### Atomic items <a href="#atomicitems.d12e1084" id="atomicitems.d12e1084"></a>

An atomic is a non-structured value that is annotated with a type.

JSONiq atomic values follow the [W3C specification](https://www.w3.org/TR/xpath-datamodel-30/#AtomicValue).

JSONiq supports most atomic values available in the [W3C specification](https://www.w3.org/TR/xpath-datamodel-30/#types-hierarchy). They are described in Chapter [The JSONiq type system](the-jsoniq-data-model.md#chapter-type-system). JSONiq furthermore defines an additional atomic value, _null_, with a type of its own, _jn:null_, which does not exist in the W3C specification.

In particular, JSONiq supports all core JSON values. Note that JSON numbers correspond to three different types in JSONiq.

* _string_: all JSON strings.
* _integer_: all JSON numbers that are integers (no dot, no exponent), infinite range.
* _decimal_: all JSON numbers that are decimals (no exponent), infinite range.
* _double_: IEEE double-precision 64-bit floating point numbers (corresponds to JSON numbers with an exponent).
* _boolean_: the JSON booleans true and false.
* _null_: the JSON null.

### Structured items <a href="#structureditems.d12e1164" id="structureditems.d12e1164"></a>

Structured items in JSONiq do not follow the XQuery 3.1 standard but are specific to JSONiq.

In JSONiq, an object represents a JSON object, i.e., a collection of string/items pairs.

Objects have the following property:

*   pairs. A set of pairs. Each pair consists of an atomic value of type _xs:string_ and of an item.

    \[ Consistency constraint: no two pairs have the same name (using fn:codepoint-equal). ]

The XQuery data model uses accessors to explain the data model. Accessors are not exposed to the user and are only used for convenience in this specification. Objects have the following accessors:

* _jdm:object-keys($o as js:object) as xs:string\*_: returns all keys in the object $o.
* _jdm:object-value($o as js:object, $s as xs:string) as js:item_: returns the value associated with $s in the object $o.

An object does not have a typed value.

In JSONiq, an array represents a JSON array, i.e., a ordered list of items.

Objects have the following property:

* members. An ordered list of items.

Arrays have the following accessors:

* _jdm:array-size($a as js:array) as xs:nonNegativeInteger_: returns the number of values in the array $a.
* _jdm:array-value($a as js:array, $i as xs:positiveInteger) as js:item_: returns the value at position $i in the array $a..

An array does not have a typed value.

Unlike in the XQuery 3.1 standard, the values in arrays and objects are single items (which disallows the empty sequence or a sequence of more than one item). Also, object keys must be strings (which disallows any other atomic value).

### Function items <a href="#section-function-items" id="section-function-items"></a>

JSONiq also supports function items, also known as higher-order functions. A function item can be passed parameters and evaluated.

A function item has an optional name and an arity. It also has a signature, which consists of the sequence type of each one of its parameters (as many as its arity), and the sequence type of the values it returns.

The fact that functions are items means that they can be returned by expressions, and passed as parameters to other functions. This is why they are also often called higher-order functions.

JSONiq function items follow the [W3C specification](https://www.w3.org/TR/xpath-datamodel-30/#function-items).
