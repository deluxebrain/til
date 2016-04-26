For a script with the following syntax:

```shell
script.sh [options] ARG1 ARG2
```

1. Parse non-positional arguments

```shell
while getopts "h:u:p" opt; do
  case "$opt" in
    h) 
      HOSTNAME="$opt" 
      ;;
    u) 
      USERNAME="$opt" 
      ;;
    p) 
      PASSWORD="$opt" 
      ;;
    \?) 
      echo "Invalid option: -$OPTARG" >&2
      exit 1
      ;;
    :) 
      echo "Option -$OPTARG requires an argument" >&2
      exit 1
      ;;
  esac
done
```

Notes on the option-string:
* Start opts with : to use silent error handling
* Arguments are postfixed with :
* Flags have no postfix

E.g, the following uses silent error handling, and expects a single flag and a single argument

```shell
getopts ":fA:"
```

2. Verify the required positional arguments were passed

```shell
if [ $(( $# - $OPTIND )) -lt 1 ]; then
  echo "Usage: $0 [options[ ARG1 ARG2"
  exit 1
fi
```

3. Retrieve poistional arguments

Positional arguments can be retrieved using array expansion:

```shell
${@:START:COUNT}
```

E.g, the following would retrieve the first and second positional arguments

```shell
ARG1=${@:OPTIND:1}
ARG2=${@:OPTIND:2}
```


