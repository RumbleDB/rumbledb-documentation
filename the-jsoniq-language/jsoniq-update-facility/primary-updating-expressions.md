# Primary updating expressions

Update expressions are the visible part of JSONiq Updates in the language. Each primary updating expression contributes an update primitive to the Pending Update List being built.

**Figure 84. JSONInsertExpr**

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/JSONInsertExpr.png" alt=""><figcaption></figcaption></figure>

A JSON insert expression is used to insert new pairs into an object. It produces a _jupd:insert-into-object_ update primitive. If the target is not an object, JNUP0008 is raised. If the content is not a sequence of objects, JNUP0019 is raised. These objects are merged prior to inserting the pairs into the target, and JNDY0003 is raised if the content to be inserted has colliding keys.

**Example 192. JSON insert expression**

```
copy $obj := { "foo" : "bar" }
modify insert json { "bar" : 123, "foobar" : [ true, false ] } into $obj
return $obj
      
```

**Result:** { "foo" : "bar", "bar" : 123, "foobar" : \[ true, false ] }

\


A JSON insert expression is also used to insert a new member into an array. It produces a _jupd:insert-into-array_ update primitive. If the target is not an array, JNUP0008 is raised. If the position is not an integer, JNUP0007 is raised.

**Example 193. JSON insert expression**

```
copy $arr := { "foo" : [1,2,3,4] }
modify insert json 5 into $arr.foo at position 3
return $arr
      
```

**Result:** { "foo" : \[ 1, 2, 5, 3, 4 ] }

\


**Figure 85. JSONDeleteExpr**

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/JSONDeleteExpr.png" alt=""><figcaption></figcaption></figure>

A JSON delete expression is used to remove a pair from an object. It produces a _jupd:delete-from-object_ update primitive. If the key is not a string, JNUP0007 is raised. If the key does not exist, JNUP0016 is raised.

**Example 194. JSON delete expression**

```
copy $obj := { "foo" : "bar", "bar" : 123 }
modify delete json $obj.foo
return $obj
      
```

**Result:** { "bar" : 123 }

\


A JSON delete expression is also used to remove a member from an array. It produces a _jupd:insert-from-array_ update primitive. If the position is not an integer, JNUP0007 is raised. If the position is out of range, JNUP0016 is raised.

**Example 195. JSON delete expression**

```
copy $arr := [1,2,3,4,5,6]
modify delete json $arr[[3]]
return $arr
      
```

**Result:** \[ 1, 2, 4, 5, 6 ]

\


**Figure 86. JSONRenameExpr**

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/JSONRenameExpr.png" alt=""><figcaption></figcaption></figure>

A JSON rename expression is used to rename a key in an object. It produces a _jupd:rename-in-object_ update primitive. If the sequence on the left of the dot is not a single object, JNUP0008 is raised. If the new name is not a single string, JNUP0007 is raised. If the old key does not exist, JNUP0016 is raised.

**Example 196. JSON rename expression**

```
copy $obj := { "foo" : "bar", "bar" : 123 }
modify rename json $obj.foo as "foobar"
return $obj
      
```

**Result:** { "bar" : 123, "foobar" : "bar" }

\


**Figure 87. JSONAppendExpr**

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/JSONAppendExpr.png" alt=""><figcaption></figcaption></figure>

A JSON append expression is used to add a new member at the end of an array. It produces a _jupd:insert-into-array_ update primitive. JNUP0008 is raised if the target is not an array.

**Example 197. JSON append expression**

```
copy $obj := { "foo" : "bar", "bar" : [1,2,3] }
modify append json 4 into $obj.bar
return $obj
      
```

**Result:** { "foo" : "bar", "bar" : \[ 1, 2, 3, 4 ] }

\


**Figure 88. JSONReplaceExpr**

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/JSONReplaceExpr.png" alt=""><figcaption></figcaption></figure>

A JSON replace expression is used to replace the value associated with a certain key in an object. It produces a _jupd:replace-in-object_ update primitive. JNUP0007 is raised if the selector is not a single string. If the selector key does not exist, JNUP0016 is raised.

**Example 198. JSON replace expression**

```
copy $obj := { "foo" : "bar", "bar" : [1,2,3] }
modify replace value of json $obj.foo with { "nested" : true }
return $obj
      
```

**Result:** { "bar" : \[ 1, 2, 3 ], "foo" : { "nested" : true } }

\


A JSON replace expression is also used to replace a member in an array. It produces a _jupd:insert-in-array_ update primitive. JNUP0007 is raised if the selector is not a single position. If the selector position is out of range, JNUP0016 is raised.

**Example 199. JSON replace expression**

```
copy $obj := { "foo" : "bar", "bar" : [1,2,3] }
modify replace value of json $obj.bar[[2]] with "two"
return $obj
      
```

**Result:** { "foo" : "bar", "bar" : \[ 1, "two", 3 ] }
