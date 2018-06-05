# Classes

## What is a class?
Let's see student.cc:
```c++
struct Student {
    int assignment, final;

    float grade(){
        return assignemnt*0.4 + final*0.6;
    }
}

int main() {
    Student a {19,90};
    Student b {30,10};
    cout << s.grade() << endl;
}
```
Formally, methods take a hidden extra parameter **this**, which is a pointer to the oberct upon which the method was invoked. 

## Constructors <a id="default"></a>
The following code defines a **constructor** for Student:
```c++
Student (int assign = 0, int mt=0, int final=0) { //same name with the class
    this -> assign = assign;
    this -> mt = mt;
    this -> final = final;
}
```
After puting the constructor into the struct, we can do this in main:  
`Student s1 = Student {12,31,112};`, and:  
`Student s2 {0, 50};`  s2 has grads 0, 50, 0  
`Student s3 {};` &nbsp; s3 has grades 0, 0, 0

**Advantages of useing constructors**:  
* Default parameters.
* Overloading.
* Can do more than simply fill the fields.

**When an object is created:**
1. Space is allocated.
2. fields are constructed.
3. constructor body runs.
But there is a problem: if one of the field is an object, when we construct a Student, the default constructor for that field will be called and then the consturctor of Student will fill that field. That is redundent. Solution:

## MIL ( member initialization list )  
Let's review our Strudent class:
```c++
struct Student {
    const int id;
    int assns;
    int mt;
    int final;
    Student (int id, int ass, int mt, int final): 
        id{id}, assns{ass}, mt{mt}, final{final} //Must be in same order in which they are declared.
    {
        .....
    }
}
```
# Every class comes with:

* [a default constructor](#default)
* [a copy constructor](#copy)
* [a copy assigment operator](#copy_ass)
* [a deconstructor](#destroy)
* [a move consturctor](#move) 
* a move assignment operator

## Copy constructor: <a id = "copy"></a>
`int x = 5, int y = x`   
the second `=` is a copy constructor: it copys the value of x and constructs y.   

**To define a copy constructor:**
```c++
struct Student {
    ...
    Student (const Studeng &other):
        assns{other.assns}, mt{other.mt}, final{other.final}
        {
    }
}
```

**Another example:**
```c++
struct Node {
    int data;
    Node *next;
    Node (int data, Node *next):
        data{data}, next{next}
    {}

    Node (const Node &other):
    data{other.data};
    next{other.next? new Node{*other.next}: nullptr}
    {}
}
int main(){
    Node *n = new Node{1, new Node{2, new Node{3, nullptr}}};
    Node m = *n; // m = "1, *seconde node in n"
    Node *p = new Node{*n}; // *p = new "1, *second node in n"
}
```

**Be careful with constructors that take one parameters.**
```c++
struct Node {
    ...
    Node (int data): data{data} next{nullptr}
    {}
}
int main() {
    Node n = 4;  //valid unless we add "explicit" in front of the constructor.
    int f(Node n){
        ....
    }
    f(4);   //valid
}
```
The copy construtor is called when: 
* an object is initialized by another object
* an object is passed by value
* an object is returned by a function

## Deconstructor <a id = "destroy"></a>

When an object is destroyed, a method called the destructor runs:   
1. the body of destructor runs
2. destructors are invoked for each field that is an object in reverse declearation order
3. space is deallocated   

We define a destructor for Node:  
```c++
~Node() {
    {delete next;}
}
```


## Copy Assignemnt Operator <a id="copy_ass"></a>
```c++
Node a{1,nullptr};
Node b{2,&a};
b = a;  //copy assignment operator
```
To overload the copy assignment operator:  
```c
Node &operator=(constant Node &other){
    if (&other == this){
        return *this;      //self assignment
    }
    Node *temp = next;
    next = other.next? new Node(*other.next): nullptr;
    delete temp;      //if this is invoked, this is already defined.    
    data = other.data;   //doing delete and reassigning after we secure copying. 
    return *this;
}
```

**copy and swap idiom**:  
```c++
void swap(Node &other)
  {using std::swap;
   swap (data, other.data);
   swap (data, other.next); }

Node &operator=(const Node &other){
    Node *temp = other;
    swap(temp);
    return *this;
}
```

## Move constructor <a id="move"></a>

> if we do `Node m3 = function(n);`  
the compiler would move the result of function(n) to stack and then call copy constructor. That would be waster of memory.  
We need a move constructor.

```c++
Node(Node &&other):
    data{other.data},
    next{other.next} 
    {
    other.next=nullptr;  //otherwise when popout, other.next will be destroyed
}