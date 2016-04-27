# Accessing positional arguments by index

Not as straight forward as I thought at first ...

1. Using array subscripts
  
  E.g. 

  ```shell
  echo ${@[n]}  # wont work!
  ```

  No real idea of why this doesn't work - though perhaps because `@` ( and `*` ) are reserved ?

2. Using array slicing

  E.g.

  ```shell
  # {array:start:len}
  echo ${@:n:1}
  ```

  Technically works but causes linter warnings as the slice operation returns an array which we are treating as a string

3. Using indirect variable expansion

  Bash uses variable indirection when the first character of a parameter is `!`. In this case, bash uses the value of the variable formed from the rest of the parameter as the name of the variable. This allows us todo the following:

  ```shell
  N=3
  echo ${!N}
  ```

