# Introduction

In this specification, we detail the JSONiq language in version 1.0. Historically, JSONiq was first created as an extension to XQuery. Later, a separate core syntax was created which makes it 100% tailored for JSON. It is the JSONiq core syntax that is detailed in this document.

The functionality directly inherited from XQuery is described on a higher level and we explicitly refer for more in-depth details to the [W3C specification](https://www.w3.org/TR/xquery-30).

### Structure of a JSONiq program. <a href="#structureofajsoniqprogram.d12e158" id="structureofajsoniqprogram.d12e158"></a>

A JSONiq program can either be a main module, which contains a query that can be executed, or a library module, which defines functions and variables that can be used in other modules.

A main or library module can be optionally prefixed with a JSONiq declaration with a version (currently 1.0) and an encoding.

Module

![](../../.gitbook/assets/Module.png)

### Main modules <a href="#mainmodules.d12e180" id="mainmodules.d12e180"></a>

A JSONiq main module is made of two parts: an optional prolog, and an expression, which is the main query.

MainModule

![](../../.gitbook/assets/MainModule.png)

The result of the main JSONiq program is the result of its main query.

In the prolog, it is possible to declare global variables and functions. Mostly, you will recognize a prolog declaration by the semi-colon it ends with. The main query does not contain semi-colons (at least in core JSONiq).

Global variables and functions can use and call each other arbitrarily, even if the dependency is further down in the prolog. If there a cycle, an error is thrown.

JSONiq largely follows the W3C standard regarding modules. The detailed specification is found [here](https://www.w3.org/TR/xquery-30/#id-query-prolog).

### Library modules <a href="#librarymodules.d12e213" id="librarymodules.d12e213"></a>

Library modules do not contain any main query, just global variables and functions. They can be imported by other modules.

A library module is introduced with a module declaration, followed by the prolog containing its variables and functions.

LibraryModule

![](../../.gitbook/assets/LibraryModule.png)

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
