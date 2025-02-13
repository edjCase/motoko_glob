# Glob: Path Pattern Matching for Motoko

A glob pattern matching library for Motoko that supports a wide range of pattern matching features.

## Installation

```bash
mops install glob
```

To set up MOPS package manager, follow the instructions from the
[MOPS Site](https://j4mwm-bqaaa-aaaam-qajbq-cai.ic0.app/)

## Quick Start

```motoko
import Glob "mo:glob";

// Basic matching
Glob.match("file.txt", "*.txt"); // true
Glob.match("dir/file.txt", "**/*.txt"); // true
Glob.match("script.js", "*.txt"); // false

// Directory matching
Glob.match("/foo/bar/baz", "/foo/*/baz"); // true
Glob.match("/foo/bar/qux/baz", "/foo/**/baz"); // true
```

## Features

### Basic Wildcards

- `*` matches any number of characters within a path segment
- `?` matches exactly one character

```motoko
Glob.match("hello.txt", "h*.txt"); // true
Glob.match("hello.txt", "h????.txt"); // true
```

### Directory Wildcards

- `*/` matches a single directory level
- `**/` matches zero or more directory levels

```motoko
Glob.match("/a/b/c", "/a/*/c"); // true
Glob.match("/a/b/c/d", "/a/**/d"); // true
```

### Character Classes

- `[abc]` matches any one character listed
- `[a-z]` matches any one character in the range
- `[!abc]` matches any one character not listed

```motoko
Glob.match("file1.txt", "file[1-3].txt"); // true
Glob.match("fileA.txt", "file[ABC].txt"); // true
Glob.match("fileD.txt", "file[!ABC].txt"); // true
```

### Pattern Negation

- `!pattern` matches paths that don't match the pattern

```motoko
Glob.match("/static/public/file.js", "!/static/private/**"); // true
Glob.match("/static/private/file.js", "!/static/private/**"); // false
```

### Special Notes

- Dot files (hidden files) are included in wildcard matches by default
- Path separator normalization is handled automatically
- Supports both absolute and relative paths
- Escaping special characters with backslash

## API Reference

### `match(path : Text, pattern : Text) : Bool`

Matches a path against a glob pattern and returns true if it matches.

Parameters:

- `path`: The path to test
- `pattern`: The glob pattern to match against

Returns:

- `Bool`: True if the path matches the pattern, false otherwise

## Examples

### File Extensions

```motoko
// Match specific file types
Glob.match("/static/file.js", "/static/*.js"); // true
Glob.match("/static/file.min.js", "/static/*.js"); // true
```

### Nested Directories

```motoko
// Match files in nested directories
Glob.match("/static/css/file.css", "/static/**/*.css"); // true
Glob.match("/static/css/nested/file.css", "/static/**/*.css"); // true
```

### Complex Patterns

```motoko
// Version directories with multiple extensions
Glob.match("/static/v1.2.3/app.js", "/static/v[0-9]*\\.[0-9]*\\.[0-9]*/**/*.js"); // true

// Multiple file extensions
Glob.match("/static/styles.min.css", "/static/**/*.min.{js,css}"); // true
```
