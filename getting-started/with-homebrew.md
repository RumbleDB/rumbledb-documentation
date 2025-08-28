# With homebrew

##

It is also possible to use RumbleDB with brew, however there is currently no way to adjust memory usage. To install RumbleDB with brew, type the commands:

```
brew tap rumbledb/rumble
brew install --build-from-source rumble
```

You can test that it works with:

```
rumbledb run -q '1+1'
```

Then, launch a JSONiq shell with:

```
rumbledb repl
```



The RumbleDB shell appears:

```
    ____                  __    __     ____  ____ 
   / __ \__  ______ ___  / /_  / /__  / __ \/ __ )
  / /_/ / / / / __ `__ \/ __ \/ / _ \/ / / / __  |  The distributed JSONiq engine
 / _, _/ /_/ / / / / / / /_/ / /  __/ /_/ / /_/ /   2.0.0 "Lemon Ironwood" beta
/_/ |_|\__,_/_/ /_/ /_/_.___/_/\___/_____/_____/  


Master: local[*]
Item Display Limit: 200
Output Path: -
Log Path: -
Query Path : -

rumble$
```

You can now start typing simple queries like the following few examples. Press _three times_ the return key to execute a query.

```
"Hello, World"
```

or

```
 1 + 1
 
```

or

```
 (3 * 4) div 5
 
```
