# Classes <a id="classes"></a>

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
* [a move assignment operator](#move_ass)

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
> we can add methods to class outside the class definition:  
>```c++
>struct Node {
>    .....
>}
>Node :: methodOfNode(){
>    ......
>}
>```


## Copy Assignemnt Operator <a id="copy_ass"></a>
```c++
Node a{1,nullptr};
Node b{2,&a};
b = a;  //copy assignment operator
```
To overload the copy assignment operator:  
```c
Node &operator=(const Node &other){
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
```
## Move Assignment operator <a id="move_ass"></a>
```c++
Node &operator=(Node &&other){  //&&other means an R-value
    using std::swap;
    swap(data,other.data);
    swap(next,orher.next);   //job done
    return *this;     //make x=y=z possible
}
```

## Elision - Constructors:
```c++
struct Vec {
    int x,y;
}
Vec makeAVec(){
    return {0,0};   //default constructor
}

int main(){
    Vec v = makeAvec();   //should be move constructor, but compiler is smart, so {0,0} will be directly pushed to v. To avoid this, use g++ -fno-elision-constructors. 
}
```
## Other operators
```c++
sturct Vec{
    int x, y;
    Vec operator+(const Vec &other){
        return {x+other.x, y+other.y};
    }
    Vec operator*(const int k){   //handles v*9
        return {x*k, y*k};
    }
}
Vec operator*(const int k, const Vec &v){  //handles 1*v
    return v*k;
}
```
Operator that can only be members:
* operator=
* operator[]
* operator->
* operator()
* operatorT

## Mutable and Class-wide variables
we can define certain fields "mutable":
```c++
struct Student{
    int grade;
    mutable int num = 0;
    static int numStudents;    //shared among all instances of Student
    void increment() const {
        ++num;                 //possble even with "const"
        return num;
    }
    static void printNum(){    //static methods dont dependo n a specific object, they do not have implicit "this"
        cout << num << endl;
    }
    Student(int x){
        grade = x;
        num++;
    }
}
int Student:: num = 0;         //static variables are initiated outside the class
int main() {
    Student a{1};
    Student b{2};
    Student:: printNum();
}
```

Static members must be defined outside the class.  

## Wrapper class
```c++
//list.h
class List{
    struct Node;
    Node *theList = nullptr;
public:
    void addToFront(int n);
    int ith(int i);
    ~List();
}

//list.cc
#include ....
struct List:: Node {
    int data;
    Node *next;
    Node(int data, Node *next): data{data} next{next}{}
    ~Node() {delete next;}     //only works if next is on heap
}
List:: ~List(){
    delete theList;
}
void List:: addToFront(int n){
    theList = new Node{n, theList};
}
int List:: ith(int i){
    Node *curr = theList;
    for(int j = 0; j < i && curr -> next; j++){
        curr = curr -> next;
    }
    return curr -> data;
}
```
> But if we want to print the whole list, we need to call `ith()` n times, which is O(n^2). In order to do it in O(n), we need to have a **iterator**.  

# Iterator <a id="iterator"></a>
is an abstraction of an object.
```c++
class List{
    struct Node;
    Node *theList;
    .....
    public:
    class Iterator {
        
        Node *p;
        //need to define all following methods.
        explicit Iterator(Node *p):p{p} {}
        public:
        int &operator*() {
            return p->data;
        }
        Iterator &operator++(){
            p = p->next;
            return *this;
        }
        bool operator==(const Iterator &other) const {
            return p==other.p;
        }
        bool operator!=(const Iterator &other) const {
            return !(*this == other);
        }
        friend class List; //let List access private fields.
    }; 
    //these are methods of lists.
    Iterator begin() {
        return Itertor(theList);
    }
    Itertor end() {
        return Iterator(nullptr);
    }
    ....
}

int main(){
    List lst;
    lst.addToFront(1);
    ....
    for(List::Iterator it = lst.begin(); it != lst.end(); ++it){
        cout << *it << endl;
    }
}
```
If c++ can interate through a collection, we can do:
```c++
for (auto &n: collection){
    ...
}
```
