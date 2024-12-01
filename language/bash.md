# Bash

## Unofficial Strict Mode

[Source](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

```sh
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'
# Note that `set +e` is the syntax to disables variable strictness.
```

- `set -e` will immediately exit the script with a non-zero exit code if any
command exits with a non-zero exit code
- `set -u` will cause the script to exit with a non-zero exit code when
attempting to use an undefined variable
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

