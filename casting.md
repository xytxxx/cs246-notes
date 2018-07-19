# Casting
## Static Cast:
This is for well-defined behavior. You know what to do and what will happen.
```c++
double b;
int a = static_cast<int> b;
```

## Reinterpret_cast
To treat something as something else.  
Except undefined behavior.   
Not commonly used.
```c++
Student s;
Turtle *t = reinterpret_cast<Turtle*>(&s);
```

## Const_cast
You are sure that `g` does not change the value `p` is pointing at.  
```c++
const int *p;
void g(int *p);
g(const_cast<int*>(p));
```

## Dynamic_cast
if `pb` is `nullptr`,  `pt` is still `nullptr`.  
Only works for classes with virtual methods.
```c++
Book *pb;
Text *pt = dynamic_cast<Text*>(pb);
```
When cast to reference, cannot pass a nullptr.