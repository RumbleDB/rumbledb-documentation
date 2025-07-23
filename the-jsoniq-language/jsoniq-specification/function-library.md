# Function library

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
