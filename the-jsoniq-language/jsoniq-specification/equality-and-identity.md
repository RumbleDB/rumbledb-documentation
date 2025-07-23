# Equality and identity

As in most language, one can distinguish between physical equality and logical equality.

Atomics can only be compared logically. Their physically identity is totally opaque to you.

### Logical comparison of two atomics

```

1 eq 1
    
```

Result (run with Zorba):true

### Logical comparison of two atomics

```

1 eq 2
    
```

Result (run with Zorba):false

### Logical comparison of two atomics

```

"foo" eq "bar"
    
```

Result (run with Zorba):false

### Logical comparison of two atomics

```

"foo" ne "bar"
    
```

Result (run with Zorba):true

Two objects or arrays can be tested for logical equality as well, using deep-equal(), which performs a recursive comparison.

### Logical comparison of two JSON items

```

deep-equal({ "foo" : "bar" }, { "foo" : "bar" })
    
```

Result (run with Zorba):true

### Logical comparison of two JSON items

```

deep-equal({ "foo" : "bar" }, { "bar" : "foo" })
    
```

Result (run with Zorba):false

The physical identity of objects and arrays is not exposed to the user in the core JSONiq language itself. Some library modules might be able to reveal it, though.
