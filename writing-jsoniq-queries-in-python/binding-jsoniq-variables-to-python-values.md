# Binding JSONiq variables to Python values

It is possible to bind a JSONiq variable to a tuple of native Python values and then use it in a query. JSONiq, variables are bound to sequences of items, just like the results of JSONiq queries are sequence of items. A Python tuple will be seamlessly converted to a sequence of items by the library.  Currently we only support strs, ints, floats, booleans, None, and (recursively) lists and dicts. But if you need more (like date, bytes, etc) we will add them without any problem. JSONiq has a rich type system.

```python
rumble.bind('$c', (1,2,3,4, 5, 6))
print(rumble.jsoniq("""
for $v in $c
let $parity := $v mod 2
group by $parity
return { switch($parity)
         case 0 return "even"
         case 1 return "odd"
         default return "?" : $v
}
""").json())

rumble.bind('$c', ([1,2,3],[4,5,6]))
print(rumble.jsoniq("""
for $i in $c
return [
  for $j in $i
  return { "foo" : $j }
]
""").json())

rumble.bind('$c', ({"foo":[1,2,3]},{"foo":[4,{"bar":[1,False, None]},6]}))
print(rumble.jsoniq('{ "results" : $c.foo[[2]] }').json())
```

It is possible to bind only one value. The it must be provided as a singleton tuple. This is because in JSONiq, an item is the same a sequence of one item.

```python
rumble.bind('$c', (42,))
print(rumble.jsoniq('for $i in 1 to $c return $i*$i').json())
```

For convenience and code readability, you can also use bindOne().

```python
rumble.bindOne('$c', 42)
print(rumble.jsoniq('for $i in 1 to $c return $i*$i').json())
```
