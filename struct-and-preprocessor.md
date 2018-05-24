# Dynamic-memory
review: in C++, we use `new` and `delete` to deal with pointers.  
> `Node *np = new Node;`  
`delete np;`

**Array**:
```c++
Node *myNodes = new Nodes[10]; //10 nodes in heap, pointer in stack.
delete[] myNodes;
```
If we want to return a reference of a Node:
```c++
Node *getNode() {
    Node n;
    .....
    return &n;
}
```
## In C++14, use `Vector v1{1,2,3}` to initialize struct

Override opeartions on struct:
```c++
struct Vec{
    int x,y;
};
Vec operator+(const Vec &v1,const Vec &v2) {
    Vec res{v1.x+v2.x, v1.y+v2.y}
    return res;
}
int main (){
    Vec v1{1,2}
    Vec v2{3,4};
    Vec v3 = v1 + v2;
}
```
Another example:
```c++
struct Grade {
    int grade;
    ....
};
ostream &operator<<(ostream &out const Grade &g) {
    out << g.grade << "%";
    return out;
}
istream &operator>>(istream &in, Grade &g) {
  in >> g.grade;
  if (g.grade < 0) g.grade = 0;
  if (g.grade > 100) g.grade = 100;
  return in;
}

int main () {
  Grade g;
  while (cin >> g) cout << g << endl;
}
```

# Preprocessor 
## Module:
\#include \<iostream>  
`#include "xxx.yyy" `  
`#include </....../...../xxx>` include a standart library by absolute path  
`#include "/..../xxx.yyy"` **INVALID**

## Variable:
`#define x 100` **Discouraged**

## Codes:
```c
#if 0
    code
    code
    code
    code 
#end if
```
## Debug
```c++
#ifdef Name //True if Name has been defined
#ifndef Name //reverse
```
usage:
```c++
int x = 9;
while (x<10){
    .....
    ....
    ...
    ...

    #ifdef DEBUG
        cout << x << endl;  
    #endif
}
```
this will only print x if we compile in this way:  
`g++14 -DDEBUG -Dvar2 -Dvar3 main.cc` (-D)
    