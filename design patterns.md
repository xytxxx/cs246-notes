
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
