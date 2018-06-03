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

## Constructors   
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

* a default constructor
* a copy constructor
* a copy of assigment operator
* a deconstructor
* a move consturctor 
* a move assignment operator

## Copy constructor:
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