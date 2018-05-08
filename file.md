# Files system of Bash
## About cd and dir
`cd ..` opens the parent directry  
`cd ~` opens the home directry  
`ls -a` shows all files  
`mkdir` makes new dir  
`rmdir` removes empty dir  
`rmdir -r` removes dir and its content  
`rm FILE` removes a file

## Input and output stream
**stdin** -- can be redirected with `<`  
**stdout** -- can be redirected with `>`  
**stderr** -- can be redirected with `2>`

## Tips of viewing a file
`cat FILENAME` displays the content without opening it  
`head -x FILENAME` displays the first x lines  
`tail -x FILENAME` displays the last x lines  
`uniq FILENAME` removes all adjacent duplicates  
`sort FILENAME` sorts all lines

**Can use pipe `|` to connect output to input**