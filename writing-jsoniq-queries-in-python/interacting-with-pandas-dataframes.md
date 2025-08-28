# Interacting with pandas DataFrames

RumbleDB can work out of the box with pandas DataFrames, both as input and (when the output has a schema) as output.

## Binding JSONiq variables to pandas DataFrames

bind() also accepts pandas dataframes

```python
data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [30,25,35]};
pdf = pd.DataFrame(data);

rumble.bind('$a',pdf);
seq = rumble.jsoniq('$a.Name')
```

The same goes for extra named parameters.

```python
data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': [30,25,35]};
pdf = pd.DataFrame(data);

seq = rumble.jsoniq('$a.Name', a=pdf)
```

## Getting the results as a pandas DataFrame

It is also possible to get the results back as a pandas dataframe with pdf() (if the output has a schema, which you can check by calling availableOutputs() and seeing if "DataFrame" is in the returned list).

```
print(seq.pdf())
```
