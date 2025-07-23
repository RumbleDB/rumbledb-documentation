# Merging updates

In the middle of a program, several PULs can be produced against the same snapshot. They are then merged with upd:mergeUpdates (part of the XQuery Update Facility standard), which is extended as follows.

* Several deletes on the same object are replaced with a unique delete on that object, with a list of all selectors (names) to be deleted, where duplicates have been eliminated.
* Several deletes on the same array and selector (position) are replaced with a unique delete on that array and with that selector.
* Several inserts on the same array and selector (position) are equivalent to a unique insert on that array and selector with the content of those original inserts appended in an implementation-dependent order (like XQUF).
* Several inserts on the same object are equivalent to a unique insert where the objects containing the pairs to insert are merged. An error jerr:JNUP0005 is raised if a collision occurs.
* Several replaces on the same object or array and with the same selector raise an error jerr:JNUP0009.
* Several renames on the same object and with the same selector raise an error jerr:JNUP0010.
* If there is a replace and a delete on the same object or array and with the same selector, the replace is omitted in the merged PUL.
* If there is a rename and a delete on the same object or array and with the same selector, the rename is omitted in the merged PUL.

\
