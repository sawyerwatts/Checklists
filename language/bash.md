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
#!/usr/bin/env bash

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

## find

- `-exec CMD {} +` replaces {} with all matching file names
- `-exec CMD {} \;` replaces {} with one file name and runs the command many times

## Recursively Crawling a Directory

`find` is the way to crawl directories, but `-exec` (and `xargs`?) don't support failing the script
if an error occurs, so here's a much better way to do this:

```shell
# You may need to add `IFS= ` immediately before `read` depending on the content of the file names.
find "./$db" -type f -name "*.sql" -print0 | while IFS= read -r -d '' file
do
  echo "$file"
done
```

## xargs

xargs is a essentially a foreach operator.

Consider the following commands.

```sh
$ ls
PS_COVERAGE_20240101.txt PS_PATIENT_20240101.txt

$ ls -1 \
| xargs --max-procs=$(ls -1 | wc -l) -I {} sh -c \
    'cat $1 | sed "s/^PUT //g" | jq > $(echo $1 | cut -d"_" -f2).json' - '{}'

$ rm *.txt

$ ls
COVERAGE.json PATIENT.json
```

`-n N` is another way to specify how many args per line to consume.

`-p` is a great debugging tool.

```sh
# Here's an example of having xargs call a func. Note that export -f appears to
# be required.
function foo() {
	echo 'yay!'
	echo $1
}
export -f foo

foo 'pre-call'

ls -1 | xargs -I {} bash -c 'foo "{}"'
```

## find + xargs

Find and xargs play very nice by utilizing null-delimiters (`-print0` and `-0`). Here's an example
(that also uses a function).

```shell
echo -e "\n> Finding and applying SQL files"
function apply_sql_file()
{
  db=${1:-}
  if [[ -z "$db" ]]
  then
    echo "Missing db name"
    exit 1
  fi

  file_name=${2:-}
  if [[ -z "$file_name" ]]
  then
    echo "Missing file name"
    exit 1
  fi

  echo -e "\n> Applying file $file_name"
  /opt/mssql-tools18/bin/sqlcmd -C -S localhost -U SA -P "$MSSQL_SA_PASSWORD" -d "$db" -i "$file_name"
}
export -f apply_sql_file
find "./$db/" -name "*.sql" -print0 | xargs -0 -I{} bash -c 'apply_sql_file $0 $1' $db {}
```

## foreach n grep

```shell
xs=$(some grep)
for x in $xs
do
done

```

