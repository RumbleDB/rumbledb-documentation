# With homebrew

It is possible to install RumbleDB with [homebrew](https://formulae.brew.sh/formula/rumbledb), however there is currently no way to adjust memory usage (we are working on finding and documenting a way, possibly through environment variables that influence the memory allocated to Spark upon its launch). To install RumbleDB with brew, type the command:

```shellscript
brew install rumbledb
```

You can test that it works with:

```shellscript
rumbledb run -q '1+1'
```

For a list of all parameters available, you can use:

```shellscript
rumbledb run --help
```

Then, launch a JSONiq shell with:

```shellscript
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

For a list of all parameters available to the RumbleDB shell, you can type (outside the shell):

```shellscript
rumbledb repl --help
```
