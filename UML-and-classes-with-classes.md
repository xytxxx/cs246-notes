# UML <a id = "uml"></a>

 name | Vec 
------|----
fields <br> (optional) |-x: integer <br>-y: integer
methods <br> (optional) | +getX() integer,<br>...<br>...
---

\- means private  
\+ means public  

---
```c++
class Vec{
    int x,y;
    public:
        Vec (int x, int y): x{x}, y{y} {}
};

class Basis {
    Vec v1, v2;
    public: 
    Basis(): v1{0, 0}, v2{0, 0}{  //MIL is a must because if in the body, default constructor of Vec will be called and fail the program.
    }
}
```
We say Basis "owns-a" 2 Vec.
If A "owns-a" B, then:
- B has no identity outside A
- if A is destroyed, then B is destroyed
- if A is copied, then B is copied as well  
![alt text][own]

This "ownership" is called **composition**.

# Aggregation "has-a" <a id="aggregation"></a>

A "has-a" B means:
- B has an existance apart from A
- if A is desdroyed, B lives on
- if A is copied, B is not.

![alt text][has]

Example:
```c++
class Teacher {   //doesnot know which department
    string name;
    public: 
    Teacher(string name): name{name}{}
    string getName(){return name}
}

class Department{  //not responsible for destroying teachers
    Teacher *teachers[100];
    int t-num;  //number of teachers
    public:
    Department()t-num{0}{}
    void add-Teacher(Teacher *t){
        teachers[t-num]=t;
        t-num++;
    }

}


```




















[own]: https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg

[has]:
https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg



        