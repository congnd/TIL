### What is the different between `which` and `whereis`?

Using `whatis` we can get the answer.

On macOS, we get this response:

```shell
$ whatis whereis
whereis(1)               - locate programs

$ whatis which
which(1)                 - locate a program file in the user's path
```

In short, while `whereis` searches programs on the standards system's locations, 
`which` searches programs in the user defined paths.
