# JSONiq Update Facility

JSONiq follows the [XQuery Update Facility standard](https://www.w3.org/TR/xquery-update-30) and introduces update primitives and update expressions specific to JSON data.

In JSONiq, updates are not immediately applied. Rather, a snapshot of the current data is taken, and a list of updates, called the Pending Update List, is collected. Then, upon explicit request by the user (via specific expressions), the Pending Update List is applied atomically, leading to a new snapshot. It is also possible for an engine to persist (to the local disk, to a database management system, to a data lake...) the resulting Pending Update List after a query has been completed.

\
