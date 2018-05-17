# Why do we use `${var}`?

```bash
var=42
echo $var          #outputs 42
echo $variable     #error
echo ${var}iable   #outputs 42iable
echo ${var%2}iable #outputs 4iable
```

# Usage in bash script

```bash
#!/bin/bash

usage(){
    echo "usage has $(#) arguments"
}
echo "script has $(#) arguments"
usage
```
Then `./script "arg"` outputs:
```
1
0
```

# For loop
`{1..10}` means 1 2 3 4 5 6 7 8 9 10

# Make File
A tool to complie separate .cc files  
Basic format:
>target: dependencies  
[TAB]command

