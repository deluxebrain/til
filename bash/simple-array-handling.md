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

This allows arrays to be populated as follows:

```shell
declare -a options

# as Bash arrays are zero indexed, the current count points to the next insert index
options[${#options[@]}]="Some value"
```

## Arrays done right

Arrays can be constructed and appended as follows:

```shell
options=( list )
options+=( list )
```

In the case of array construction, the `list` used to construct the array is a string that is separated on a certain character. The string splitting is done on the `IFS` environment variable, which defaults to *whitespace*.

e.g. 

```shell
local options
# execute in subshell to preserve IFS
(
  IFS=","
  optionstring="option 1, option 2, options 3"
  options=( $optionstring )
)
```



