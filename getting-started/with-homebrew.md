# With homebrew

It is possible to install RumbleDB with homebrew, however there is currently no way to adjust memory usage. To install RumbleDB with brew, type the command:

```
brew install rumbledb
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
 / _, _/ /_/ / / / / / / /_/ / /  __/ /_/ / /_/ /   3.0.0 "Coast Redwood" beta
/_/ |_|\__,_/_/ /_/ /_/_.___/_/\___/_____/_____/  

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
