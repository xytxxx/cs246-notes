# UML <a id = "uml"></a>

 name | Vec 
------|----
fields <br> (optional) |-x: integer <br>-y: integer
methods <br> (optional) | +getX() integer,<br>...<br>...
---

\- means private  
\+ means public  

# Composition "owns-a"
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
int main (){
    Teacher *t1 = new Teacher("a");
    Teacher *t2 = new Teacher("b");
    {
        Department d;
        d.add-Teacher(t1);
        d.add-Teacher(t2);
    }   //outside this block, d is destroied.
    cout << t1->getName() << endl; //still valid.


}

```

# Inheritance "is-a" <a id="inheritance"></a>

```c++
class Book {    //superclass
    string title, author;
    int numPages;
public:
    Book(string t, string a, int n);
    bool isBig() {return numPages > 200;}
    ....
};

class Text: public Book {  //subclass
    string topic;
public:
    Text(string title, string autho, int numPages, string topic): 
        Book{title,author,numpages}, topic{topic} {};
    bool isBig() {return numPages > 300;} //different to isBig() in Book.
    ...
};

class Comic: public Book { //another subclass
    string hero;
public:
    Comic(...);
    bool isBig() {return numPages > 400;} //different to isBig() in Book.
    ...
}

int main() {
    Book b{"","",250};
    Comic c{"","",250,""};
    b.isBig() //returns true;
    c.isBig() //returns false;
}
```
If the super class has no default constructor, the subclass must invoke a non-default super class constructor in the MIL.  

**When a subclass is constructed:**
1. space is allocated
2. superclass parts are constructed
3. fields are constructed
4. constructor body runs

Comic or Text cannot access fields of Book because of `private`. We need to use the keyword `protected`. We can even have **protected accessor and mutator**.

If we do:
```c++
Book b = Comic {"", "", 40, ""};
```
The complier will **slice** the Comic into a Book and store it in a space of size of a Book (fourth parameter is dropped).
```c++
Comic c {"","",250,""};
Book *pb = &c;
Comic *pc = &c;
pb -> isBig() //returns true;
pc -> isBig() //returns false;
```
but if we do:
```c++
Class Book{
    ...
    virtual bool isBig(){ ...}
};
Class Comic: public Book{
    ...
    bool isBig() const override{...}
};
```
Then
```c++
Book *myBooks[20];
for (int i = 0; i< 20; i++){
    cout << myBooks[i]->isBig() << endl; 
    // this will display the correct output based on what exact type is each pointer. 
}
```

[own]: https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg

[has]:
https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg



        