
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

```yuml
//{type: class}
 [Pizza]
```


