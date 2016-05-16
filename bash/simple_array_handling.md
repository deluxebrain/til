# The very basics of Bash array handling

## Arrays the long way

Bash arrays are declared as follows:

```shell
declare -a options
```

The length of an array can be queried as follows:

```shell
length=${#options[@]}
```

This allows arrays to be constructed as follows:

```shell
declare -a options
options[$#options[@]}]="Some value"
```

## Arrays done right

Arrays can be constructed and appended as follows:

```shell
options=(...)
options+=(...)
```

