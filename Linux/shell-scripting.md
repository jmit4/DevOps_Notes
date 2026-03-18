# Shell Scripting

Notes on writing bash scripts for automation.

---

## Script Structure

```bash
#!/usr/bin/env bash
# Description: What this script does
# Usage: ./script.sh [args]

set -euo pipefail   # Exit on error, unset variable, or pipe failure

main() {
  echo "Hello, $1"
}

main "$@"
```

`set -euo pipefail` is a safety best practice:
- `-e` — exit immediately on error
- `-u` — treat unset variables as errors
- `-o pipefail` — catch errors in piped commands

---

## Variables

```bash
NAME="Alice"
echo "Hello, $NAME"

RESULT=$(date)       # Command substitution
echo "$RESULT"

readonly PI=3.14     # Constant (read-only)
```

## Arguments

```bash
$0   # Script name
$1   # First argument
$#   # Number of arguments
$@   # All arguments (as separate words)
$?   # Exit status of last command
```

## Conditionals

```bash
if [ "$1" == "hello" ]; then
  echo "Hi there!"
elif [ "$1" == "bye" ]; then
  echo "Goodbye!"
else
  echo "Unknown greeting"
fi

# File checks
if [ -f "/etc/hosts" ]; then echo "File exists"; fi
if [ -d "/tmp" ];        then echo "Directory exists"; fi
if [ -z "$VAR" ];        then echo "Variable is empty"; fi
if [ -n "$VAR" ];        then echo "Variable is not empty"; fi
```

## Loops

```bash
# For loop
for i in 1 2 3 4 5; do
  echo "Item: $i"
done

# For loop over files
for file in /tmp/*.log; do
  echo "Processing $file"
done

# While loop
COUNT=0
while [ $COUNT -lt 5 ]; do
  echo "Count: $COUNT"
  COUNT=$((COUNT + 1))
done
```

## Functions

```bash
greet() {
  local name="$1"
  echo "Hello, $name!"
}

greet "World"
```

## String Operations

```bash
STR="hello world"
echo "${#STR}"            # Length: 11
echo "${STR^^}"           # Uppercase: HELLO WORLD
echo "${STR,,}"           # Lowercase: hello world
echo "${STR/world/bash}"  # Replace: hello bash
echo "${STR:0:5}"         # Substring: hello
```

## Arrays

```bash
FRUITS=("apple" "banana" "cherry")
echo "${FRUITS[0]}"        # First element
echo "${FRUITS[@]}"        # All elements
echo "${#FRUITS[@]}"       # Array length

for fruit in "${FRUITS[@]}"; do
  echo "$fruit"
done
```

## Error Handling

```bash
command_that_may_fail || {
  echo "Command failed!" >&2
  exit 1
}

# Trap errors
trap 'echo "Error on line $LINENO"' ERR
```

## Useful Snippets

```bash
# Check if running as root
if [ "$(id -u)" -ne 0 ]; then
  echo "Please run as root" >&2
  exit 1
fi

# Confirm before proceeding
read -r -p "Are you sure? [y/N] " response
if [[ "$response" != "y" ]]; then
  echo "Aborted."
  exit 0
fi

# Get script directory
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
```
