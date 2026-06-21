---
type: Reference
title: introduction to AWK
timestamp: 2026-06-21T12:26:37+00:00
tags: [unix, learning, data processing]
---

## Intro

AWK is a light weight programming text processing language shipped with most unix operating systems
it is designed for scanning and data extraction. eg `ps -ef | awk '{print $1}'` this would print the first column in the ps output

### syntax

the awk programming language follows this pattern in it syntax

```
pattern {action}
```

the interpreter reads each line of it input and performs the action if the pattern matches

### Special patterns

1. `BEGIN {}` this action is done once before any input is read.
2. `END {}` this action is performed after all input have been processed
3. `/regex/ {}` this action is performed on every line that matches the regex
4. `expression {}` this action is performed on every line where the expression is true
5. `(none) {}` action is performed for every line

### Typical awk code

```awk
BEGIN {print "Starting..."}
/error/ {print "Found error:", $0}
END {print "Done."}
```

### Built in Variables

1. `$0` this is assigned the current line
2. `$1` this is assigned the first column
3. `$n` this is assigned the nth column
4. `$NF` this is the Number of fields, this is the number of columns
5. `$FS` this is the field separator (default: whitespace)
6. `$OFS` Output field separator
7. `$RS` Record separator, i.e. separator for each line (default: newline `\n`)
8. `$ORS` Output file Record separator,
9. `$FILENAME` name of the current input file

### Print function

```awk
{print NR, $1, $NF} #print line number, first and last field
```

### Field separator

changing field separator can be done by passing argument `-F` to the interpreter or inside the program assigning
value to FS at begin

```awk
BEGIN {FS = ","}
```

### Operator

1. Arithmetic: `+ - & / % ^`
2. Comparison: `== != < > <= >=`
3. Logical: `&& || !`
4. String concat: done by just putting string values next to each other e.g. `"Hello" "world"`
5. Match: `~` and `!~`

### Control Flow

The following statements control program flow inside blocks.

```awk

# if else
{if ( expr ) statement}
{if ( expr ) statement else statement}

# while
{while ( expr ) statement}

{do statement while ( expr )}

# for
{for ( opt_expr ; opt_expr ; opt_expr ) statement}
{for ( var in array ) statement}

# continue
continue

# break
break
```

### Data types

there are two basic data types in awk, numeric and string

- Numeric, this can be int `-2`, decimal `0.9` or scientific notion like `.2E-3` all number are represented
  internal as a float by the interpreter, so `0.2e2 == 20` is true and true is represented as `1.0`
- String constants are enclosed with double quote e.g. `"This is a string constant"`. String can be continued along multiple lines by
  escaping the `(\)` the newline
  The following escape sequences are recognized.

              \\        \
              \"        "
              \a        alert, ascii 7
              \b        backspace, ascii 8
              \t        tab, ascii 9
              \n        newline, ascii 10
              \v        vertical tab, ascii 11
              \f        formfeed, ascii 12
              \r        carriage return, ascii 13
              \ddd      1, 2 or 3 octal digits for ascii ddd
              \xhh      1 or 2 hex digits for ascii  hh

### Regular expressions

in AWK recors, fields and string are often tested for matching a regular expression. in AWK regex are enclosed in slashes

    `expr ~ /r/`

in AWK a regular expression evaluate to 1 if there is match

`/r/ {action}` and `$0 ~ /r/ {action}` are the same, they both mean action is executed if expression matches regex
AWK uses extended regular expression as with the `-E` option of grep, that is it treats key metacharacters as special operators by default

key metacharacters are `\ ^ . [ ] | ( ) * + ? { }`

### Arrays

AWK provides one-dimensional arrays. the elements are expressed as `arr[expr]`, `expr` is internally converted to string type. so `A[1]` is same as `A[""]`
Arrays index by strings are called associative arrays like HashMaps in other languages.
Multi-dimension arrays are represented using composite of keys to access a value

### Built in Functions

AWK provide various built in function across different categories, more of this function can be seen in the unix manual `man awk`

#### String functions

- `gsub(r,s,t)` - global substitution, search and replace
- `index(s, t)` - return first index where t occurs
- `match(s, r)` - return index of first longest match of r in s
- `splut(s, A, r)` split string using r and load into array A
- `sprintf(format, expr-list)` return a string constructed from `expr-list` based on `format`
- `sub(r,s,t)` same as `gsub` but return just one
- `tolower` `toupper`

#### Time functions

- `mktime(specification)` converts a date specification to timestamp
- `strftime` format a given timestamp using a format

#### Arithmetic functions

- `atan2(x,y)`
- `cos(x)`
- `exp(x)`
  ... etc

#### I/O functions

there are two output functions, `print` and `printf`,
there is `getline` for input and this has different variations

#### User defined functions

syntax

```awk
function name(args) {statement}

#eg

function csplit(s, A,  n,i)
{
    n = length(s)
    for(i = 1, i <= n; i++) A[i] = substr(s, i, 1)
    return n
}

# putting extra space before between passed arguments and local variables is convention, the function name and the '(' must touch to prevent confusion with concatenation
```

### resource

- unix manual
