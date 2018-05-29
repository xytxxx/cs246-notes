# Separate Compilation

Separate the program into modules.  
Each module include:  
* interface file (*.h)  
* implecation file (*.cc)  

In the **interface file**:   
* prototypes functions
* declarations
* structure definations
* global variables

In the **implecation file**:
* full details 
* function defination
* allocating space

## Example:
```c++
//vector.h
struct vec {
    int x;
    int y;
}
Vec operator+(....);

//-------------------------------------

//vector.cc
#include "vector.h"
Vec operator+(...){
    ...
}

//-------------------------------------

//main.cc
#include "vector.h"
int main(){
    Vec v{1,2};
    Vec v2 = v + v2;
}
```

Then we do:  
`g++14 -c vector.cc`  
`g++14 -c main.cc`  
This means **compile only, create object file (.o), do not link, do not create executable program.**   
*This will give you chance to fix stupid problem like syntax.*

>Recall that to create an executable:  
`g++14 filename.cc -o outputfileName`  

Finally, we link the files:  
`g++14 vector.o main.o`

>If we changed one of the file, for example, vector.cc, then we need to redo:
`g++14 -c vector.cc`  
`g++14 vector.o main.o`

## ! If we add a global vartiable in interface:  
`int global = 10`  
Then each time you complie a .cc that includes the interface, you define the variable global one time. Then we define it multiple times. So we need to: 
`extern int global;`  
and in main.cc:  
`global = 100;`

Another approach:   
**prevent the interface file from being included more than once:**
```c++
//vector.h
#ifndef _VEC_H_
#define _VEC_H_
    ....
    ....
    ....
#endif
