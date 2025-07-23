# JSON update primitives

A Pending Update List is an unordered list of update primitives. Update primitives are internal and do not appear in the syntax. Each kind of update primitive models one individual update to an object or an array.

JSONiq adds the following new update primitives, specific to JSON. They are similar to those defined by the XQuery Update Facility for XML.

&#x20;Update primitives within a PUL are applied with strict snapshot semantics. For examples, the positions are resolved against the array before the updates. Names are resolved on the object before the updates.

### Update primitive for objects and arrays (in collections or in memory)

_jupd:insert-into-object($o as object(), $p as object())_: Inserts all pairs of the object $p into the object $o.

_jupd:insert-into-array($a as array(), $i as xs:integer, $c as item()\*)_: Inserts all items in the sequence $c before position $i into the array $a.

_jupd:delete-from-object($o as object(), $s as xs:string\*)_: Removes the pairs the names of which appear in $s from the object $o.

_jupd:delete-from-array($a as array(), $i as xs:integer)_: Removes the item at position $i from the array $a (causes all following items in the array to move one position to the left).

_jupd:replace-in-array($a as array(), $i as xs:integer, $v as item())_: Replaces the item at position $i in the array $a with the item $v (do nothing if $i is not comprised between 1 and jdm:size($a)).

_jupd:replace-in-object($o as object(), $n as xs:string, $v as item())_: Replaces the value of the pair named $n in the object $o with the item $v (do nothing if there is no such pair).

_jupd:rename-in-object($o as object(), $n as xs:string, $p as xs:string)_: Renames the pair originally named $n in the object $o as $p (do nothing if there is no such pair).

### Update primitives at the collection level

_Credits: Dwij Dixit/Ghislain Fourny (student project at ETH)_

| Update primitive                                                                            | Description                                                                                                                                              |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _jupd:create-collection($name as string, $mode as string, $content as item()\*)_            | Creates a collection initialized with the provided items. Mode determines the kind of collection (e.g., a Hive metastore table, a delta lake file, etc). |
| _jupd:truncate-collection($name as string, $mode as string)_                                | Deletes the specified collection.                                                                                                                        |
| _jupd:edit($target as item(), $content as item())_                                          | Modifies an item in a collection into another item, preserving its identity and location.                                                                |
| _jupd:delete-in-collection($target as item())_                                              | Deletes the provided item from its collection.                                                                                                           |
| _jupd:insert-first-into-collection($name as string, $mode as string, $content as item()\*)_ | Inserts the provided items at the very beginning of the specified collection.                                                                            |
| _jupd:insert-last-into-collection($name as string, $mode as string, $content as item()\*)_  | Inserts the provided items at the very end of the specified collection.                                                                                  |
| _jupd:insert-before-into-collection($target as item, $content as item()\*)_                 | Inserts the provided items before the specified item in its collection.                                                                                  |
| _jupd:insert-last-into-collection($target as item, $content as item()\*)_                   | Inserts the provided items after the specified item in its collection.                                                                                   |



\
