## Scripts in Bash

`x=1` &nbsp;define x = 1  
`echo ${x} or echo $x` prints the value of x  
```bash
dir = ~/cs246
echo $dir # /USERNAME/cs246
ls ${dir} # Files in ~/cs246
```
`$PATH` is the default location custom scripts are stored    

To execute a script: 
```bash
SCRIPT_NAME     #executes SCRIPT_NAME in default locatiom
PATH_TO_SCRIPT  #executes SCRIPT in given path
```

## Example of scripts:
```bash
#!/bin/bash
#the first line states that this is a bash script
egrep "^$1$" /usr/share/dict/words 
```
>`$1` fetches the value of first argument

```bash
#!/bin/bash
egrep "^$1$" /usr/share/dict/words > dev/null #redirect the output of egrep so that it is not printed  
if [ $? -eq 0 ]; then                 
    echo Word is found
else 
    echo Word is not found
fi
```
>`dev/null` is the garbage location  
`$?` is the status of last command  
`-eq` checks equality  
**variables in shell is of the form string.**


To check if the arguments match:


```bash
if [ ${#} -ne 1]; then
    echo "Usage: $0 password" >&2
    exit 1
fi
```
>`$(#)` is the number of arguments  
`-ne` not   
`$0` the command line itself  
`>&2`: stderr

To use loops:

```bash
#!/bin/bash
x=1
while [ $x -le $1 ]; do 
    echo $x
    x=$((x+1)) 
done
```
>`-le` less or equal  
`$((...))` does arithmetic
```bash
#!/bin/bash
for name in *.c; do
    mv ${name} ${name%c}cc
done
```
>`${name%c}` removes the tailing "c"  

```bash
report() {
    if [ $2 ]; then 
        echo -n ${2}
    else 
        echo -n "This month"
    fi
    if [ $1 -eq 31 ]; then 
        echo "'s payday is on the 31st."
    else 
        echo "'s payday is on the ${1}th."
    fi
}
report $(cal $1 $2 | awk '{print $6}' | grep "[0-9]" | tail -1) $1
```

