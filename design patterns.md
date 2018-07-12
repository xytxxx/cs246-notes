
# Oberver Pattern

* publish-subscribe model 
* behavioral pattern
* one class called publisher/subject to generate data.
* one or more classes called subscribers/observers - receive data and react on it.

```c++
class Subject {
    vector<Observer*> observers;
  public:
    Subject();
    void attach(Oberver *ob) { obsservers.emplace_back(ob)}
    void detach(Observer *ob) {
        for (auto it == observers.begin(); it!=observers.end(); ++it){
            if(*it == ob) {
                observers.erase(it);
                break;
            }
        }
    }
    void notifyObservers(){
        for(auto &ob: observers) ob->notify();
    }
    virtual ~Subject() = 0
};

class Observer {
    public:
    virtual void notify() = 0;
    virtual ~Observer(){}
};
```
Sequence of method calls:
1. subject state updated
2. subject notify observers by `subject.notifyObservers`
3. each observer calls its notify function

# Decorator pattern

* structured pattern
* to attach additional responsibilities to objects dynamically (during runtime)
```c++
class BaseClass {                    //abstract
  public:
    virtual string BaseFunction() = 0;
};
class Concrete: public BaseClass {
  public:
    virtual string BaseFunction() override {return "concrete" }
};
class Decorator: public BaseClass {  //abstract
  protected: 
    BaseClass* component;
};
class ExampleDecorator: public Decorator {
    string exampleField;
  public:
    string BaseFunction() override {return component->BaseFuntion() + examplefield;}
};
int main() {
    BaseClass *p = new ExampleDecorator1{"field1", new ExampleDecorator{"field2", new Concrete}};
}
```
# Factory Method Pattern
```c++
class Level {
  public:
    virtual Enemy *createEnemy = 0;     //factory method
};

class NormalLevel: public Level {
  public:
    Enemy *createEnemy() override {
        //create mostly turtles;
    }
}

class HardLevel: public Level {
  public:
    Enemy *createEnemy() override {
        //create mostly monsters;
    }
}
```
# NVI idiom: Non - Virtual Interface
* all public methods are non-virtual
* all virtual methods are private
* maybe except the destructor
* publcic methods call private methods

# Map
Map is a template in STL.  
```c++
#include <map>
map <string, int> m;
m["abc"] = 1;
m["def"] = 4;
cout << m["abc"]; //prints 1
cout << m["ghi"]; //(ghi, 0) is added to m.
m.erase("abc");
for (auto &p : m) {   //traverse all pairs by ascending order of key
    p.first; //is key of p
    p.second; //is value of p
}
```

# Unique Pointer

> **Stack Unwinding:** all allocated-stack data is cleaned up, dtors call, memory reclaimed.

Let's look at the following code:  
```c++
void  f() {
    int *p = new int {1};
    g();
    delete p;
}
```
It will memory leak if `g()` fails because of stack rewinding.  
We can use **smart pointer**:  
`class std::unique_ptr <T> `   is a class that holds `T*`, and the dtor will free the heap-allocated data that the pointer is pointing at.

# RAII (Resource Acquisite Is Initialization)

```c++
#include <memory>
class Basic {
    public:
    int x;
    // ctor and dtor
}
int main() {
    auto bp = make_unique<Basic>(5);
    cout << bp -> x; // outputs 5.

    //==========================================
    unique_ptr <Basic> p {new Basic{1}};
    unique_ptr <Basic> p2 = p; 
    // problem: if two ptrs are destroyed in order, when it comes to p2, 
    // it will call dtor on null.
    // We should not have two unique_ptr to same thing.
    // In fact, unique_prt does not even have a copy ctor.
}
```

But what if we want a bunch of pointers to the same data?

# Shared Pointer 

It maintains a reference `count` to counted all shared pointers that are pointing to the same heap-allocated data.  
The memory is freed if `count` reaches 0.

We should use unique and shared pointers as much as possible. 