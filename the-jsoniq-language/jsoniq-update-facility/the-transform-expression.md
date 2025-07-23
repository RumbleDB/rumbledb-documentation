# The transform expression

\


<figure><img src="https://www.jsoniq.org/docs/JSONiq/webhelp/images/CopyModifyExpr.png" alt=""><figcaption></figcaption></figure>

Updates can be applied to a clone of an existing instance with the [copy-modify-return](https://www.w3.org/TR/xquery-update-30/#id-copy-modify) expression.

The content of the modify clause may build a complex Pending Update List with multiple updates. Remember that, with snapshot semantics, each update is applied against the initial snapshot, and updates do not see each other's effects.

Updating expression can also be combined with conditional expressions (in the then and else clauses), switch expressions (in the return clauses), FLWOR expressions (in the return clause), etc for more powerful queries based on patterns in the available data (from any source visible to the JSONiq query).

The updates generated inside the modify clause may only target the cloned object, i.e., the variable specified in the copy clause.

**Example 191. JSON copy-modify-return expression**

```
copy $obj := { "foo" : "bar", "bar" : [ 1,2,3 ] }
modify (
  insert json { "bar" : 123, "foobar" : [ true, false ] } into $obj,
  delete json $obj.bar,
  replace value of json $obj.foo with true
)
return $obj
      
```

**Result:** { "foo" : true, "bar" : 123, "foobar" : \[ true, false ] }

\


In the remainder of this chapter, we showcase the individual updating expressions one by one, inside a copy-modify-return expression.

Update expressions can also appear outside of a copy-modify-return expression, in which case they propagate and/or persist directly to their targets, to the extent that the context makes it meaningful and possible.

\
