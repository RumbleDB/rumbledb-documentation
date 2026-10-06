# XQuery coverage

RumbleDB 3.0 now officially supports XQuery with 95% of passing tests from the QT3 test suite. We are continuing to work on fixing the remaining 5%.

RumbleDB supports in particular:

* XML Schema validation and annotation
* Modules
* Static typing
* Typed data
* Regular expressions
* Serialization to XML, CSV, HTML, XHTML, JSON, Text
* Higher-order functions (they are even integrated with ML models for training and predicting)

The doc() function opens individual XML files as expected. We did not yet connect collection() to any layers, but you can use xml-files() instead to open many files e.g. in S3 or HDFS with Amazon EMR.
