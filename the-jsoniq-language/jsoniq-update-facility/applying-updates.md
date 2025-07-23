# Applying updates

At the end of an updating program, the resulting PUL is applied with upd:applyUpdates (part of the XQuery Update Facility standard), which is extended as follows:

* First, before applying any update, each update primitive (except the jupd:insert-into-object primitives, which do not have any target) locks onto its target by resolving the selector on the object or array it updates. If the selector is resolved to the empty sequence, the update primitive is ignored in step 2. After this operation, each of these update primitives will contain a reference to either the pair (for an object) or the value (for an array) on or relatively to which it operates.
* Then each update primitive is applied, using the target references that were resolved at step 1. The order in which they are applied is not relevant and does not affect the final instance of the data model. After applying all updates, an error jerr:JNUP0006 is raised upon pair name collision within the same object.
