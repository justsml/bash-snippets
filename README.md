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

# `removePath [path]` Outputs the $PATH omitting all occurences of the specified path.
function removePath () {
  # Check matches in middle of path
  tmpPath="$PATH"
  if [[ "$PATH" == *":$1:"* ]]; then
    echo 'matched PATH middle'
    tmpPath="${tmpPath//:$1:/:}"
  fi
  # Test start of path
  if [[ "$PATH" == "$1:"* ]]; then
    echo 'matched PATH start'
    tmpPath="${tmpPath//:$1/}"
  fi
  # Test end of path
  if [[ "$PATH" == *":$1" ]]; then
    echo 'matched PATH end'
    tmpPath="${tmpPath//:$1/}"
  fi
  tmpPath="${tmpPath//::/:}"
  printf "$tmpPath"
}
```

