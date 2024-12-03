# Bash

## `getopts`

```sh
while getopts 'lha:' OPTION; do
  case "$OPTION" in
    l) echo "linuxconfig" ;;
    h) echo "you have supplied the -h option" ;;
    a)
      avalue="$OPTARG"
      echo "The value provided is $OPTARG"
      ;;
    ?)
      echo "script usage: $(basename $0) [-l] [-h] [-a somevalue]" >&2
      exit 1
      ;;
  esac
done
```

## Unofficial Strict Mode

[Source](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

```sh
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'
# Note that `set +e` is the syntax to disables variable strictness. This is
# particularly helpful if you need to source a script that violates any of these
# `set`s.
```

- `set -e` will immediately exit the script with a non-zero exit code if any
command exits with a non-zero exit code
  - Be warned that some commands can return non-zero even though it's fine, like
  `grep` without any matches. Here, you don't need to `set +e`, you can `|| true`
  to ensure a successful exit code from the command.
  - If your script needs to clean up unmanaged resources or otherwise cleanup
  upon closure, [Bash Exit Traps](http://redsymbol.net/articles/bash-exit-traps/)
  can be used just like a `finally` block. There can only be one exit func at a
  time, and subsequent registrations will replace the prior registration.

  ```sh
  scratch=$(mktemp -d -t tmp.XXXXXXXXXX)
  function finish {
    echo "Cleaning up $scratch"
    rm -rf "$scratch"
  }
  trap finish EXIT
  ```

  - Short-circuiting (chaining `&&`s and `||`s) will cause the line to evaluate
  to the a failure in any fail. However, when doing `CONDITION && COMMAND`,
  this can be undesirable as the last line of the script will determine the
  script's exit code. So don't short-circuit on the last line, if ever (use the
  normal if-statement syntax).

- `set -u` will cause the script to exit with a non-zero exit code when
attempting to use an undefined variable with error message `unbound variable`.
This includes positional arguments (`$1, $2, etc`) and excludes `$*` and `$@`.
Here's how to get those positional arguments:

  ```sh
  # ${VAR_NAME:-DEFAULT_VALUE} is essentially C#'s `??` operator and plays nice
  # w/ `set -u`.
  name=${1:-}
  # Recall that `-z VAR` evals to true if the string is empty
  if [[ -z "$name" ]]; then
      echo "usage: $0 NAME"
      exit 1
  fi
  echo "Hello, $name"
  ```

- `set -o pipefile` will cause pipelines to return a non-zero exit code when any
of the commands return non-zero, and short circuit the pipeline when any command
fails
- `IFS=$'\n\t'` tells Bash to split on newlines and tabs (which is different
from the default behavior as the default is to split on spaces, newlines, and
tabs). This is generally more helpful, especially when file names have spaces.

  ```sh
  IFS=$'\n\t'

  names=(
    "Aaron Maxwell"
    "Wayne Gretzky"
    "David Beckham"
    "Anderson da Silva"
  )

  for name in ${names[@]}; do
    echo "$name"
  done

  # Aaron Maxwell
  # Wayne Gretzky
  # David Beckham
  # Anderson da Silva
  ```

