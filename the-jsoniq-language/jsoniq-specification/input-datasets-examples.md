# Input datasets (examples)

Even though you can build your own JSON values with JSONiq by copying-and-pasting JSON documents, most of the time, your JSON data will come from an external input dataset.

How this dataset is access depends on the JSONiq implementation and of the context. Some engines can read the data from a file located on a file system, local or distributed (HDFS, S3); some others get data from the Web; some others are full-fledged datastores and have collections that can be created, queried, modified and persisted.

It is up to each engine to document which functions should be used, and how, in order to read datasets into a JSONiq Data Model instance. These functions will take implementation-defined parameters and typically return sequences of objects, or sequences of strings, or sequences of items, etc.

For the purpose of examples given in this specification, we assume that a hypothetical engine has collections that are sequences of objects, identified by a name which is a string. We assume that there is a collection() function that returns all objects associated with the provided collection name.

We assume in particular that there are three example collections, shown below.

### Collection 1

```

collection("one-object")
    
```

Result

```
{ "foo" : "bar" }
```

### Collection 2

```

collection("captains")
    
```

Result

```

{ "name" : "James T. Kirk", "series" : [ "The original series" ], "century" : 23 }
{ "name" : "Jean-Luc Picard", "series" : [ "The next generation" ], "century" : 24 }
{ "name" : "Benjamin Sisko", "series" : [ "The next generation", "Deep Space 9" ], "century" : 24 }
{ "name" : "Kathryn Janeway", "series" : [ "The next generation", "Voyager" ], "century" : 24  }
{ "name" : "Jonathan Archer", "series" : [ "Entreprise" ], "century" : 22 }
{ "codename" : "Emergency Command Hologram", "surname" : "The Doctor", "series" : [ "Voyager" ], "century" : 24 }
{ "name" : "Samantha Carter", "series" : [ ], "century" : 21 }
      
```

### Collection 3

```
collection("films")
```

Result

```

{ "id" : "I", "name" : "The Motion Picture", "captain" : "James T. Kirk" }
{ "id" : "II", "name" : "The Wrath of Kahn", "captain" : "James T. Kirk" }
{ "id" : "III", "name" : "The Search for Spock", "captain" : "James T. Kirk" }
{ "id" : "IV", "name" : "The Voyage Home", "captain" : "James T. Kirk" }
{ "id" : "V", "name" : "The Final Frontier", "captain" : "James T. Kirk" }
{ "id" : "VI", "name" : "The Undiscovered Country", "captain" : "James T. Kirk" }
{ "id" : "VII", "name" : "Generations", "captain" : [ "James T. Kirk", "Jean-Luc Picard" ] }
{ "id" : "VIII", "name" : "First Contact", "captain" : "Jean-Luc Picard" }
{ "id" : "IX", "name" : "Insurrection", "captain" : "Jean-Luc Picard" }
{ "id" : "X", "name" : "Nemesis", "captain" : "Jean-Luc Picard" }
{ "id" : "XI", "name" : "Star Trek", "captain" : "Spock" }
{ "id" : "XII", "name" : "Star Trek Into Darkness", "captain" : "Spock" }
      
```
