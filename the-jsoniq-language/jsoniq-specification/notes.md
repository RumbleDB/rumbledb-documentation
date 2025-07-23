# Notes

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
