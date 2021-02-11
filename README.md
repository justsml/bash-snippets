# Bash and ZSH snippets

Assorted shell script syntax &amp; examples.


## `removePath` Function

Note: Implementation is below the example **use cases**.

### Use case: Prefix homebrew's path to the $PATH

```bash
 # Note: Add this (along with function) to `~/.bashrc`, `~/.zshrc`, or similar script.
PATH="/opt/homebrew/bin:$(removePath "/opt/homebrew/bin")"
```

### Use Case: Append '/sbin' to the `$PATH`

```bash
PATH="$(removePath "/sbin"):/sbin"
```

### Use Case: Remove first path containing `ruby`

```bash
rubyPath="$(which ruby)"
rubyPath=$(dirname "$rubyPath")
PATH="$(removePath "$rubyPath")"
```

### Implementation

```bash

# `removePath [path]` - Outputs the $PATH omitting all occurences of the specified path.
function removePath () {
  if [[ "$1" == "" ]]; then
    printf "$PATH"
    return
  fi
  tmpPath="$PATH"
  if [[ "$PATH" == *":$1:"* ]]; then
    # matched PATH middle
    tmpPath="${tmpPath//:$1:/:}"
  fi
  if [[ "$PATH" == "$1:"* ]]; then
    # matched PATH start
    tmpPath="${tmpPath//:$1/}"
  fi
  if [[ "$PATH" == *":$1" ]]; then
    # matched PATH end
    tmpPath="${tmpPath//:$1/}"
  fi
  tmpPath="${tmpPath//::/:}"
  printf "$tmpPath"
  return
}
```

