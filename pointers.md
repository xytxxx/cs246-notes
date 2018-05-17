# Pointers
Same as in C, all varaibles are stored in memory stack.  
```c
int x = 9;
int *p = &x;

int * const cp = &x;
cp = &y; //invalid
*cp = 1231; //valid

const int *cp2 = &x;
cp2 = &y; //valid
*cp2 = 209; //invalid

const int * const cp3 = &x; //cannot change anything
```
# Reference
**In C++, there is another pointer-like: reference**  
References are constant pointers  
`cin >> x` can change the value of x.
```c
int x = 5;
int &z = x;  //z is lvalue-reference, the address of x
z = 199; //changed the value of x to 199
```
>**z is an alias to x**
```c
z = x + y; //invalid because x + y is an rvalue
```
```c
int f (int &n) {....}
int g (const int &n) {....}
f(5); //invalid
g(5); //valid since cannot change value with a const reference
```

# Dynamic memory allocation 
```c
struct Node {
    int data;
    Node *next;
}
Node *np = new Node; //np allocated in the stack, Node is allocated in heap (NEED FREE)
delete np; //free (np)
```