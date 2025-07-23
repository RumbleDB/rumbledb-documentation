# Modules

Module

![](../../.gitbook/assets/Module.png)

You can group functions and variables in separate library modules.

MainModule

![](../../.gitbook/assets/MainModule.png)

Up to now, everything we encountered were main modules, i.e., a prolog followed by a main query.

LibraryModule

![](../../.gitbook/assets/LibraryModule.png)

A library module does not contain any query - just functions and variables that can be imported by other modules.

A library module must be assigned to a namespace. For convenience, this namespace is bound to an alias in the module declaration. All variables and functions in a library module must be prefixed with this alias.

### A library module

```

module namespace my = "http://www.example.com/my-module";
declare variable $my:variable := { "foo" : "bar" };
declare variable $my:n := 42;
declare function my:function($i as integer) { $i * $i };
    
```

ModuleImport

![](../../.gitbook/assets/ModuleImport.png)

Here is a main module which imports the former library module. An alias is given to the module namespace (my). Variables and functions from that module can be accessed by prefixing their names with this alias. The alias may be different than the internal alias defined in the imported module.

### An importing main module

```

import module namespace other= "http://www.example.com/my-module";
other:function($other:n)
    
```

Result (run with Zorba):1764
