# Parttern matching for a file
new command --- egrep:  
```bash
egrep [OPTIONS] pattern [FILE]
```
options include:
```bash
-c // print number of matching lines
-v // print all non-matching lines
-i // ignore cases
-n // prefix each line this its line number
```
Pattern is a regular expression.
___
## Regular Expressions
### Characher Classes
`[abcd]` matches any of a b c d\
`[^abcd]` avoids any of a b c d\
`[a-z]` matches any single chars from 'a' to 'z'\
`.` matches any single char **expect '\n'**\
> ex. `(..)*$` matches all lines with even number of **characters**

`\d` matches any digits\
`\s` matches any white-space char\
`\S` matches any non-white-space char

### Symbols
`^EXP` matches EXP at the beginning of the line\
`EXP$` matches EXP at the end of the line\
`EXP*` matches 0 or more occurences of EXP\
`EXP+` matches 1 or more occurences of EXP\
`EXP?` matches 0 or 1 occrences or EXP\
`EXP{n}` matches EXP exactly n times\
`EXP{n,}` matches EXP at least n times\
`EXP{n,m}` matches EXP at least n times but no more than m times

### Common combinations
`.*` matches any string\
`.+` matches any non-empty string\





