# Passing arguments through to a command

Sometimes it's useful to wrap a call to a command in a script to handle pre and post handling related to the command.

For example, *packer* doesn't have native support for injecting user variables into the `autounattend.xml` file. A way around this is to pre-transform the autounattend file into a temporary file that is passed to the `packer build` command. After command execution (regardless of exit status) this temporary file needs to be deleted.

A wrapper script around the `packer build` command would need to take in the full list of positional and non-positional arguments required by packer, as well as additional arguments specifying the autounattend file to transform. Something like this:

```shell
wrapper -x <my_autounattend_file> -- /
  -var-file=<my_var_file> /
  <my_template_file>
```

Internally the script would then transform the autounattend file, and pass the arguments onto packer. Note, the `--` is used to demarcate the argument ownership between the wrapper and packer (or whatever command is being proxied).

This can be achieved as follows:

```shell
#!/bin/bash

# bring in gnu getopt (a keg only brew package that isnt linked by default)
PATH="$(brew --prefix)"/opt/gnu-getopt/bin:$PATH

# use -q as not interested in hearing about invalid options
# -n foo -> report errors from foo
# -o -> single character options
# + -> POSIX compliant mode
eval set -- "$(getopt -q -n foo -o +m:ih -- "$@")"
while test "$1" != "--"; do 
      case "$1" in
            -h) : ;;
            -i) : ;;
            -m) shift ;; # shift off the argument
      esac
      shift  # shift off the argument
done

shift  # delete the "--"
echo "remaining args are: $*"

# which we can then use to proxy a command
eval "ls $*"
```
