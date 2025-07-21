# JSONiq 1.0

## Introduction <a href="#chapter-introduction" id="chapter-introduction"></a>

In this specification, we detail the JSONiq language in version 1.0. Historically, JSONiq was first created as an extension to XQuery. Later, a separate core syntax was created which makes it 100% tailored for JSON. It is the JSONiq core syntax that is detailed in this document.

The functionality directly inherited from XQuery is described on a higher level and we explicitly refer for more in-depth details to the [W3C specification](https://www.w3.org/TR/xquery-30).

### Structure of a JSONiq program. <a href="#structureofajsoniqprogram.d12e158" id="structureofajsoniqprogram.d12e158"></a>

A JSONiq program can either be a main module, which contains a query that can be executed, or a library module, which defines functions and variables that can be used in other modules.

A main or library module can be optionally prefixed with a JSONiq declaration with a version (currently 1.0) and an encoding.

Module

![](../.gitbook/assets/Module.png)

### Main modules <a href="#mainmodules.d12e180" id="mainmodules.d12e180"></a>

A JSONiq main module is made of two parts: an optional prolog, and an expression, which is the main query.

MainModule

![](../.gitbook/assets/MainModule.png)

The result of the main JSONiq program is the result of its main query.

In the prolog, it is possible to declare global variables and functions. Mostly, you will recognize a prolog declaration by the semi-colon it ends with. The main query does not contain semi-colons (at least in core JSONiq).

Global variables and functions can use and call each other arbitrarily, even if the dependency is further down in the prolog. If there a cycle, an error is thrown.

JSONiq largely follows the W3C standard regarding modules. The detailed specification is found [here](https://www.w3.org/TR/xquery-30/#id-query-prolog).

### Library modules <a href="#librarymodules.d12e213" id="librarymodules.d12e213"></a>

Library modules do not contain any main query, just global variables and functions. They can be imported by other modules.

A library module is introduced with a module declaration, followed by the prolog containing its variables and functions.

LibraryModule

![](../.gitbook/assets/LibraryModule.png)

### Feature matrix <a href="#featurematrix.d12e236" id="featurematrix.d12e236"></a>

JSONiq is 99% reliant on XQuery, a W3C standard. For everything taken over from the W3C standard, a brief, non-normative explanation is provided with a link to the corresponding part in the W3C specification.

| Feature                                  | Specification status                                                                                                                                                 |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JSONiq Data Model                        |                                                                                                                                                                      |
| Atomic items                             | W3C-conformant                                                                                                                                                       |
| Structured items                         | JSONiq-specific                                                                                                                                                      |
| Function items                           | W3C-conformant                                                                                                                                                       |
| Node items (XML)                         | Omitted (optional support by some engines)                                                                                                                           |
| JSONiq Type System                       |                                                                                                                                                                      |
| Atomic types                             | W3C-conformant, but support for xs:ID, xs:IDREF, xs:IDREFS, xs:Name, xs:NCName, xs:ENTITY, xs:ENTITIES, xs:NOTATION omitted (except for engines also supporting XML) |
| js:null type                             | JSONiq-specific                                                                                                                                                      |
| js:item, js:atomic types                 | JSONiq-specific synonyms for item() and xs:anyAtomicType                                                                                                             |
| Structured types                         | JSONiq-specific                                                                                                                                                      |
| Function types                           | W3C-conformant                                                                                                                                                       |
| Empty sequence type                      | JSONiq-specific notation () for empty-sequence()                                                                                                                     |
| XML node types                           | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Concepts                                 |                                                                                                                                                                      |
| Effective boolean value                  | W3C-conformant, extended with object, array and null semantics                                                                                                       |
| Atomization                              | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Expressions                              |                                                                                                                                                                      |
| Numeric literals                         | W3C-conformant                                                                                                                                                       |
| String literals                          | W3C-conformant, but escape is done with \ not with &                                                                                                                 |
| Boolean and null literals                | JSONiq-specific                                                                                                                                                      |
| Variable reference                       | W3C-conformant                                                                                                                                                       |
| Parenthesized expressions                | W3C-conformant                                                                                                                                                       |
| Context item expressions                 | W3C-conformant but \$$ syntax instead of .                                                                                                                           |
| Static function calls                    | W3C-conformant                                                                                                                                                       |
| Named function reference                 | W3C-conformant                                                                                                                                                       |
| Inline function expressions              | W3C-conformant                                                                                                                                                       |
| Filter expressions                       | W3C-conformant                                                                                                                                                       |
| Dynamic function calls                   | W3C-conformant                                                                                                                                                       |
| Path expressions (XML)                   | Omitted (optional support by engines supporting XML, but relative paths must start with ./)                                                                          |
| Object lookup                            | JSONiq-specific                                                                                                                                                      |
| Array lookup                             | JSONiq-specific                                                                                                                                                      |
| Array unboxing                           | JSONiq-specific                                                                                                                                                      |
| Sequence expressions                     | W3C-conformant                                                                                                                                                       |
| Arithmetic expressions                   | W3C-conformant, no atomization needed (except for engines also supporting XML)                                                                                       |
| String concatenation expressions         | W3C-conformant                                                                                                                                                       |
| Comparison expressions                   | W3C-conformant, no need to atomize or convert from untyped and untypedAtomic (except for engines also supporting XML)                                                |
| Logical expressions                      | W3C-conformant                                                                                                                                                       |
| XML constructors                         | Omitted (optional support by engines supporting XML)                                                                                                                 |
| JSON (object and array) constructors     | JSONiq-specific                                                                                                                                                      |
| FLWOR expressions                        | W3C-conformant                                                                                                                                                       |
| Unordered and ordered expressions        | W3C-conformant                                                                                                                                                       |
| Conditional expressions                  | W3C-conformant                                                                                                                                                       |
| Switch expressions                       | W3C-conformant                                                                                                                                                       |
| Quantified expressions                   | W3C-conformant                                                                                                                                                       |
| Try-catch expressions                    | W3C-conformant                                                                                                                                                       |
| Instance-of expressions                  | W3C-conformant                                                                                                                                                       |
| Typeswitch expressions                   | W3C-conformant                                                                                                                                                       |
| Cast expressions                         | W3C-conformant                                                                                                                                                       |
| Castable expressions                     | W3C-conformant                                                                                                                                                       |
| Constructor functions                    | W3C-conformant, additional constructor function for null()                                                                                                           |
| Treat expressions                        | W3C-conformant                                                                                                                                                       |
| Simple map operator                      | W3C-conformant                                                                                                                                                       |
| Validate expressions                     | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Extension expressions                    | W3C-conformant                                                                                                                                                       |
| Static context                           |                                                                                                                                                                      |
| XPath 1.0 compatibility mode             | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Statically known namespaces              | W3C-conformant                                                                                                                                                       |
| Default element/type namespace           | W3C-conformant, strong recommendation for implementations to overwrite with the proxy namespace _http://jsoniq.org/default-type-namespace_ to omit prefixes.         |
| Default function namespace               | W3C-conformant, strong recommendation for implementations to overwrite with _http://jsoniq.org/default-function-namespace_ to omit prefixes.                         |
| In-scope schema definitions              | Omitted (optional support by engines supporting XML)                                                                                                                 |
| In-scope variables                       | W3C-conformant                                                                                                                                                       |
| Context item static type                 | W3C-conformant                                                                                                                                                       |
| Statically known function signatures     | W3C-conformant, augmented with all JSONiq builtin functions                                                                                                          |
| Statically known collations              | W3C-conformant                                                                                                                                                       |
| Default collation                        | W3C-conformant                                                                                                                                                       |
| Construction mode                        | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Ordering mode                            | W3C-conformant                                                                                                                                                       |
| Default order for empty sequences        | W3C-conformant                                                                                                                                                       |
| Boundary-space policy                    | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Copy-namespaces mode                     | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Static Base URI                          | W3C-conformant                                                                                                                                                       |
| Statically known documents               | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Statically known collections             | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Statically known default collection type | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Statically known decimal formats         | W3C-conformant                                                                                                                                                       |
| Dynamic context                          |                                                                                                                                                                      |
| Context item                             | W3C-conformant (but with syntax \$$ not .)                                                                                                                           |
| Initial context item                     | W3C-conformant                                                                                                                                                       |
| Context position                         | W3C-conformant                                                                                                                                                       |
| Context size                             | W3C-conformant                                                                                                                                                       |
| Variable values                          | W3C-conformant                                                                                                                                                       |
| Named functions                          | W3C-conformant                                                                                                                                                       |
| Current dateTime                         | W3C-conformant                                                                                                                                                       |
| Implicit timezone                        | W3C-conformant                                                                                                                                                       |
| Default language                         | W3C-conformant                                                                                                                                                       |
| Default calendar                         | W3C-conformant                                                                                                                                                       |
| Default place                            | W3C-conformant                                                                                                                                                       |
| Available documents                      | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Available text resources                 | W3C-conformant                                                                                                                                                       |
| Available node collections               | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Default node collection                  | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Available resource collections           | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Default resource collection              | Omitted (optional support by engines supporting XML)                                                                                                                 |
| Environment variables                    | W3C-conformant                                                                                                                                                       |

### Namespaces <a href="#namespaces.d12e941" id="namespaces.d12e941"></a>

The namespace _http://jsoniq.org/functions_ is used for JSONiq builtin functions defined by this specification. This namespace is exposed to the user and is bound by default to the prefix _jn_. For instance, the function name _jn:keys()_ is in this namespace.

The namespace _http://jsoniq.org/types_ is used for JSONiq builtin types defined by this specification (including synonyms for some XQuery types). This namespace is exposed to the user and is bound by default to the prefix _js_. For instance, the type name _js:null_ is in this namespace.

The namespace _http://jsoniq.org/default-function-namespace_ is a proxy namespace that maps to the _jn:_ (JSONiq), _fn:_ (XQuery) and _math:_ (XQuery) namespaces. It is the default function namespace, allowing to call all these functions with no prefix.

The namespace _http://jsoniq.org/default-type-namespace_ is a proxy namespace that maps to the _js:_ (JSONiq) and _xs:_ (XQuery) namespaces. It is the default type namespace, allowing to use all builtin types with no prefix.

Accessors used in JSONiq Data Model use the _jdm:_ prefix. These functions are not exposed to the user and are for explanatory purposes of the data model within this document only. The _jdm:_ prefix is not associated with a namespace.

## The JSONiq data model <a href="#chapter-data-model" id="chapter-data-model"></a>

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

JSONiq supports most atomic values available in the [W3C specification](https://www.w3.org/TR/xpath-datamodel-30/#types-hierarchy). They are described in Chapter [The JSONiq type system](jsoniq-specification.md#chapter-type-system). JSONiq furthermore defines an additional atomic value, _null_, with a type of its own, _jn:null_, which does not exist in the W3C specification.

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

## Input datasets <a href="#chapter-collections" id="chapter-collections"></a>

Even though you can build your own JSON values with JSONiq by copying-and-pasting JSON documents, most of the time, your JSON data will come from an external input dataset.

How this dataset is access depends on the JSONiq implementation and of the context. Some engines can read the data from a file located on a file system, local or distributed (HDFS, S3); some others get data from the Web; some others are full-fledged datastores and have collections that can be created, queried, modified and persisted.

It is up to each engine to document which functions should be used, and how, in order to read datasets into a JSONiq Data Model instance. These functions will take implementation-defined parameters and typically return sequences of objects, or sequences of strings, or sequences of items, etc.

For the purpose of examples given in this specification, we assume that a hypothetical engine has collections that are sequences of objects, identified by a name which is a string. We assume that there is a collection() function that returns all objects associated with the provided collection name.

We assume in particular that there are three example collections, shown below.

### Collection 1

```

collection("one-object")
    
```

Result

```
{ "foo" : "bar" }
```

### Collection 2

```

collection("captains")
    
```

Result

```

{ "name" : "James T. Kirk", "series" : [ "The original series" ], "century" : 23 }
{ "name" : "Jean-Luc Picard", "series" : [ "The next generation" ], "century" : 24 }
{ "name" : "Benjamin Sisko", "series" : [ "The next generation", "Deep Space 9" ], "century" : 24 }
{ "name" : "Kathryn Janeway", "series" : [ "The next generation", "Voyager" ], "century" : 24  }
{ "name" : "Jonathan Archer", "series" : [ "Entreprise" ], "century" : 22 }
{ "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : [ "Voyager" ], "century" : 24 }
{ "name" : "Samantha Carter", "series" : [ ], "century" : 21 }
      
```

### Collection 3

```
collection("films")
```

Result

```

{ "id" : "I", "name" : "The Motion Picture", "captain" : "James T. Kirk" }
{ "id" : "II", "name" : "The Wrath of Kahn", "captain" : "James T. Kirk" }
{ "id" : "III", "name" : "The Search for Spock", "captain" : "James T. Kirk" }
{ "id" : "IV", "name" : "The Voyage Home", "captain" : "James T. Kirk" }
{ "id" : "V", "name" : "The Final Frontier", "captain" : "James T. Kirk" }
{ "id" : "VI", "name" : "The Undiscovered Country", "captain" : "James T. Kirk" }
{ "id" : "VII", "name" : "Generations", "captain" : [ "James T. Kirk", "Jean-Luc Picard" ] }
{ "id" : "VIII", "name" : "First Contact", "captain" : "Jean-Luc Picard" }
{ "id" : "IX", "name" : "Insurrection", "captain" : "Jean-Luc Picard" }
{ "id" : "X", "name" : "Nemesis", "captain" : "Jean-Luc Picard" }
{ "id" : "XI", "name" : "Star Trek", "captain" : "Spock" }
{ "id" : "XII", "name" : "Star Trek Into Darkness", "captain" : "Spock" }
      
```

## The JSONiq type system <a href="#chapter-type-system" id="chapter-type-system"></a>

This section describes JSONiq types as well as the sequence type syntax.

JSONiq manipulates semi-structured data: in general, JSONiq allows you, but does not require you to specify types. So you have as much or as little type verification as you wish.

JSONiq is still strongly typed, so that you will be told if there is a type inconsistency or mismatch in your programs.

Whenever you do not specify the type of a variable or the type signature of a function, the most general type for any sequence of items, item\*, is assumed.

Section [Expressions dealing with types](jsoniq-specification.md#section-type-expressions) introduces expressions which work with values of these types, as well as type operations (variable types, casts, ...).

### Sequence types <a href="#sequencetypes.d12e1331" id="sequencetypes.d12e1331"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-sequencetype-syntax) regarding sequence occurrence indicators. The following explanations, provided as an informal summary for convenience, are non-normative.

A sequence is an ordered list of items.

All sequences match the sequence type _js:item\*_.

A sequence type is made of an item type followed by an occurrence indicator:

* \* stands for a sequence of any length (zero or more)
*
  * stands for a non-empty sequence (one or more)
* ? stands for an empty or a singleton sequence (zero or one)
* The absence of indicator stands for a singleton sequence (one).

Examples:

* string matches any singleton sequence containing a string.
* item+ matches any non-empty sequence.
* object? matches the empty sequence and any sequence containing one object.

JSONiq defines the syntax _()_ for the empty sequence, rather than _empty-sequence()_.

SequenceType

![](../.gitbook/assets/SequenceType.png)

### Item types <a href="#itemtypes.d12e1415" id="itemtypes.d12e1415"></a>

Item types are the first component of a sequence type, together with the cardinality indicator. Thus, an item type matches (or not) a single item. For example, "foo" matches the item type xs:string.

There are three categories of item types:

* Atomic types (W3C-conformant, additional js:null and js:atomic)
* Structured types (JSONiq-specific)
* Function types (W3C-conformant)

JSONiq uses a JSONiq-specific, implementation-defined default type namespace that acts as a proxy namespace to all types (xs: or js:). As a consequence, buitin atomic types do not need to be prefixed in the JSONiq syntax (_integer_ instead of _xs:integer_, _null_ instead of _js:null_).

All items match the item type _js:item_, which is a JSONiq-specific synonym for the W3C-confirmant _item()_.

ItemType

![](../.gitbook/assets/ItemType.png)

#### Atomic types <a href="#atomictypes.d12e1473" id="atomictypes.d12e1473"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-types) for atomic types except for modifications in the list of available atomic types and a simplified syntax for xs:anyAtomicType. The following explanations, provided as an informal summary for convenience, are non-normative.

Atomic types are organized in a tree hierarchy.

JSONiq defines the following build-in types that have a direct relation with JSON:

*   _xs:string_: the value space is all strings made of Unicode characters.

    All string literals build an atomic which matches string.
*   _xs:integer_ (W3C-conformant): the value space is that of all mathematical integral numbers (N), with an infinite range. This is a subtype of _decimal_, so that all integers also match the item type _decimal_.

    All integer literals build an atomic which matches integer.
*   _xs:decimal_ (W3C-conformant): the value space is that of all mathematical decimal numbers (D), with an infinite range.

    All decimal literals build an atomic which matches decimal.
*   _xs:double_ (W3C-conformant): the value space is that of all IEEE double-precision 64-bit floating point numbers.

    All double literals build an atomic which matches double.
*   _xs:boolean_ (W3C-conformant): the value space contains the booleans true and false.

    All boolean literals build an atomic which matches boolean.
*   _js:null_ (JSONiq-specific): the value space is a singleton and only contains null.

    All null literals build an atomic which matches null.
*   _js:atomic_ (JSONiq-specific synonym of, and W3C-conformant with, _xs:anyAtomicType_): all atomic types.

    All literals build an atomic which matches atomic.

JSONiq also supports further atomic types, which are conformant with [XML Schema](http://www.w3.org/TR/xmlschema11-2/#built-in-datatypes).

These datatypes are already used as a set of atomic datatypes by the other two semi-structured data formats of the Web: XML and RDF, as well as by the corresponding query languages: XQuery and SPARQL, so it is natural for a complete JSON data model to reuse them.

* Further number types: xs:float, xs:long, xs:int, xs:short, xs:byte, xs:float, xs:positiveInteger, xs:negativeInteger, xs:nonPositiveInteger, xs:nonNegativeInteger, xs:unsignedLong, xs:unsignedInt, xs:unsignedShort, xs:unsignedByte.
* Date or time types: xs:date, xs:dateTime, xs:dateTimeStamp, xs:gDay, xs:gMonth, xs:gMonthDay, xs:gYear, xs:xs:gYearMonth, xs:time.
* Duration types: xs:duration, xs:dayTimeDuration, xs:yearMonthDuration.
* Binary types: xs:base64Binary, xs:hexBinary.
* An URI type: xs:anyURI.

The support of xs:ID, xs:IDREF, xs:IDREFS, xs:NOTATION, xs:Name, xs:NCName, xs:NMTOKEN, xs:NMTOKENS, xs:ENTITY, xs:ENTITIES is not required by JSONiq, although engines that also support XML can support them.

AtomicType

![](../.gitbook/assets/AtomicType.png)

#### Structured types <a href="#structuredtypes.d12e1620" id="structuredtypes.d12e1620"></a>

JSONiq introduces four more types for matching objects and arrays. Like atomic types, they do not need the _js:_ prefix in the syntax (_object_ instead of _js:object_, etc.).

All objects match the item type _js:object_.

All arrays match the item type _js:array_.

All objects and arrays match the item type _js:json-item_.

For engines that also support optionally XML, _js:structured-item_ matches both XML nodes and JSON objects and arrays.

StructuredType

![](../.gitbook/assets/StructuredType.png)

#### Function types <a href="#functiontypes.d12e1668" id="functiontypes.d12e1668"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-function-test) regarding function types. The following explanations are non-normative.

FunctionType

![](../.gitbook/assets/FunctionType.png)

AnyFunctionType

![](../.gitbook/assets/AnyFunctionType.png)

TypedFunctionType

![](../.gitbook/assets/TypedFunctionType.png)

## Expressions <a href="#chapter-expressions" id="chapter-expressions"></a>

### Construction of items <a href="#chapter-construction" id="chapter-construction"></a>

In JSONiq, objects, arrays and basic atomic values (string, number, boolean, null) are constructed exactly as they are constructed in JSON. Any JSON document is also a valid JSONiq query which just "returns itself".

Because JSONiq expressions are fully composable, however, in objects and arrays constructors, it is possible to put any JSONiq expression and not only atomic literals, object constructors and array constructors. Furthermore, JSONiq supports the construction of other W3C-standardized builtin types (date, hexBinary, etc).

The following examples are a few of many operators available in JSONiq: "to" for creating arithmetic sequences, "||" for concatenating strings, "+" for adding numbers, "," for appending sequences.

In an array, the operand expression will evaluated to a sequence of items, and these items will be copied and become members of the newly created array.

#### Composable array constructors

```

  [ 1 to 10 ]
    
```

Result:\[ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]

In an object, the expression you use for the key must evaluate to an atomic - if it is not a string, it will just be cast to it.

#### Composable object keys

```

  { "foo" || "bar" : true }
      
```

Result:{ "foobar" : true }

An error is raised if the key expressions is not an atomic.

#### Non-atomic object keys

```

  { [ 1, 2 ] : true }
      
```

Result:An error was raised: can not atomize an array item: an array has probably been passed where an atomic value is expected (e.g., as a key, or to a function expecting an atomic item)

If the value expression is empty, null will be used as a value, and if it contains two items or more, they will be wrapped into an array.

If the colon is preceded with a question mark, then the pair will be omitted if the value expression evaluates to the empty sequence.

#### Composable object values

```

  { "foo" : 1 + 1 }
      
```

Result:{ "foo" : 2 }

#### Composable object values and automatic conversion

```

  { "foo" : (), "bar" : (1, 2) }
      
```

Result:{ "foo" : null, "bar" : \[ 1, 2 ] }

#### Optional pair (not implemented yet in Zorba)

```

  { "foo" ?: (), "bar" : (1, 2) }
      
```

Result:An error was raised: invalid expression: syntax error, unexpected "?", expecting "end of file" or "," or "}"

The {| |} syntax can be used to merge several objects.

#### Merging object constructor

```

  {| { "foo" : "bar" }, { "bar" : "foo" } |}
      
```

Result:{ "foo" : "bar", "bar" : "foo" }

An error is raised if the operand expression does not evaluate to a sequence of objects.

#### Merging object constructor with a type error

```

  {| 1 |}
      
```

Result:An error was raised: xs:integer can not be treated as type object()\*

#### Numbers <a href="#numbers.d12e1832" id="numbers.d12e1832"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-literals) for constructing numbers. The following explanations, provided as an informal summary for convenience, are non-normative.

Literal

![](../.gitbook/assets/Literal.png)

NumericLiteral

![](../.gitbook/assets/NumericLiteral.png)

IntegerLiteral

![](../.gitbook/assets/IntegerLiteral.png)

DecimalLiteral

![](../.gitbook/assets/DecimalLiteral.png)

DoubleLiteral

![](../.gitbook/assets/DoubleLiteral.png)

The syntax for creating numbers is identical to that of JSON (it is actually a more flexible superset, for example leading 0s are allowed, and a decimal literal can begin with a dot). Note that JSONiq distinguishes between integers (no dot, no scientific notation), decimals (dot but no scientific notation) and doubles (scientific notation). As expected, an integer literal creates an atomic of type integer, and so on.

**Integer literals**

```

42
      
```

Result:42

**Decimal literals**

```

3.14
      
```

Result:3.14

**Double literals**

```

+6.022E23
      
```

Result:6.022E23

#### Strings <a href="#strings.d12e1941" id="strings.d12e1941"></a>

The syntax for creating string items is conformant to [JSON](https://www.ecma-international.org/publications/standards/Ecma-404.htm) rather than to the W3C standard for string literals. This means concretely that escaping is done with backslashes and not with ampersands. Also, like in JSON, double quotes are required and single quotes are forbidden.

StringLiteral

![](../.gitbook/assets/StringLiteral.png)

**String literals**

```

  "foo"
        
```

Result:foo

**String literals with escaping**

```

  "This is a line\nand this is a new line"
        
```

Result:This is a line and this is a new line

**String literals with Unicode character escaping**

```

  "\u0001"
        
```

Result:\&#x1;

**String literals with a nested quote**

```

  "This is a nested \"quote\""
        
```

Result:This is a nested "quote"

#### Booleans and null <a href="#booleansandnull.d12e2006" id="booleansandnull.d12e2006"></a>

JSONiq also introduces three more literals for constructing booleans and nulls: true, false and null. This makes in particular the functions true() and false() superfluous.

BooleanLiteral

![](../.gitbook/assets/BooleanLiteral.png)

NullLiteral

![](../.gitbook/assets/NullLiteral.png)

**Boolean literals (true)**

```

true
      
```

Result:true

**Boolean literals (false)**

```

false
      
```

Result:false

**Null literals**

```

null
      
```

Result:null

#### Other atomic values <a href="#otheratomicvalues.d12e2070" id="otheratomicvalues.d12e2070"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-constructor-functions) for constructing most atomic values with constructors. In JSONiq, the _xs_ prefix is optional.

#### Objects <a href="#objects.d12e2084" id="objects.d12e2084"></a>

Expressions constructing objects are JSONiq-specific and introduced in this specification.

ObjectConstructor

![](../.gitbook/assets/ObjectConstructor.png)

PairConstructor

![](../.gitbook/assets/PairConstructor.png)

The syntax for creating objects is identical to that of JSON. You can use for an object key any string literal, and for an object value any literal, object constructor or array constructor.

**Empty object constructors**

```

{}
      
```

Result:{ }

**Object constructors 1**

```

{ "foo" : "bar" }
      
```

Result:{ "foo" : "bar" }

**Object constructors 2**

```

{ "foo" : [ 1, 2, 3, 4, 5, 6 ] }
      
```

Result:{ "foo" : \[ 1, 2, 3, 4, 5, 6 ] }

**Object constructors 3**

```

{ "foo" : true, "bar" : false }
      
```

Result:{ "foo" : true, "bar" : false }

**Nested object constructors**

```

{ "this is a key" : { "value" : "a value" } }
      
```

Result:{ "this is a key" : { "value" : "a value" } }

As in JavaScript, if your key is simple enough (like alphanumerics, underscores, dashes, this kind of things), the quotes can be omitted. The strings for which quotes are not mandatory are called NCNames. This class of strings can be used for unquoted keys, for variable and function names, and for module aliases.

**Object constructors with unquoted key 1**

```

{ foo : "bar" }
      
```

Result:{ "foo" : "bar" }

**Object constructors with unquoted key 2**

```

{ foo : [ 1, 2, 3, 4, 5, 6 ] }
      
```

Result:{ "foo" : \[ 1, 2, 3, 4, 5, 6 ] }

**Object constructors with unquoted key 3**

```

{ foo : "bar", bar : "foo" }
      
```

Result:{ "foo" : "bar", "bar" : "foo" }

**Object constructors with needed quotes around the key**

```

{ "but you need the quotes here" : null }
    
```

Result:{ "but you need the quotes here" : null }

Objects can be constructed more dynamically (e.g., dynamic keys) by constructing and merging smaller objects. Duplicate key names throw an error.

**Object constructors with needed quotes around the key**

```

{|
  for $i in 1 to 3
  return { "foo" || $i : $i }
|}
    
```

Result:{ "foo1" : 1, "foo2" : 2, "foo3" : 3 }

#### Arrays <a href="#arrays.d12e2225" id="arrays.d12e2225"></a>

Expressions constructing arrays are JSONiq-specific and introduced in this specification.

ArrayConstructor

![](../.gitbook/assets/ArrayConstructor.png)

Expr

![](../.gitbook/assets/Expr.png)

The syntax for creating arrays is identical to that of JSON: square brackets, comma separated literals, object constructors and arrays constructors.

**Empty array constructors**

```

[]
      
```

Result:\[ ]

**Array constructors**

```

[ 1, 2, 3, 4, 5, 6 ]
      
```

Result:\[ 1, 2, 3, 4, 5, 6 ]

**Nested array constructors**

```

[ "foo", 3.14, [ "Go", "Boldly", "When", "No", "Man", "Has", "Gone", "Before" ], { "foo" : "bar" }, true, false, null ]
      
```

Result:\[ "foo", 3.14, \[ "Go", "Boldly", "When", "No", "Man", "Has", "Gone", "Before" ], { "foo" : "bar" }, true, false, null ]

Square brackets are mandatory. Do not push it.

#### Functions <a href="#functions.d12e2293" id="functions.d12e2293"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-inline-func) for constructing function items with inline expressions or [named function references](https://www.w3.org/TR/xquery-30/#id-named-function-ref). The following explanations, provided as an informal summary for convenience, are non-normative.

Function items can be constructed in two ways: by definining its body directly (inline function expression), or by referring by name to a function declared in a prolog.

FunctionItemExpr

![](../.gitbook/assets/FunctionItemExpr.png)

**Inline function expression**

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-inline-func) for constructing function items with inline expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

A function can be built directly by specifying its parameters and its body as expression. Types are optional and by default, assumed to be item\*.

Function items can also be produced with a partial function application.

**Inline function expression**

```

           function ($x as integer, $y as integer) as integer { $x + 2 },
           function ($x) { $x + 2 }
       
```

Result(two function items)

InlineFunctionExpr

![](../.gitbook/assets/InlineFunctionExpr.png)

ParamList

![](../.gitbook/assets/ParamList.png)

**Named function reference**

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-named-function-ref) for constructing function items with named function references. The following explanations, provided as an informal summary for convenience, are non-normative.

If a function is builtin or declared in a prolog, in the same module or imported, then it is also possible to build a function item by referring to its name and arity.

**Named function reference**

```

           declare function local:sum($x as integer, $y as integer) as integer
           {
             $x + 2
           };
           local:sum#2
       
```

Result(a function items)

NamedFunctionRef

![](../.gitbook/assets/NamedFunctionRef.png)

### Manipulating atomic values <a href="#chapter-basic-operations" id="chapter-basic-operations"></a>

We now introduce the expressions that manipulate atomic values: arithmetics, logics, comparison, string concatenation.

#### Arithmetics <a href="#arithmetics.d12e2420" id="arithmetics.d12e2420"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-arithmetic) for arithmetic expressions, and naturally extends to return errors for null values. The following explanations, provided as an informal summary for convenience, are non-normative.

JSONiq supports the basic four operations, integer division and modulo.

Multiplicative operations have precedence over additive operations. Parentheses can override it.

**Basic arithmetic operations with precedence override**

```

1 * ( 2 + 3 ) + 7 idiv 2 - (-8) mod 2
      
```

Result (run with Zorba):8

Dates, times and durations are also supported in a natural way.

**Using basic operations with dates.**

```

date("2013-05-01") - date("2013-04-02")
      
```

Result (run with Zorba):P29D

If any of the operands is a sequence of more than one item, an error is raised.

**Sequence of more than one number in an addition**

```

(1, 2) + 3
      
```

Result (run with Zorba):An error was raised: sequence of more than one item can not be promoted to parameter type xs:anyAtomicType? of function add()

If any of the operands is not a number, a date, a time or a duration, an error is raised, which seamlessly includes raising errors for null with no need to extend the specification.

**Null in an addition**

```

1 + null
      
```

Result (run with Zorba):An error was raised: arithmetic operation not defined between types "xs:integer" and "js:null"

If one of the operands evaluates to the empty sequence, then the operation results in the empty sequence.

If the two operands do not have the same number type, JSONiq will do the adequate conversions.

**Basic arithmetic operations with an empty sequence**

```

() + 2
      
```

Result (run with Zorba):

AdditiveExpr

![](../.gitbook/assets/AdditiveExpr.png)

MultiplicativeExpr

![](../.gitbook/assets/MultiplicativeExpr.png)

UnaryExpr

![](../.gitbook/assets/UnaryExpr.png)

#### String concatenation <a href="#stringconcatenation.d12e2536" id="stringconcatenation.d12e2536"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-string-concat-expr) for string concatenation. The following explanations, provided as an informal summary for convenience, are non-normative.

Two strings or more can be concatenated using the concatenation operator.

**String concatenation**

```

"Captain" || " " || "Kirk"
      
```

Result (run with Zorba):Captain Kirk

An empty sequence is treated like an empty string.

**String concatenation with the empty sequence**

```

"Captain" || () || "Kirk"
      
```

Result (run with Zorba):CaptainKirk

StringConcatExpr

![](../.gitbook/assets/StringConcatExpr.png)

#### Comparison <a href="#comparison.d12e2585" id="comparison.d12e2585"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-comparisons) for comparison, and only extends its semantics to null values as follows.

null can be compared for equality or inequality to anything - it is only equal to itself so that false is returned when comparing if for equality with any non-null atomic. True is returned when comparing it with non-equality with any non-null atomic.

**Equality and non-equality comparison with null**

```

1 eq null, "foo" ne null, null eq null
      
```

Result (run with Zorba):false true true

For ordering operators (lt, le, gt, ge), null is considered the smallest possible value (like in JavaScript).

**Ordering comparison with null**

```

1 lt null
      
```

Result (run with Zorba):false

The following explanations, provided as an informal summary for convenience, are non-normative.

ComparisonExpr

![](../.gitbook/assets/ComparisonExpr.png)

Atomics can be compared with the usual six comparison operators (equality, non-equality, lower-than, greater-than, lower-or-equal, greater-or-equal), and with the same two-letter symbols as in MongoDB.

**Equality comparison**

```

1 + 1 eq 2, 1 lt 2
      
```

Result (run with Zorba):true true

Comparison is only possible between two compatible types, otherwise, an error is raised.

**Comparisons with a type mismatch**

```

"foo" eq 1
      
```

Result (run with Zorba):An error was raised: "xs:string": invalid type: can not compare for equality to type "xs:integer"

Like for arithmetic operations, if an operand is the empty sequence, the empty sequence is returned as well.

**Comparison with the empty sequence**

```

() eq 1
      
```

Result (run with Zorba):

Comparisons and logic operators are fundamental for a query language and for the implementation of a query processor as they impact query optimization greatly. The current comparison semantics for them is carefully chosen to have the right characteristics as to enable optimization.

#### Logics <a href="#logics.d12e2678" id="logics.d12e2678"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-logical-expressions) for logical expressions; it introduces a prefix unary not operator as a synonym for fn:not, and extends the semantics of effective boolean values to objects, arrays and nulls. The following explanations, provided as an informal summary for convenience, are non-normative.

OrExpr

![](../.gitbook/assets/OrExpr.png)

AndExpr

![](../.gitbook/assets/AndExpr.png)

NotExpr

![](../.gitbook/assets/NotExpr.png)

JSONiq logics support is based on two-valued logics: just true and false.

Non-boolean operands get automatically converted to either true or false, or an error is raised. The boolean() function performs a manual conversion.

* An empty sequence is converted to false.
* A singleton sequence of one null is converted to false.
* A singleton sequence of one string is converted to true except the empty string which is converted to false.
* A singleton sequence of one number is converted to true except zero or NaN which are converted to false.
* An operand singleton sequence whose first item is an object or array is converted to true.
* Other operand sequences cannot be converted and an error is raised.

JSONiq supports the most famous three boolean operations: conjunction, disjunction and negation. Negation has the highest precedence, then conjunction, then disjunction. Parentheses can override.

**Logics with booleans**

```

true and ( true or not true )
      
```

Result (run with Zorba):true

**Logics with comparing operands**

```

1 + 1 eq 2 or 1 + 1 eq 3
      
```

Result (run with Zorba):true

**Conversion of the empty sequence to false**

```

boolean(())
      
```

Result (run with Zorba):false

**Conversion of null to false**

```

boolean(null)
      
```

Result (run with Zorba):false

**Conversion of a string to true**

```

boolean("foo"), boolean("")
      
```

Result (run with Zorba):true false

**Conversion of a number to false**

```

0 and true, not (not 1e42)
      
```

Result (run with Zorba):false true

**Conversion of an object to a boolean (not implemented in Zorba at this point)**

```

{ "foo" : "bar" } or false
      
```

Result (run with Zorba):true

If the input sequence has more than one item, and the first item is not an object or array, an error is raised.

**Error upon conversion of a sequence of more than one item, not beginning with a JSON item, to a boolean**

```

( 1, 2, 3 ) or false
      
```

Result (run with Zorba):An error was raised: invalid argument type for function fn:boolean(): effective boolean value not defined for sequence of more than one item that starts with "xs:integer"

Unlike in C++ or Java, you cannot rely on the order of evaluation of the operands of a boolean operation. The following query may return true or may return an error.

**Non-determinism in presence of errors.**

```

true or (1 div 0)
      
```

Result (run with Zorba):true

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-quantified-expressions) for quantified expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

QuantifiedExpr

![](../.gitbook/assets/QuantifiedExpr.png)

It is possible to perform a conjunction or a disjunction on a predicate for each item in a sequence.

**Universal quantifier**

```

every $i in 1 to 10 satisfies $i gt 0
      
```

Result (run with Zorba):true

**Existential quantifier on several variables**

```

some $i in -5 to 5, $j in 1 to 10 satisfies $i eq $j
      
```

Result (run with Zorba):true

Variables can be annotated with a type. If no type is specified, item\* is assumed. If the type does not match, an error is raised.

**Existential quantifier with type checking**

```

some $i as integer in -5 to 5, $j as integer in 1 to 10 satisfies $i eq $j
      
```

Result (run with Zorba):true

### Manipulating sequences <a href="#chapter-manipulating-sequences" id="chapter-manipulating-sequences"></a>

JSONiq can create sequences with concatenation (comma) or with a range. Parentheses can be used for overriding precedence.

#### Comma operator <a href="#commaoperator.d12e2926" id="commaoperator.d12e2926"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#construct_seq) for the concatenation of sequences with commas. The following explanations, provided as an informal summary for convenience, are non-normative.

Expr

![](../.gitbook/assets/Expr.png)

Use a comma to concatenate two sequences, or even single items. This operator has the lowest precedence of all.

**Comma**

```

1, 2, 3, 4, 5, 6, 7, 8, 9, 10
  
```

Result (run with Zorba):1 2 3 4 5 6 7 8 9 10

**Comma**

```

{ "foo" : "bar" }, [ 1 ]
  
```

Result (run with Zorba):{ "foo" : "bar" } \[ 1 ]

Sequences do not nest. You need to use arrays in order to nest.

#### Range operator <a href="#rangeoperator.d12e2983" id="rangeoperator.d12e2983"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#construct_seq) for range expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

RangeExpr

![](../.gitbook/assets/RangeExpr.png)

With the binary operator "to", you can generate larger sequences with just two integer operands.

**Range operator**

```

1 to 10
  
```

Result (run with Zorba):1 2 3 4 5 6 7 8 9 10

If one operand evaluates to the empty sequence, then the range operator returns the empty sequence.

**Range operator with the empty sequence**

```

() to 10, 1 to ()
  
```

Result (run with Zorba):

Otherwise, if an operand evaluates to something else than a single integer or an empty sequence, an error is raised.

**Range operator with a type inconsistency**

```

(1, 2) to 10
  
```

Result (run with Zorba):An error was raised: sequence of more than one item can not be promoted to parameter type xs:integer? of function to()

#### Parenthesized expression <a href="#parenthesizedexpression.d12e3056" id="parenthesizedexpression.d12e3056"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-paren-expressions) for parenthesized expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

ParenthesizedExpr

![](../.gitbook/assets/ParenthesizedExpr.png)

Use parentheses to override the precedence of expressions.

If the parentheses are empty, the empty sequence is produced.

**Empty sequence**

```

()
      
```

Result (run with Zorba):

### Calling functions <a href="#chapter-function-calls" id="chapter-function-calls"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-eval-function-call) for function calls. The following explanations, provided as an informal summary for convenience, are non-normative.

Function calls in JSONiq can either be made statically, with a named function, or dynamically, by passing a function item on the fly.

The syntax for function calls is similar to many other languages. JSONiq supports four sorts of functions:

* Builtin functions: these have no prefix and can be called without any import.
* Local functions: they are defined in the prolog, to be used in the main query. They have the prefix _local:_. Chapter [Prologs](jsoniq-specification.md#chapter-prolog) describes how to define your own local functions.
* Imported functions: they are defined in a library module. They have the prefix corresponding to the alias to which the imported module has been bound to. Chapter [Modules](jsoniq-specification.md#chapter-modules) describes how to define your own modules.
* Anonymous functions: they are defined on the fly, by inline function expressions or partial evaluation.

The first three are named functions and can be called statictically. All four can be called dynamically, as a named function can be also passed as an item with a named function reference.

#### Static function calls <a href="#staticfunctioncalls.d12e3148" id="staticfunctioncalls.d12e3148"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-function-calls) for static function calls. The following explanations, provided as an informal summary for convenience, are non-normative.

A static function call consists of the name of the function and of expressions returning its parameters. An error is thrown if no function with the corresponding name and arity is found.

**A builtin function call.**

```

       keys({ "foo" : "bar", "bar" : "foo" })
     
```

Result:foo bar

**A builtin function call.**

```

       concat("foo", "bar")
     
```

Result:foobar

An error is raised if the actual types do not match the expected types.

**A type error in a function call.**

```

       sum({ "foo" : "bar" })
     
```

Result:An error was raised: can not atomize an object item: an object has probably been passed where an atomic value is expected (e.g., as a key, or to a function expecting an atomic item)

JSONiq static function calls follow the [W3C specification](https://www.w3.org/TR/xquery-30/#id-function-calls).

FunctionCall

![](../.gitbook/assets/FunctionCall.png)

#### Dynamic function calls <a href="#dynamicfunctioncalls.d12e3213" id="dynamicfunctioncalls.d12e3213"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-dynamic-function-invocation) for dynamic function calls. The following explanations, provided as an informal summary for convenience, are non-normative.

A dynamic function call is a postfix expression. Its left-hand-side is an expression that must return a single function item (see in the data model [Function items](jsoniq-specification.md#section-function-items)). Its right-hand side is a list of parameters, each one of which is an arbitrary expression providing a sequence of items, one such sequence for each parameter.

**A dynamic function call.**

```

       let $f := function($x) { $x + 1 }
       return $f(2)
     
```

Result:3

If the number of parameters does not match the arity of the function, an error is raised. An error is also raised if an argument value does not match the corresponding type in the function signature.

Otherwise, the function is evaluated with the supplied parameters. If the result matches the return type of the function, it is returned, otherwise an error is raised.

**A dynamic function call with signature**

```

       let $f := function($x as integer) as integer { $x + 1 }
       return $f(2)
     
```

Result:3

JSONiq dynamic function calls follow the [W3C specification](https://www.w3.org/TR/xquery-30/#id-dynamic-function-invocation).

PostfixExpr

![](../.gitbook/assets/PostfixExpr.png)

ArgumentList

![](../.gitbook/assets/ArgumentList.png)

Argument

![](../.gitbook/assets/Argument.png)

#### Partial application <a href="#partialapplication.d12e3301" id="partialapplication.d12e3301"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#dt-partial-function-application) for partial application. The following explanations, provided as an informal summary for convenience, are non-normative.

A static or dynamic function call also have placeholder parameters, represented with a question mark in the syntax. When this is the case, the function call returns a function item that is the partial application of the original function, and its arity is the number of remaining placeholders.

**A partial application.**

```

       let $f := function($x as integer, $y as integer) as integer { $x + $y }
       let $g := $f(?, 2)
       return $g(2)
     
```

Result:4

JSONiq dynamic function calls follow the [W3C specification](https://www.w3.org/TR/xquery-30/#dt-partial-function-application).

### Navigating objects <a href="#chapter-selectors" id="chapter-selectors"></a>

Like in JavaScript, it is possible to navigate through objects and arrays. This is a specific JSONiq extension.

JSONiq also allows to filter sequences with a predicate and predicates are fully W3C-conformant.

JSONiq supports filtering items from a sequence, looking up the value associated with a given key in an object, looking up the item at a given position in an array, and looking up all items in an array.

PostfixExpr

![](../.gitbook/assets/PostfixExpr.png)

#### Object field selector <a href="#objectfieldselector.d12e3356" id="objectfieldselector.d12e3356"></a>

ObjectLookup

![](../.gitbook/assets/ObjectLookup.png)

The simplest way to navigate in an object is similar to JavaScript, using a dot. This will work as soon as you do not push it too much: alphanumerical characters, dashes, underscores - just like unquoted keys in object constructors, any NCName is allowed.

**Object lookup**

```

{ "foo" : "bar" }.foo
      
```

Result (run with Zorba):bar

Since JSONiq expressions are composable, you can also use any expression for the left-hand side. You might need parentheses depending on the precedence.

**Lookup on a single-object collection.**

```

collection("one-object").foo
      
```

Result (run with Zorba):bar

The dot operator does an implicit mapping on the left-hand-side, i.e., it applies the lookup in turn on each item. Lookup on an object returns the value associated with the supplied key, or the empty sequence if there is none. Lookup on any item which is not an object (arrays and atomics) results in the empty sequence.

**Object lookup with an iteration on several objects**

```

({ "foo" : "bar" }, { "foo" : "bar2" }, { "bar" : "foo" }).foo
        
```

Result (run with Zorba):bar bar2

**Object lookup with an iteration on a collection**

```

collection("captains").name
      
```

Result (run with Zorba):James T. Kirk Jean-Luc Picard Benjamin Sisko Kathryn Janeway Jonathan Archer Samantha Carter

**Object lookup on a mixed sequence**

```

({ "foo" : "bar1" }, [ "foo", "bar" ], { "foo" : "bar2" }, "foo").foo
      
```

Result (run with Zorba):bar1 bar2

Of course, unquoted keys will not work for strings that are not NCNames, e.g., if the field contains a dot or begins with a digit. Then you will need quotes.

**Quotes for object lookup**

```

{ "foo bar" : "bar" }."foo bar"
      
```

Result (run with Zorba):bar

If you use an expression on the right side of the dot, it must always have parentheses. The result of the right-hand-side expression is cast to a string. An error is raised if the cast fails.

**Object lookup with a nested expression**

```

{ "foobar" : "bar" }.("foo" || "bar")
      
```

Result (run with Zorba):bar

**Object lookup with a nested expression**

```

{ "foobar" : "bar" }.("foo", "bar")
      
```

Result (run with Zorba):An error was raised: sequence of more than one item can not be treated as type xs:string

**Object lookup with a nested expression**

```

{ "1" : "bar" }.(1)
      
```

Result (run with Zorba):bar

Variables, or a context item reference, do not need parentheses. Variables are introduced later, but here is a sneak peek:

**Object lookup with a variable**

```

let $field := "foo" || "bar"
return { "foobar" : "bar" }.$field
      
```

Result (run with Zorba):bar

#### Array member selector <a href="#arraymemberselector.d12e3489" id="arraymemberselector.d12e3489"></a>

ArrayLookup

![](../.gitbook/assets/ArrayLookup.png)

Array lookup uses double square brackets.

**Array lookup**

```

[ "foo", "bar" ] [[2]]
      
```

Result (run with Zorba):bar

Since JSONiq expressions are composable, you can also use any expression for the left-hand side. You might need parentheses depending on the precedence.

**Array lookup after an object lookup**

```

{ field : [ "one",  { "foo" : "bar" } ] }.field[[2]].foo
      
```

Result (run with Zorba):bar

The array lookup operator does an implicit mapping on the left-hand-side, i.e., it applies the lookup in turn on each item. Lookup on an array returns the item at that position in the array, or the empty sequence if there is none (position larger than size or smaller than 1). Lookup on any item which is not an array (objects and atomics) results in the empty sequence.

**Array lookup with an iteration on several arrays**

```

([ 1, 2, 3 ], [ 4, 5, 6 ])[[2]]
        
```

Result (run with Zorba):2 5

**Array lookup with an iteration on a collection**

```

collection("captains").series[[1]]
      
```

Result (run with Zorba):The original series The next generation The next generation The next generation Entreprise Voyager

**Array lookup on a mixed sequence**

```

([ 1, 2, 3 ], [ 4, 5, 6 ], { "foo" : "bar" }, true)[[3]]
      
```

Result (run with Zorba):3 6

The expression inside the double-square brackets may be any expression. The result of evaluating this expression is cast to an integer. An error is raised if the cast fails.

**Array lookup with a right-hand-side expression**

```

[ "foo", "bar" ] [[ 1 + 1 ]]
      
```

Result (run with Zorba):bar

ArrayUnboxing

![](../.gitbook/assets/ArrayUnboxing.png)

You can also extract all items from an array (i.e., as a sequence) with the \[] syntax. The \[] operator also implicitly iterates on the left-hand-side, returning the empty sequence for non-arrays.

**Extracting all items from an array**

```

[ "foo", "bar" ][]
      
```

Result (run with Zorba):foo bar

**Extracting all items from arrays in a mixed sequence**

```

([ "foo", "bar" ], { "foo" : "bar" }, true, [ 1, 2, 3 ] )[]
      
```

Result (run with Zorba):foo bar 1 2 3

#### Sequence predicates <a href="#sequencepredicates.d12e3612" id="sequencepredicates.d12e3612"></a>

Predicate

![](../.gitbook/assets/Predicate.png)

A predicate allows filtering a sequence, keeping only items that fulfill it.

The predicate is evaluated once for each item in the left-hand-side sequence, with the context item set to that item. The predicate expression can use \$$ to access this context item.

ContextItemExpr

![](../.gitbook/assets/ContextItemExpr.png)

If the predicate evaluates to an integer, it is matched against the item position in the left-hand side sequence automatically

**Predicate expression**

```

(1 to 10)[2]
      
```

Result (run with Zorba):2

Otherwise, the result of the predicate is converted to a boolean.

All items for which the converted predicate result evaluates to true are then output.

**Predicate expression**

```

(1 to 10)[$$ mod 2 eq 0]
      
```

Result (run with Zorba):2 4 6 8 10

### Control flow expressions <a href="#chapter-control-flow" id="chapter-control-flow"></a>

JSONiq supports control flow expressions such as if-then-else, switch and typeswitch following the W3C standard.

#### Conditional expressions <a href="#conditionalexpressions.d12e3681" id="conditionalexpressions.d12e3681"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-conditionals) for conditional expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

IfExpr

![](../.gitbook/assets/IfExpr.png)

A conditional expressions allows you to pick one or another value depending on a boolean value.

**A conditional expression**

```

if (1 + 1 eq 2) then { "foo" : "yes" } else { "foo" : "false" }
      
```

Result (run with Zorba):{ "foo" : "yes" }

The behavior of the expression inside the if is similar to that of logical operations (two-valued logics), meaning that non-boolean values get converted to a boolean.

**A conditional expression**

```

if (null) then { "foo" : "yes" } else { "foo" : "no" }
      
```

Result (run with Zorba):{ "foo" : "no" }

**A conditional expression**

```

if (1) then { "foo" : "yes" } else { "foo" : "no" }
      
```

Result (run with Zorba):{ "foo" : "yes" }

**A conditional expression**

```

if (0) then { "foo" : "yes" } else { "foo" : "no" }
        
```

Result (run with Zorba):{ "foo" : "no" }

**A conditional expression**

```

if ("foo") then { "foo" : "yes" } else { "foo" : "no" }
      
```

Result (run with Zorba):{ "foo" : "yes" }

**A conditional expression**

```

if ("") then { "foo" : "yes" } else { "foo" : "no" }
        
```

Result (run with Zorba):{ "foo" : "no" }

**A conditional expression**

```

if (()) then { "foo" : "yes" } else { "foo" : "no" }
        
```

Result (run with Zorba):{ "foo" : "no" }

**A conditional expression**

```

if (({ "foo" : "bar" }, [ 1, 2, 3, 4])) then { "foo" : "yes" } else { "foo" : "no" }
        
```

Result (run with Zorba):{ "foo" : "yes" }

Note that the else clause is mandatory (but can be the empty sequence)

**A conditional expression**

```

if (1+1 eq 2) then { "foo" : "yes" } else ()
        
```

Result (run with Zorba):{ "foo" : "yes" }

#### Switch expressions <a href="#switchexpressions.d12e3839" id="switchexpressions.d12e3839"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-switch) for switch expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

SwitchExpr

![](../.gitbook/assets/SwitchExpr.png)

SwitchCaseClause

![](../.gitbook/assets/SwitchCaseClause.png)

A switch expression evaluates the expression inside the switch. If it is an atomic, it compares it in turn to the provided atomic values (with the semantics of the eq operator) and returns the value associated with the first matching case clause.

Note that if there is an object or array in the base switch expression or any case expression, a JSONiq-specific type error JNTY0004 will be raised, because objects and arrays cannot be atomized and the W3C standard requires atomization of the base and case expressions.

**A switch expression**

```

switch ("foo")
case "bar" return "foo"
case "foo" return "bar"
default return "none"
        
```

Result (run with Zorba):bar

If it is not an atomic, an error is raised.

**A switch expression**

```

switch ({ "foo" : "bar" })
case "bar" return "foo"
case "foo" return "bar"
default return "none"
        
```

Result (run with Zorba):An error was raised: can not atomize an object item: an object has probably been passed where an atomic value is expected (e.g., as a key, or to a function expecting an atomic item)

If no value matches, the default is used.

**A switch expression**

```

switch ("no-match")
case "bar" return "foo"
case "foo" return "bar"
default return "none"
        
```

Result (run with Zorba):none

The case clauses support composability of expressions as well.

**A switch expression**

```

switch (2)
case 1 + 1 return "foo"
case 2 + 2 return "bar"
default return "none"
        
```

Result (run with Zorba):foo

**A switch expression**

```

switch (true)
case 1 + 1 eq 2 return "1 + 1 is 2"
case 2 + 2 eq 5 return "2 + 2 is 5"
default return "none of the above is true"
        
```

Result (run with Zorba):1 + 1 is 2

#### Try-catch expressions <a href="#trycatchexpressions.d12e3958" id="trycatchexpressions.d12e3958"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-try-catch) for try-catch expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

TryCatchExpr

![](../.gitbook/assets/TryCatchExpr.png)

A try catch expression evaluates the expression inside the try block and returns its resulting value.

However, if an error is raised dynamically, the catch clause is evaluated and its result value returned.

**A try catch expression**

```

try { 1 div 0 } catch * { "division by zero!" } 
      
```

Result (run with Zorba):division by zero!

Only errors raised within the lexical scope of the try block are caught.

**A try catch expression**

```

let $x := 1 div 0
return try { $x }
       catch * { "division by zero!" } 
      
```

Result (run with Zorba):An error was raised: division by zero

Errors that are detected statically within the try block are still reported statically.

**A try catch expression**

```

try { x } catch * { "syntax error" } 
      
```

Result (run with Zorba):syntax error

### FLWOR expressions <a href="#chapter-flwor" id="chapter-flwor"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-flwor-expressions) for FLWOR expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

FLWORExpr

![](../.gitbook/assets/FLWORExpr.png)

FLWOR expressions are probably the most powerful JSONiq construct and correspond to SQL's SELECT-FROM-WHERE statements, but they are more general and more flexible. In particular, clauses can almost appear in any order (apart that it must begin with a for or let clause, and end with a return clause).

Here is a bit of theory on how it works.

A clause binds values to some variables according to its own semantics, possibly several times. Each time, a tuple of variable bindings (mapping variable names to sequences) is passed on to the next clause.

This goes all the way down, until the return clause. The return clause is eventually evaluated for each tuple of variable bindings, resulting in a sequence of items for each tuple.

These sequences of items are concatenated, in the order of the incoming tuples, and the obtained sequence is returned by the FLWOR expression.

We are now giving practical examples with a hint on how it maps to SQL.

#### For clauses <a href="#forclauses.d12e4070" id="forclauses.d12e4070"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-xquery-for-clause) for for clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

ForClause

![](../.gitbook/assets/ForClause.png)

For clauses allow iteration on a sequence.

For each incoming tuple, the expression in the for clause is evaluated to a sequence. Each item in this sequence is in turn bound to the for variable. A tuple is hence produced for each incoming tuple, and for each item in the sequence produced by the for clause for this tuple.

The order in which items are bound by the for clause can be relaxed with unordered expressions, as described later in this section.

The following query, using a for and a return clause, is the counterpart of SQL's "SELECT name FROM captains". $x is bound in turn to each item in the captains collection.

**A for clause.**

```

for $x in collection("captains")
return $x.name
      
```

Result (run with Zorba):James T. Kirk Jean-Luc Picard Benjamin Sisko Kathryn Janeway Jonathan Archer Samantha Carter

For clause expressions are composable, there can be several of them.

**Two for clauses.**

```

for $x in ( 1, 2, 3 )
for $y in ( 1, 2, 3 )
return 10 * $x + $y
      
```

Result (run with Zorba):11 12 13 21 22 23 31 32 33

**A for clause.**

```

for $x in ( 1, 2, 3 ), $y in ( 1, 2, 3 )
return 10 * $x + $y
      
```

Result (run with Zorba):11 12 13 21 22 23 31 32 33

A for variable is visible to subsequence bindings.

**A for clause.**

```

for $x in ( [ 1, 2, 3 ], [ 4, 5, 6 ], [ 7, 8, 9 ] ), $y in $x[]
return $y
      
```

Result (run with Zorba):1 2 3 4 5 6 7 8 9

**A for clause.**

```

for $x in collection("captains"), $y in $x.series[]
return { "captain" : $x.name, "series" : $y }
      
```

Result (run with Zorba):{ "captain" : "James T. Kirk", "series" : "The original series" } { "captain" : "Jean-Luc Picard", "series" : "The next generation" } { "captain" : "Benjamin Sisko", "series" : "The next generation" } { "captain" : "Benjamin Sisko", "series" : "Deep Space 9" } { "captain" : "Kathryn Janeway", "series" : "The next generation" } { "captain" : "Kathryn Janeway", "series" : "Voyager" } { "captain" : "Jonathan Archer", "series" : "Entreprise" } { "captain" : null, "series" : "Voyager" }

It is also possible to bind the position of the current item in the sequence to a variable.

**A for clause.**

```

for $x at $position in collection("captains")
return { "captain" : $x.name, "id" : $position }
        
```

Result (run with Zorba):{ "captain" : "James T. Kirk", "id" : 1 } { "captain" : "Jean-Luc Picard", "id" : 2 } { "captain" : "Benjamin Sisko", "id" : 3 } { "captain" : "Kathryn Janeway", "id" : 4 } { "captain" : "Jonathan Archer", "id" : 5 } { "captain" : null, "id" : 6 } { "captain" : "Samantha Carter", "id" : 7 }

JSONiq supports joins. For example, the counterpart of "SELECT c.name AS captain, m.name AS movie FROM captains c JOIN movies m ON c.name = m.name" is:

**A join**

```

for $captain in collection("captains"), $movie in collection("movies")[ try { $$.captain eq $captain.name } catch * { false } ]
return { "captain" : $captain.name, "movie" : $movie.name }
        
```

Result (run with Zorba):{ "captain" : "James T. Kirk", "movie" : "The Motion Picture" } { "captain" : "James T. Kirk", "movie" : "The Wrath of Kahn" } { "captain" : "James T. Kirk", "movie" : "The Search for Spock" } { "captain" : "James T. Kirk", "movie" : "The Voyage Home" } { "captain" : "James T. Kirk", "movie" : "The Final Frontier" } { "captain" : "James T. Kirk", "movie" : "The Undiscovered Country" } { "captain" : "Jean-Luc Picard", "movie" : "First Contact" } { "captain" : "Jean-Luc Picard", "movie" : "Insurrection" } { "captain" : "Jean-Luc Picard", "movie" : "Nemesis" }

Note how JSONiq handles semi-structured data in a flexible way.

Outer joins are also possible with "allowing empty", i.e., output will also be produced if there is no matching movie for a captain. The following query is the counterpart of "SELECT c.name AS captain, m.name AS movie FROM captains c LEFT JOIN movies m ON c.name = m.captain".

**A join**

```

for $captain in collection("captains"), $movie allowing empty in collection("movies")[ try { $$.captain eq $captain.name } catch * { false } ]
return { "captain" : $captain.name, "movie" : $movie.name }
        
```

Result (run with Zorba):{ "captain" : "James T. Kirk", "movie" : "The Motion Picture" } { "captain" : "James T. Kirk", "movie" : "The Wrath of Kahn" } { "captain" : "James T. Kirk", "movie" : "The Search for Spock" } { "captain" : "James T. Kirk", "movie" : "The Voyage Home" } { "captain" : "James T. Kirk", "movie" : "The Final Frontier" } { "captain" : "James T. Kirk", "movie" : "The Undiscovered Country" } { "captain" : "Jean-Luc Picard", "movie" : "First Contact" } { "captain" : "Jean-Luc Picard", "movie" : "Insurrection" } { "captain" : "Jean-Luc Picard", "movie" : "Nemesis" } { "captain" : "Benjamin Sisko", "movie" : null } { "captain" : "Kathryn Janeway", "movie" : null } { "captain" : "Jonathan Archer", "movie" : null } { "captain" : null, "movie" : null } { "captain" : "Samantha Carter", "movie" : null }

#### Where clauses <a href="#whereclauses.d12e4197" id="whereclauses.d12e4197"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-where) for where clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

WhereClause

![](../.gitbook/assets/WhereClause.png)

Where clauses are used for filtering (selection operator in the relational algebra).

For each incoming tuple, the expression in the where clause is evaluated to a boolean (possibly converting an atomic to a boolean). if this boolean is true, the tuple is forwarded to the next clause, otherwise it is dropped.

The following query corresponds to "SELECT series FROM captains WHERE name = 'Kathryn Janeway'".

**A where clause.**

```

for $x in collection("captains")
where $x.name eq "Kathryn Janeway"
return $x.series
      
```

Result (run with Zorba):\[ "The next generation", "Voyager" ]

#### Order clauses <a href="#orderclauses.d12e4239" id="orderclauses.d12e4239"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-order-by-clause) for order by clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

OrderByClause

![](../.gitbook/assets/OrderByClause.png)

Order clauses are for reordering tuples.

For each incoming tuple, the expression in the where clause is evaluated to an atomic. The tuples are then sorted based on the atomics they are associated with, and then forwarded to the next clause.

Like for ordering comparisons, null values are always considered the smallest.

The following query is the counterpart of SQL's "SELECT \* FROM captains ORDER BY name".

**An order by clause.**

```

for $x in collection("captains")
order by $x.name
return $x
      
```

Result (run with Zorba):{ "name" : "Benjamin Sisko", "series" : \[ "The next generation", "Deep Space 9" ], "century" : 24 } { "name" : "James T. Kirk", "series" : \[ "The original series" ], "century" : 23 } { "name" : "Jean-Luc Picard", "series" : \[ "The next generation" ], "century" : 24 } { "name" : "Jonathan Archer", "series" : \[ "Entreprise" ], "century" : 22 } { "name" : "Kathryn Janeway", "series" : \[ "The next generation", "Voyager" ], "century" : 24 } { "name" : "Samantha Carter", "series" : \[ ], "century" : 21 } { "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : \[ "Voyager" ], "century" : 24 }

Multiple sorting criteria can be given - they are treated like a lexicographic order (most important criterium first).

**An order by clause.**

```

for $x in collection("captains")
order by size($x.series), $x.name
return $x
      
```

Result (run with Zorba):{ "name" : "Samantha Carter", "series" : \[ ], "century" : 21 } { "name" : "James T. Kirk", "series" : \[ "The original series" ], "century" : 23 } { "name" : "Jean-Luc Picard", "series" : \[ "The next generation" ], "century" : 24 } { "name" : "Jonathan Archer", "series" : \[ "Entreprise" ], "century" : 22 } { "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : \[ "Voyager" ], "century" : 24 } { "name" : "Benjamin Sisko", "series" : \[ "The next generation", "Deep Space 9" ], "century" : 24 } { "name" : "Kathryn Janeway", "series" : \[ "The next generation", "Voyager" ], "century" : 24 }

It can be specified whether the order is ascending or descending. Empty sequences are allowed and it can be chosen whether to put them first or last.

**An order by clause.**

```

for $x in collection("captains")
order by $x.name descending empty greatest
return $x
      
```

Result (run with Zorba):{ "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : \[ "Voyager" ], "century" : 24 } { "name" : "Samantha Carter", "series" : \[ ], "century" : 21 } { "name" : "Kathryn Janeway", "series" : \[ "The next generation", "Voyager" ], "century" : 24 } { "name" : "Jonathan Archer", "series" : \[ "Entreprise" ], "century" : 22 } { "name" : "Jean-Luc Picard", "series" : \[ "The next generation" ], "century" : 24 } { "name" : "James T. Kirk", "series" : \[ "The original series" ], "century" : 23 } { "name" : "Benjamin Sisko", "series" : \[ "The next generation", "Deep Space 9" ], "century" : 24 }

An error is raised if the expression does not evaluate to an atomic or the empty sequence.

**An order by clause.**

```

for $x in collection("captains")
order by $x
return $x.name
      
```

Result (run with Zorba):An error was raised: can not atomize an object item: an object has probably been passed where an atomic value is expected (e.g., as a key, or to a function expecting an atomic item)

Collations can be used to give a specific way of how strings are to be ordered. A collation is identified by a URI.

**Use of a collation in an order by clause.**

```

for $x in collection("captains")
order by $x.name collation "http://www.w3.org/2005/xpath-functions/collation/codepoint"
return $x.name
      
```

Result (run with Zorba):Benjamin Sisko James T. Kirk Jean-Luc Picard Jonathan Archer Kathryn Janeway Samantha Carter

#### Group clauses <a href="#groupclauses.d12e4331" id="groupclauses.d12e4331"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-group-by) for group by clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

GroupByClause

![](../.gitbook/assets/GroupByClause.png)

Grouping is also supported, like in SQL.

For each incoming tuple, the expression in the group clause is evaluated to an atomic (a grouping key). The incoming tuples are then grouped according to the key they are associated with.

For each group, a tuple is output, with a binding from the grouping variable to the key of the group.

**An order by clause.**

```

for $x in collection("captains")
group by $century := $x.century
return { "century" : $century  }
      
```

Result (run with Zorba):{ "century" : 21 } { "century" : 22 } { "century" : 23 } { "century" : 24 }

As for the other (non-grouping) variables, their values within one group are all concatenated, keeping the same name. Aggregations can be done on these variables.

The following query is equivalent to "SELECT century, COUNT(\*) FROM captains GROUP BY century".

**An order by clause.**

```

for $x in collection("captains")
group by $century := $x.century
return { "century" : $century, "count" : count($x) }
      
```

Result (run with Zorba):{ "century" : 21, "count" : 1 } { "century" : 22, "count" : 1 } { "century" : 23, "count" : 1 } { "century" : 24, "count" : 4 }

JSONiq's group by is more flexible than SQL and is fully composable.

**An order by clause.**

```

for $x in collection("captains")
group by $century := $x.century
return { "century" : $century, "captains" : [ $x.name ] }
      
```

Result (run with Zorba):{ "century" : 21, "captains" : \[ "Samantha Carter" ] } { "century" : 22, "captains" : \[ "Jonathan Archer" ] } { "century" : 23, "captains" : \[ "James T. Kirk" ] } { "century" : 24, "captains" : \[ "Jean-Luc Picard", "Benjamin Sisko", "Kathryn Janeway" ] }

Unlike SQL, JSONiq does not need a having clause, because a where clause works perfectly after grouping as well.

The following query is the counterpart of "SELECT century, COUNT(\*) FROM captains GROUP BY century HAVING COUNT(\*) > 1"

**An order by clause.**

```

for $x in collection("captains")
group by $century := $x.century
where count($x) gt 1
return { "century" : $century, "count" : count($x) }
      
```

Result (run with Zorba):{ "century" : 24, "count" : 4 }

#### Let clauses <a href="#letclauses.d12e4413" id="letclauses.d12e4413"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-xquery-let-clause) for let clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

LetClause

![](../.gitbook/assets/LetClause.png)

Let bindings can be used to define aliases for any sequence, for convenience.

For each incoming tuple, the expression in the let clause is evaluated to a sequence. A binding is added from this sequence to the let variable in each tuple. A tuple is hence produced for each incoming tuple.

**An order by clause.**

```

for $x in collection("captains")
let $century := $x.century
group by $century
let $number := count($x)
where $number gt 1
return { "century" : $century, "count" : $number }
      
```

Result (run with Zorba):{ "century" : 24, "count" : 4 }

Note that it is perfectly fine to reuse a variable name and hide a variable binding.

**An order by clause.**

```

for $x in collection("captains")
let $century := $x.century
group by $century
let $number := count($x)
let $number := count(distinct-values(for $series in $x.series
                                     return typeswitch($series)
                                            case array return $series()
                                            default return $series ))
where $number gt 1
return { "century" : $century, "number of series" : $number }
      
```

Result (run with Zorba):{ "century" : 24, "number of series" : 3 }

#### Count clauses <a href="#countclauses.d12e4464" id="countclauses.d12e4464"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-count) for count clauses. The following explanations, provided as an informal summary for convenience, are non-normative.

CountClause

![](../.gitbook/assets/CountClause.png)

For each incoming tuple, a binding from the position of this tuple in the tuple stream to the count variable is added. The new tuple is then forwarded to the next clause.

**An order by clause.**

```

for $x in collection("captains")
order by $x.name
count $c
return { "id" : $c, "captain" : $x }
      
```

Result (run with Zorba):{ "id" : 1, "captain" : { "name" : "Benjamin Sisko", "series" : \[ "The next generation", "Deep Space 9" ], "century" : 24 } } { "id" : 2, "captain" : { "name" : "James T. Kirk", "series" : \[ "The original series" ], "century" : 23 } } { "id" : 3, "captain" : { "name" : "Jean-Luc Picard", "series" : \[ "The next generation" ], "century" : 24 } } { "id" : 4, "captain" : { "name" : "Jonathan Archer", "series" : \[ "Entreprise" ], "century" : 22 } } { "id" : 5, "captain" : { "name" : "Kathryn Janeway", "series" : \[ "The next generation", "Voyager" ], "century" : 24 } } { "id" : 6, "captain" : { "name" : "Samantha Carter", "series" : \[ ], "century" : 21 } } { "id" : 7, "captain" : { "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : \[ "Voyager" ], "century" : 24 } }

#### Map operator <a href="#mapoperator.d12e4500" id="mapoperator.d12e4500"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-map-operator) for the map operator, except that it changes the syntax for the context item to _\$$_ instead of the _._ syntax.

The following explanations, provided as an informal summary for convenience, are non-normative.

SimpleMapExpr

![](../.gitbook/assets/SimpleMapExpr.png)

ContextItemExpr

![](../.gitbook/assets/ContextItemExpr.png)

JSONiq provides a shortcut for a for-return construct, automatically binding each item in the left-hand-side sequence to the context item.

**A simple map**

```

(1 to 10) ! ($$ * 2)
      
```

Result (run with Zorba):2 4 6 8 10 12 14 16 18 20

**An equivalent query**

```

for $i in 1 to 10
return $i * 2
      
```

Result (run with Zorba):2 4 6 8 10 12 14 16 18 20

#### Variable references <a href="#variablereferences.d12e4566" id="variablereferences.d12e4566"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-variables) for variable references, except that it disallows the character _._ in variable names, which is instead used for object lookup.

#### Composing FLWOR expressions <a href="#composingflworexpressions.d12e4580" id="composingflworexpressions.d12e4580"></a>

Like all other expressions, FLWOR expressions can be composed. In the following examples, a FLWOR is nested in a function call, nested in a FLWOR, nested in an array constructor:

**Nested FLWORs**

```

        [
          for $c in collection("captains")
          where exists(for $m in collection("movies")
                       where some $moviecaptain in let $captain := $m.captain
                                                   return typeswitch ($captain)
                                                          case array return $captain()
                                                          default return $captain
                             satisfies
                             $moviecaptain eq $c.name
                       return $m)
          return $c.name
        ]
      
```

Result (run with Zorba):\[ "James T. Kirk", "Jean-Luc Picard" ]

#### Ordered and Unordered expressions <a href="#orderedandunorderedexpressions.d12e4597" id="orderedandunorderedexpressions.d12e4597"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-unordered-expressions) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

OrderedExpr

![](../.gitbook/assets/OrderedExpr.png)

UnorderedExpr

![](../.gitbook/assets/UnorderedExpr.png)

By default, the order in which a for clause binds its items is important.

This behaviour can be relaxed in order give the optimizer more leeway. An unordered expression relaxes ordering by for clauses within its operand scope:

**An unordered expression.**

```

unordered {
  for $captain in collection("captains")
  where $captain.century eq 24
  return $captain
}
      
```

Result (run with Zorba):{ "name" : "Jean-Luc Picard", "series" : \[ "The next generation" ], "century" : 24 } { "name" : "Benjamin Sisko", "series" : \[ "The next generation", "Deep Space 9" ], "century" : 24 } { "name" : "Kathryn Janeway", "series" : \[ "The next generation", "Voyager" ], "century" : 24 } { "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : \[ "Voyager" ], "century" : 24 }

An ordered expression can be used to reactivate ordering behaviour in a subscope.

**An ordered expression.**

```

unordered {
  for $captain in collection("captains")
  where ordered { exists(for $movie at $i in collection("movies")
                         where $i eq 5
                         where $movie.captain eq $captain.name
                         return $movie) }
  return $captain
}
      
```

Result (run with Zorba):{ "name" : "James T. Kirk", "series" : \[ "The original series" ], "century" : 23 }

### Expressions dealing with types <a href="#section-type-expressions" id="section-type-expressions"></a>

This section describes JSONiq types as well as the sequence type syntax.

#### Instance-of expressions <a href="#instanceofexpressions.d12e4668" id="instanceofexpressions.d12e4668"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-instance-of) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

InstanceofExpr

![](../.gitbook/assets/InstanceofExpr.png)

An instance expression can be used to tell whether a JSONiq value matches a given sequence type.

**Instance of expression**

```

1 instance of integer
      
```

Result (run with Zorba):true

**Instance of expression**

```

1 instance of string
      
```

Result (run with Zorba):false

**Instance of expression**

```

"foo" instance of string
      
```

Result (run with Zorba):true

**Instance of expression**

```

{ "foo" : "bar" } instance of object
      
```

Result (run with Zorba):true

**Instance of expression**

```

({ "foo" : "bar" }, { "bar" : "foo" }) instance of json-item+
      
```

Result (run with Zorba):true

**Instance of expression**

```

[ 1, 2, 3 ] instance of array?
      
```

Result (run with Zorba):true

**Instance of expression**

```

() instance of ()
      
```

Result (run with Zorba):true

#### Treat expressions <a href="#treatexpressions.d12e4765" id="treatexpressions.d12e4765"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-treat) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

TreatExpr

![](../.gitbook/assets/TreatExpr.png)

A treat expression checks that a JSONiq value matches a given sequence type. If it is not the case, an error is raised.

**Treat as expression**

```

1 treat as integer
      
```

Result (run with Zorba):1

**Treat as expression**

```

1 treat as string
      
```

Result (run with Zorba):An error was raised: "xs:integer" cannot be treated as type xs:string

**Treat as expression**

```

"foo" treat as string
      
```

Result (run with Zorba):foo

**Treat as expression**

```

{ "foo" : "bar" } treat as object
      
```

Result (run with Zorba):{ "foo" : "bar" }

**Treat as expression**

```

({ "foo" : "bar" }, { "bar" : "foo" }) treat as json-item+
      
```

Result (run with Zorba):{ "foo" : "bar" } { "bar" : "foo" }

**Treat as expression**

```

[ 1, 2, 3 ] treat as array?
      
```

Result (run with Zorba):\[ 1, 2, 3 ]

**Treat as expression**

```

() treat as ()
      
```

Result (run with Zorba):

#### Castable expressions <a href="#castableexpressions.d12e4862" id="castableexpressions.d12e4862"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-castable) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

CastableExpr

![](../.gitbook/assets/CastableExpr.png)

A castable expression checks whether a JSONiq value can be cast to a given atomic type and returns true or false accordingly. It can be used before actually casting to that type.

**Castable as expression**

```

"1" castable as integer
      
```

Result (run with Zorba):true

**Castable as expression**

```

"foo" castable as integer
      
```

Result (run with Zorba):false

**Castable as expression**

```

"2013-04-02" castable as date
      
```

Result (run with Zorba):true

**Castable as expression**

```

() castable as date
      
```

Result (run with Zorba):false

**Castable as expression**

```

("2013-04-02", "2013-04-03") castable as date
      
```

Result (run with Zorba):false

The question mark allows for an empty sequence.

**Castable as expression**

```

() castable as date?
      
```

Result (run with Zorba):true

#### Cast expressions <a href="#castexpressions.d12e4952" id="castexpressions.d12e4952"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-cast) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

CastExpr

![](../.gitbook/assets/CastExpr.png)

A cast expression casts a JSONiq value to a given atomic type. The resulting value is annotated with this type.

**Cast as expression**

```

"1" cast as integer
      
```

Result (run with Zorba):1

**Cast as expression**

```

"foo" cast as integer
      
```

Result (run with Zorba):An error was raised: "foo": value of type xs:string is not castable to type xs:integer

**Cast as expression**

```

"2013-04-02" cast as date
      
```

Result (run with Zorba):2013-04-02

**Cast as expression**

```

() cast as date
      
```

Result (run with Zorba):An error was raised: empty sequence can not be cast to type with quantifier '1'

**Cast as expression**

```

("2013-04-02", "2013-04-03") cast as date
      
```

Result (run with Zorba):An error was raised: sequence of more than one item can not be cast to type with quantifier '1' or '?'

The question mark allows for an empty sequence.

**Cast as expression**

```

() cast as date?
      
```

Result (run with Zorba):

**Cast as expression**

```

"2013-04-02" cast as date?
      
```

Result (run with Zorba):2013-04-02

#### Typeswitch expressions <a href="#typeswitchexpressions.d12e5052" id="typeswitchexpressions.d12e5052"></a>

JSONiq follows the [W3C standard](https://www.w3.org/TR/xquery-30/#id-typeswitch) for ordered and unordered expressions. The following explanations, provided as an informal summary for convenience, are non-normative.

TypeswitchExpr

![](../.gitbook/assets/TypeswitchExpr.png)

CaseClause

![](../.gitbook/assets/CaseClause.png)

A typeswitch expressions tests if the value resulting from the first operand matches a given list of types. The expression corresponding to the first matching case is finally evaluated. If there is no match, the expression in the default clause is evaluated.

**Typeswitch expression**

```

typeswitch("foo")
case integer return "integer"
case string return "string"
case object return "object"
default return "other"
      
```

Result (run with Zorba):string

In each clause, it is possible to bind the value of the first operand to a variable.

**Typeswitch expression**

```

typeswitch("foo")
case $i as integer return $i + 1
case $s as string return $s || "foo"
case $o as object return [ $o ]
default $d return $d
      
```

Result (run with Zorba):foofoo

The vertical bar can be used to allow several types in the same case clause.

**Typeswitch expression**

```

typeswitch("foo")
case $a as integer | string return { "integer or string" : $a }
case $o as object return [ $o ]
default $d return $d
      
```

Result (run with Zorba):{ "integer or string" : "foo" }

## Prologs <a href="#chapter-prolog" id="chapter-prolog"></a>

This section introduces prologs, which allows declaring functions and global variables that can then be used in the main query. A prolog also allows setting some default behaviour.

MainModule

![](../.gitbook/assets/MainModule.png)

Prolog

![](../.gitbook/assets/Prolog.png)

The prolog appears before the main query and is optional. It can contain setters and module imports, followed by function and variable declarations.

Module imports are explained in the next chapter.

### Setters. <a href="#setters.d12e5165" id="setters.d12e5165"></a>

Setters allow to specify a default behaviour for various aspects of the language.

#### Default collation <a href="#defaultcollation.d12e5173" id="defaultcollation.d12e5173"></a>

DefaultCollationDecl

![](../.gitbook/assets/DefaultCollationDecl.png)

This specifies the default collation used for grouping and ordering clauses in FLWOR expressions. It can be overriden with a collation directive in these clauses.

#### Default ordering mode <a href="#defaultorderingmode.d12e5193" id="defaultorderingmode.d12e5193"></a>

OrderingModeDecl

![](../.gitbook/assets/OrderingModeDecl.png)

This specifies the default behaviour of from clauses, i.e., if they bind tuples in the order in which items occur in the binding sequence. It can be overriden with ordered and unordered expressions.

#### Default ordering behaviour for empty sequences <a href="#defaultorderingbehaviourforemptysequences.d12e5214" id="defaultorderingbehaviourforemptysequences.d12e5214"></a>

EmptyOrderDecl

![](../.gitbook/assets/EmptyOrderDecl.png)

This specifies whether empty sequences come first or last in an ordering clause. It can be overriden by the corresponding directives in such clauses.

#### Default decimal format <a href="#defaultdecimalformat.d12e5234" id="defaultdecimalformat.d12e5234"></a>

DecimalFormatDecl

![](../.gitbook/assets/DecimalFormatDecl.png)

DFPropertyName

![](../.gitbook/assets/DFPropertyName.png)

This specifies a default decimal format for the builtin function format-number().

### Global variables <a href="#globalvariables.d12e5268" id="globalvariables.d12e5268"></a>

VarDecl

![](../.gitbook/assets/VarDecl.png)

Variables can be declared global. Global variables are declared in the prolog.

#### Global variable

```

  declare variable $obj := { "foo" : "bar" };
  $obj
      
```

Result (run with Zorba):{ "foo" : "bar" }

#### Global variable

```

  declare variable $numbers := (1, 2, 3, 4, 5);
  [ $numbers ]
      
```

Result (run with Zorba):\[ 1, 2, 3, 4, 5 ]

You can specify a type for a variable. If the type does not match, an error is raised. Types will be explained later. In general, you do not need to worry too much about variable types except if you want to make sure that what you bind to a variable is really what you want. In most cases, the engine will take care of types for you.

#### Global variable with a type

```

  declare variable $obj as object := { "foo" : "bar" };
  $obj
      
```

Result (run with Zorba):{ "foo" : "bar" }

An external variable allows you to pass a value from the outside environment, which can be very useful. Each implementation can choose their own way of passing a value to an external variable. A default value for an external variable can also be supplied in case none is provided outside.

#### An external global variable

```

  declare variable $obj external;
  $obj
      
```

Result (run with Zorba):An error was raised: "obj": variable has no value

#### An external global variable with a default value

```

  declare variable $obj external := { "foo" : "bar" };
  $obj
      
```

Result (run with Zorba):{ "foo" : "bar" }

### Functions <a href="#functions.d12e5343" id="functions.d12e5343"></a>

VarDecl

![](../.gitbook/assets/FunctionDecl.png)

You can define your own functions in the prolog. These user-defined functions must be prefixed with _local:_, both in the declaration and when called.

Remember than types are optional, and if you do not specify any, item\* is assumed, both for parameters and for the return type.

#### An external global variable with a default value

```

declare function local:say-hello($x) { "Hello, " || $x || "!" };
local:say-hello("Mister Spock")
      
```

Result (run with Zorba):Hello, Mister Spock!

#### An external global variable with a default value

```

declare function local:say-hello($x as string) { "Hello, " || $x || "!" };
local:say-hello("Mister Spock")
      
```

Result (run with Zorba):Hello, Mister Spock!

#### An external global variable with a default value

```

declare function local:say-hello($x as string) as string { "Hello, " || $x || "!" };
local:say-hello("Mister Spock")
      
```

Result (run with Zorba):Hello, Mister Spock!

If you do specify types, an error is raised in case of a mismatch

#### An external global variable with a default value

```

declare function local:say-hello($x) { "Hello, " || $x || "!" }; 
local:say-hello(1)
      
```

Result (run with Zorba):Hello, 1!

## Modules <a href="#chapter-modules" id="chapter-modules"></a>

Module

![](../.gitbook/assets/Module.png)

You can group functions and variables in separate library modules.

MainModule

![](../.gitbook/assets/MainModule.png)

Up to now, everything we encountered were main modules, i.e., a prolog followed by a main query.

LibraryModule

![](../.gitbook/assets/LibraryModule.png)

A library module does not contain any query - just functions and variables that can be imported by other modules.

A library module must be assigned to a namespace. For convenience, this namespace is bound to an alias in the module declaration. All variables and functions in a library module must be prefixed with this alias.

### A library module

```

module namespace my = "http://www.example.com/my-module";
declare variable $my:variable := { "foo" : "bar" };
declare variable $my:n := 42;
declare function my:function($i as integer) { $i * $i };
    
```

ModuleImport

![](../.gitbook/assets/ModuleImport.png)

Here is a main module which imports the former library module. An alias is given to the module namespace (my). Variables and functions from that module can be accessed by prefixing their names with this alias. The alias may be different than the internal alias defined in the imported module.

### An importing main module

```

import module namespace other= "http://www.example.com/my-module";
other:function($other:n)
    
```

Result (run with Zorba):1764

## Function Library <a href="#chapter-functions" id="chapter-functions"></a>

JSONiq provides a rich set of functions.

### JSON specific functions. <a href="#jsonspecificfunctions.d12e5506" id="jsonspecificfunctions.d12e5506"></a>

Some functions are specific to JSON.

#### keys <a href="#keys.d12e5512" id="keys.d12e5512"></a>

This function returns the distinct keys of all objects in the supplied sequence, in an implementation-dependent order.

`keys($o as item*) as string*`

**Getting all distinct key names in the supplied objects, ignoring non-objects.**

```

let $o := ("foo", [ 1, 2, 3 ], { "a" : 1, "b" : 2 }, { "a" : 3, "c" : 4 })
return keys($o)
        
```

Result (run with Zorba):a b c

**Retrieving all Pairs from an Object:**

```

let $map := { "eyes" : "blue", "hair" : "fuchsia" }
for $key in keys($map)
return { $key : $map.$key }
        
```

Result (run with Zorba):{ "eyes" : "blue" } { "hair" : "fuchsia" }

#### members <a href="#members.d12e5544" id="members.d12e5544"></a>

This functions returns all members of all arrays of the supplied sequence.

`members($a as item*) as item*`

**Retrieving the members of all supplied arrays, ignoring non-arrays.**

```

let $planets :=  ( "foo", { "foo" : "bar "}, [ "mercury", "venus", "earth", "mars" ], [ 1, 2, 3 ])
return members($planets)
        
```

Result (run with Zorba):mercury venus earth mars 1 2 3

#### null <a href="#null.d12e5566" id="null.d12e5566"></a>

This function returns the JSON null.

`null() as null`

#### parse-json <a href="#parsejson.d12e5579" id="parsejson.d12e5579"></a>

This function parses its first parameter (a string) as JSON, and returns the resulting sequence of objects and arrays.

`parse-json($arg as string?) as json-item*`

`parse-json($arg as string?, $options as object) as json-item*`

The object optionally supplied as the second parameter may contain additional options:

* `jsoniq-multiple-top-level-items` (boolean): indicates whether parsing to zero, or several objects is allowed. An error is raised if this value is false and there is not exactly one object that was parsed.

If parsing is not successful, an error is raised. Parsing is considered in particular to be non-successful if the boolean associated with "jsoniq-multiple-top-level-items" in the additional parameters is false and there is extra content after parsing a single abject or array.

**Parsing a JSON document**

```

parse-json("{ \"foo\" : \"bar\" }", { "jsoniq-multiple-top-level-items" : false })
        
```

Result (run with Zorba):{ "foo" : "bar" }

**Parsing multiple, whitespace-separated JSON documents**

```

parse-json("{ \"foo\" : \"bar\" } { \"bar\" : \"foo\" }")
        
```

Result (run with Zorba):{ "foo" : "bar" } { "bar" : "foo" }

#### size <a href="#size.d12e5631" id="size.d12e5631"></a>

This function returns the size of the supplied array, or the empty sequence if the empty sequence is provided.

`size($a as array?) as integer?`

**Retrieving the size of an array**

```

let $a := [1 to 10]
return size($a)
        
```

Result (run with Zorba):10

#### accumulate <a href="#accumulate.d12e5653" id="accumulate.d12e5653"></a>

This function dynamically builds an object, like the {| |} syntax, except that it does not throw an error upon pair collision. Instead, it accumulates them, wrapping into an array if necessary. Non-objects are ignored.

```

declare function accumulate($seq as item*) as object
{
  {|
    keys($seq) ! { $$ : $seq.$$ }
  |}
};
      
```

#### descendant-arrays <a href="#descendantarrays.d12e5662" id="descendantarrays.d12e5662"></a>

This function returns all arrays contained within the supplied items, regardless of depth.

```

declare function descendant-arrays($seq as item*) as array*
{
  for $i in $seq
  return typeswitch ($i)
  case array return ($i, descendant-arrays($i[])
  case object return descendant-arrays(values($i))
  default return ()
};
      
```

#### descendant-objects <a href="#descendantobjects.d12e5671" id="descendantobjects.d12e5671"></a>

This function returns all objects contained within the supplied items, regardless of depth.

```

declare function descendant-objects($seq as item*) as object*
{
  for $i in $seq
  return typeswitch ($i)
  case object return ($i, descendant-objects(values($i)))
  case array return descendant-objects($i[])
  default return ()
};
      
```

#### descendant-pairs <a href="#descendantpairs.d12e5680" id="descendantpairs.d12e5680"></a>

This function returns all descendant pairs within the supplied items.

```

declare function descendant-pairs($seq as item*)
{
  for $i in $seq
  return typeswitch ($i)
  case object return
    for $k in keys($o)
    let $v := $o.$k
    return ({ $k : $v }, descendant-pairs($v))
  case array return descendant-pairs($i[])
  default return ()
};
      
```

**Accessing all descendant pairs**

```

let $o := 
{
  "first" : 1,
  "second" : { 
    "first" : "a", 
    "second" : "b" 
  }
}
return descendant-pairs($o)
        
```

Result (run with Zorba):An error was raised: "descendant-pairs": function with arity 1 not declared

#### flatten <a href="#flatten.d12e5700" id="flatten.d12e5700"></a>

This function recursively flattens arrays in the input sequence, leaving non-arrays intact.

```

declare function flatten($seq as item*) as item*
{
  for $value in $seq
  return typeswitch ($value)
         case array return flatten($value[])
         default return $value
};
	  
```

#### intersect <a href="#intersect.d12e5709" id="intersect.d12e5709"></a>

This function returns the intersection of the supplied objects, and aggregates values corresponding to the same name into an array. Non-objects are ignored.

```

declare function intersect($seq as item*)
{
  {|
    let $objects := $seq[. instance of object()]
    for $key in keys(head($objects))
    where every $object in tail($objects)
          satisfies exists(index-of(keys($object), $key))
    return { $key : $objects.$key }
  |}
};
      
```

#### project <a href="#project.d12e5718" id="project.d12e5718"></a>

This function iterates on the input sequence. It projects objects by filtering their pairs and leaves non-objects intact.

```

declare function project($seq as item*, $keys as string*) as item*
{
  for $item in $seq
  return typeswitch ($item)
         case $object as object return
         {|
           for $key in keys($object)
           where some $to-project in $keys satisfies $to-project eq $key
           let $value := $object.$key
           return { $key : $value }
         |}
         default return $item
};
      
```

**Projecting an object 1**

```

let $o := {
  "Captain" : "Kirk",
  "First Officer" : "Spock",
  "Engineer" : "Scott"
  }
return project($o, ("Captain", "First Officer"))
        
```

Result (run with Zorba):{ "Captain" : "Kirk", "First Officer" : "Spock" }

**Projecting an object 2**

```

let $o := {
  "Captain" : "Kirk",
  "First Officer" : "Spock",
  "Engineer" : "Scott"
  }
return project($o, "XQuery Evangelist")
        
```

Result (run with Zorba):{ }

#### remove-keys <a href="#removekeys.d12e5747" id="removekeys.d12e5747"></a>

This function iterates on the input sequence. It removes the pairs with the given keys from all objects and leaves non-objects intact.

```

declare function remove-keys($seq as item*, $keys as string*) as item*
{
  for $item in $seq
  return typeswitch ($item)
         case $object as object return
         {|
           for $key in keys($object)
           where every $to-remove in $keys satisfies $to-remove ne $key
           let $value := $object.$key
           return { $key : $value }
         |}
         default return $item
};
      
```

**Removing keys from an object (not implemented yet)**

```

let $o := {
  "Captain" : "Kirk",
  "First Officer" : "Spock",
  "Engineer" : "Scott"
  }
return remove-keys($o, ("Captain", "First Officer"))
        
```

Result (run with Zorba):An error was raised: "remove-keys": function with arity 2 not declared

#### values <a href="#values.d12e5766" id="values.d12e5766"></a>

This function returns all values in the supplied objects. Non-objects are ignored.

```

declare function values($seq as item*) as item* {
  for $i in $seq
  for $k in jn:keys($i)
  return $i($k)
};
      
```

#### encode-for-roundtrip <a href="#encodeforroundtrip.d12e5775" id="encodeforroundtrip.d12e5775"></a>

This function encodes any sequence of items, even containing non-JSON types, to a sequence of JSON items that can be serialized as pure JSON, in a way that it can be parsed and decoded back using decode-from-roundtrip. JSON features are left intact, while atomic items annotated with a non-JSON type are converted to objects embedding all necessary information.

`encode-for-roundtrip($items as item*) as json-item*`

#### decode-from-roundtrip <a href="#decodefromroundtrip.d12e5788" id="decodefromroundtrip.d12e5788"></a>

This function decodes a sequence previously encoded with encode-for-roundtrip.

`decode-from-roundtrip($items as json-item*) as item*`

### Functions taken from XQuery <a href="#functionstakenfromxquery.d12e5801" id="functionstakenfromxquery.d12e5801"></a>

* Access to the external environment: [collection#1](https://www.w3.org/TR/xpath-functions-30/#func-collection)
* Function to turn atomics into booleans for use in two-valued logics: [boolean#1](https://www.w3.org/TR/xpath-functions-30/#func-boolean)
* Raising errors: [error#0](https://www.w3.org/TR/xpath-functions-30/#func-error), [error#1](https://www.w3.org/TR/xpath-functions-30/#func-error), [error#2](https://www.w3.org/TR/xpath-functions-30/#func-error), [error#3](https://www.w3.org/TR/xpath-functions-30/#func-error).
* Functions on numeric values: [abs#1](https://www.w3.org/TR/xpath-functions-30/#func-abs), [ceilingabs#1](https://www.w3.org/TR/xpath-functions-30/#func-ceilingabs), [floorabs#1](https://www.w3.org/TR/xpath-functions-30/#func-floorabs), [roundabs#1](https://www.w3.org/TR/xpath-functions-30/#func-roundabs), [round-half-to-even#1](https://www.w3.org/TR/xpath-functions-30/#func-round-half-to-even)
* Parsing numbers: [number#0](https://www.w3.org/TR/xpath-functions-30/#func-number), [number#1](https://www.w3.org/TR/xpath-functions-30/#func-number)
* Formatting integers: [format-integer#2](https://www.w3.org/TR/xpath-functions-30/#func-format-integer), [format-integer#3](https://www.w3.org/TR/xpath-functions-30/#func-format-integer)
* Formatting numbers: [format-numberreturn r#2](https://www.w3.org/TR/xpath-functions-30/#func-format-number), [format-number#3](https://www.w3.org/TR/xpath-functions-30/#func-format-number)
* Trigonometric and exponential functions: [pi#0](https://www.w3.org/TR/xpath-functions-30/#func-pi), [exp#1](https://www.w3.org/TR/xpath-functions-30/#func-exp), [exp10#1](https://www.w3.org/TR/xpath-functions-30/#func-exp10), [log#1](https://www.w3.org/TR/xpath-functions-30/#func-log), [log10#1](https://www.w3.org/TR/xpath-functions-30/#func-log10), [pow#2](https://www.w3.org/TR/xpath-functions-30/#func-pow), [sqrt#1](https://www.w3.org/TR/xpath-functions-30/#func-sqrt), [sin#1](https://www.w3.org/TR/xpath-functions-30/#func-sin), [cos#1](https://www.w3.org/TR/xpath-functions-30/#func-cos), [tan#1](https://www.w3.org/TR/xpath-functions-30/#func-tan), [asin#1](https://www.w3.org/TR/xpath-functions-30/#func-asin), [acos#1](https://www.w3.org/TR/xpath-functions-30/#func-acos), [atan#1](https://www.w3.org/TR/xpath-functions-30/#func-atan), [atan2#1](https://www.w3.org/TR/xpath-functions-30/#func-atan2)
* Functions to assemble and disassemble strings: [codepoints-to-string#1](https://www.w3.org/TR/xpath-functions-30/#func-codepoints-to-string), [string-to-codepoints#1](https://www.w3.org/TR/xpath-functions-30/#func-string-to-codepoints)
* Comparison of strings: [compare#2](https://www.w3.org/TR/xpath-functions-30/#func-compare), [compare#3](https://www.w3.org/TR/xpath-functions-30/#func-compare), [codepoint-equal#2](https://www.w3.org/TR/xpath-functions-30/#func-codepoint-equal)
* Functions on string values: [concat#2](https://www.w3.org/TR/xpath-functions-30/#func-concat), [string-join#1](https://www.w3.org/TR/xpath-functions-30/#func-string-join), [string-join#2](https://www.w3.org/TR/xpath-functions-30/#func-string-join), [substring#2](https://www.w3.org/TR/xpath-functions-30/#func-substring), [substring#3](https://www.w3.org/TR/xpath-functions-30/#func-substring), [string-length#0](https://www.w3.org/TR/xpath-functions-30/#func-string-length), [string-length#1](https://www.w3.org/TR/xpath-functions-30/#func-string-length), [normalize-space#0](https://www.w3.org/TR/xpath-functions-30/#func-normalize-space), [normalize-space#1](https://www.w3.org/TR/xpath-functions-30/#func-normalize-space), [normalize-unicode#1](https://www.w3.org/TR/xpath-functions-30/#func-normalize-unicode), [normalize-unicode#2](https://www.w3.org/TR/xpath-functions-30/#func-normalize-unicode), [upper-case#1](https://www.w3.org/TR/xpath-functions-30/#func-upper-case), [lower-case#1](https://www.w3.org/TR/xpath-functions-30/#func-lower-case), [translate#3](https://www.w3.org/TR/xpath-functions-30/#func-translate)
* Functions based on substring matching: [contains#2](https://www.w3.org/TR/xpath-functions-30/#func-contains), [contains#3](https://www.w3.org/TR/xpath-functions-30/#func-contains), [starts-with#2](https://www.w3.org/TR/xpath-functions-30/#func-starts-with), [starts-with#3](https://www.w3.org/TR/xpath-functions-30/#func-starts-with), [ends-with#2](https://www.w3.org/TR/xpath-functions-30/#func-ends-with), [ends-with#3](https://www.w3.org/TR/xpath-functions-30/#func-ends-with), [substring-before#2](https://www.w3.org/TR/xpath-functions-30/#func-substring-before), [substring-before#3](https://www.w3.org/TR/xpath-functions-30/#func-substring-before), [substring-after#2](https://www.w3.org/TR/xpath-functions-30/#func-substring-after), [substring-after#3](https://www.w3.org/TR/xpath-functions-30/#func-substring-after)
* String functions that use regular expressions: [matches#2](https://www.w3.org/TR/xpath-functions-30/#func-matches), [matches#3](https://www.w3.org/TR/xpath-functions-30/#func-matches), [replace#3](https://www.w3.org/TR/xpath-functions-30/#func-replace), [replace#4](https://www.w3.org/TR/xpath-functions-30/#func-replace), [tokenize#2](https://www.w3.org/TR/xpath-functions-30/#func-tokenize), [tokenize#3](https://www.w3.org/TR/xpath-functions-30/#func-tokenize)
* Functions that manipulate URIs: [resolve-uri#1](https://www.w3.org/TR/xpath-functions-30/#func-resolve-uri), [resolve-uri#2](https://www.w3.org/TR/xpath-functions-30/#func-resolve-uri), [encode-for-uri#1](https://www.w3.org/TR/xpath-functions-30/#func-encode-for-uri), [iri-to-uri#1](https://www.w3.org/TR/xpath-functions-30/#func-iri-to-uri), [escape-html-uri#1](https://www.w3.org/TR/xpath-functions-30/#func-escape-html-uri)
* General functions on sequences: [empty#1](https://www.w3.org/TR/xpath-functions-30/#func-empty), [exists#1](https://www.w3.org/TR/xpath-functions-30/#func-exists), [head#1](https://www.w3.org/TR/xpath-functions-30/#func-head), [tail#1](https://www.w3.org/TR/xpath-functions-30/#func-tail), [insert-before#3](https://www.w3.org/TR/xpath-functions-30/#func-insert-before), [remove#2](https://www.w3.org/TR/xpath-functions-30/#func-remove), [reverse#1](https://www.w3.org/TR/xpath-functions-30/#func-reverse), [subsequence#2](https://www.w3.org/TR/xpath-functions-30/#func-subsequence), [subsequence#3](https://www.w3.org/TR/xpath-functions-30/#func-subsequence), [unordered#1](https://www.w3.org/TR/xpath-functions-30/#func-unordered)
* Function that compare values in sequences: [distinct-values#1](https://www.w3.org/TR/xpath-functions-30/#func-distinct-values), [distinct-values#2](https://www.w3.org/TR/xpath-functions-30/#func-distinct-values), [index-of#2](https://www.w3.org/TR/xpath-functions-30/#func-index-of), [index-of#3](https://www.w3.org/TR/xpath-functions-30/#func-index-of), [deep-equal#2](https://www.w3.org/TR/xpath-functions-30/#func-deep-equal). [deep-equal#3](https://www.w3.org/TR/xpath-functions-30/#func-deep-equal)
* Functions that test the cardinality of sequences: [zero-or-one#1](https://www.w3.org/TR/xpath-functions-30/#func-zero-or-one), [one-or-more#1](https://www.w3.org/TR/xpath-functions-30/#func-one-or-more), [exactly-one#1](https://www.w3.org/TR/xpath-functions-30/#func-exactly-one)
* Aggregate functions: [count#1](https://www.w3.org/TR/xpath-functions-30/#func-count), [avg#1](https://www.w3.org/TR/xpath-functions-30/#func-avg), [max#1](https://www.w3.org/TR/xpath-functions-30/#func-max), [min#1](https://www.w3.org/TR/xpath-functions-30/#func-min), [sum#1](https://www.w3.org/TR/xpath-functions-30/#func-sum)
* Serializing functions: [serialize#1](https://www.w3.org/TR/xpath-functions-30/#func-serialize) (unary)
* Context information: [current-dateTime#1](https://www.w3.org/TR/xpath-functions-30/#func-current-dateTime), [current-date#1](https://www.w3.org/TR/xpath-functions-30/#func-current-date), [current-time#1](https://www.w3.org/TR/xpath-functions-30/#func-current-time), [implicit-timezone#1](https://www.w3.org/TR/xpath-functions-30/#func-implicit-timezone), [default-collation#1](https://www.w3.org/TR/xpath-functions-30/#func-default-collation)
* Constructor functions: for all builtin types, with the name of the builtin type and unary. Equivalent to a cast expression.

## Equality and identity <a href="#chapter-equality-and-identity" id="chapter-equality-and-identity"></a>

As in most language, one can distinguish between physical equality and logical equality.

Atomics can only be compared logically. Their physically identity is totally opaque to you.

### Logical comparison of two atomics

```

1 eq 1
    
```

Result (run with Zorba):true

### Logical comparison of two atomics

```

1 eq 2
    
```

Result (run with Zorba):false

### Logical comparison of two atomics

```

"foo" eq "bar"
    
```

Result (run with Zorba):false

### Logical comparison of two atomics

```

"foo" ne "bar"
    
```

Result (run with Zorba):true

Two objects or arrays can be tested for logical equality as well, using deep-equal(), which performs a recursive comparison.

### Logical comparison of two JSON items

```

deep-equal({ "foo" : "bar" }, { "foo" : "bar" })
    
```

Result (run with Zorba):true

### Logical comparison of two JSON items

```

deep-equal({ "foo" : "bar" }, { "bar" : "foo" })
    
```

Result (run with Zorba):false

The physical identity of objects and arrays is not exposed to the user in the core JSONiq language itself. Some library modules might be able to reveal it, though.

## Notes <a href="#chapter-areas-of-confusion" id="chapter-areas-of-confusion"></a>

### Sequences vs. Arrays <a href="#sequencesvsarrays.d12e6309" id="sequencesvsarrays.d12e6309"></a>

Even though JSON supports arrays, JSONiq uses a different construct as its first class citizens: sequences. Any value returned by or passed to an expression is a sequence.

The main difference between sequences and arrays is that sequences are completely flat, meaning they cannot contain other sequences.

Since sequences are flat, expressions of the JSONiq language just concatenate them to form bigger sequences.

This is crucial to allow streaming results, for example through an HTTP session.

#### Flat sequences

```

( (1, 2), (3, 4) )
      
```

Result (run with Zorba):1 2 3 4

Arrays on the other side can contain nested arrays, like in JSON.

#### Nesting arrays

```

[ [ 1, 2 ], [ 3, 4 ] ]
      
```

Result (run with Zorba):\[ \[ 1, 2 ], \[ 3, 4 ] ]

Many expressions return single items - actually, they really return a singleton sequence, but a singleton sequence of one item is considered the same as this item.

#### Singleton sequences

```

1 + 1
      
```

Result (run with Zorba):2

This is different for arrays: a singleton array is distinct from its unique member, like in JSON.

#### Singleton sequences

```

[ 1 + 1 ]
      
```

Result (run with Zorba):\[ 2 ]

An array is a single item. A (non-singleton) sequence is not. This can be observed by counting the number of items in a sequence.

#### count() on an array

```

count([ 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } ])
      
```

Result (run with Zorba):1

#### count() on a sequence

```

count( ( 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } ) )
      
```

Result (run with Zorba):4

Other than that, arrays and sequences can contain exactly the same members (atomics, arrays, objects).

#### Members of an array

```

[ 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } ]
      
```

Result (run with Zorba):\[ 1, "foo", \[ 1, 2, 3, 4 ], { "foo" : "bar" } ]

#### Members of an sequence

```

( 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } )
      
```

Result (run with Zorba):1 foo \[ 1, 2, 3, 4 ] { "foo" : "bar" }

Arrays can be converted to sequences, and vice-versa.

#### Converting an array to a sequence

```

[ 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } ] []
      
```

Result (run with Zorba):1 foo \[ 1, 2, 3, 4 ] { "foo" : "bar" }

#### Converting a sequence to an array

```

[ ( 1, "foo", [ 1, 2, 3, 4 ], { "foo" : "bar" } ) ]
      
```

Result (run with Zorba):\[ 1, "foo", \[ 1, 2, 3, 4 ], { "foo" : "bar" } ]

### Null vs. empty sequence <a href="#nullvsemptysequence.d12e6437" id="nullvsemptysequence.d12e6437"></a>

Null and the empty sequence are two different concepts.

Null is an item (an atomic value), and can be a member of an array or of a sequence, or the value associated with a key in an object. Sequences cannot, as they represent the absence of any item.

#### Null values in an array

```

[ null, 1, null, 2 ]
      
```

Result (run with Zorba):\[ null, 1, null, 2 ]

#### Null values in an object

```

{ "foo" : null }
      
```

Result (run with Zorba):{ "foo" : null }

#### Null values in a sequence

```

(null, 1, null, 2)
      
```

Result (run with Zorba):null 1 null 2

If an empty sequence is found as an object value, it is automatically converted to null.

#### Automatic conversion to null.

```

{ "foo" : () }
      
```

Result (run with Zorba):{ "foo" : null }

In an arithmetic opration or a comparison, if an operand is an empty sequence, an empty sequence is returned. If an operand is a null, an error is raised except for equality and inequality.

#### Empty sequence in an arithmetic operation.

```

() + 2
      
```

Result (run with Zorba):

#### Null in an arithmetic operation.

```

null + 2
      
```

Result (run with Zorba):An error was raised: arithmetic operation not defined between types "js:null" and "xs:integer"

#### Null and empty sequence in an arithmetic operation.

```

null + ()
      
```

Result (run with Zorba):

#### Empty sequence in a comparison.

```

() eq 2
      
```

Result (run with Zorba):

#### Null in a comparison.

```

null eq 2
      
```

Result (run with Zorba):false

#### Null in a comparison.

```

null lt 2
      
```

Result (run with Zorba):true

#### Null and the empty sequence in a comparison.

```

null eq ()
      
```

Result (run with Zorba):

#### Null and the empty sequence in a comparison.

```

null lt ()
      
```

Result (run with Zorba):

## Open Issues <a href="#chapter-open-issues" id="chapter-open-issues"></a>

The [JSON update syntax](http://jsoniq.org/docs/JSONiqExtensionToXQuery/html/section-json-updates.html) was not integrated yet into the core language. This is planned, and the syntax will be simplified (no json keyword, dot lookup allowed here as well).

The semantics for the [JSON serialization method](http://jsoniq.org/docs/JSONiqExtensionToXQuery/html/section-json-serialization.html) is the same as in the JSONiq Extension to XQuery. It is still under discussion how to escape special characters with the Text output method.
