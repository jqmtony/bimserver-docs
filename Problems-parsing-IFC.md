# Parsing strings

The STEP specification on Strings is not clear on the following. The order of parsing all the variants of alternative encodings is not defined. For example the following string could be parsed in 2 ways:

```
\S\X\S\r\S\s\S\j
```

```
\S\X
\S\r
\S\s
\S\j
```

OR

```
\S
\X\S\
r
\S\s
\S\j
```

Which will result in an error because the 2 characters following the \X\ are not numeric.