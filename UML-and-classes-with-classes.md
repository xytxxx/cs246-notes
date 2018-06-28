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

# Polymorphism 

```c++
class One {
    int x, y;
    public:
    One(int x = 0, int y = 0): x{x}, y{y} {}
};

class Two {
    int z;
    public
    Two(int x = 0, int y = 0, int z = 0): One{x,y}, z{z} {}
};

void f(One *a){
    a[0] = One{6,7};
    a[1] = One{8,9};
}

int main{
    Two myArray[2] = {{1,2,3}, {4,5,6}}; //now array is |123|456|
    f(myArray);  //Now array is |678|956|, not good.
```
**Never use array of objects polymorphically, use an array of pointers instead.**
When the object X allocates memory, be caution: if subclass Y allocates more memory, but an instance is initiallized like this:
```c++
X *myX = new Y {...};
delete myX;  
```
will cause memory leak. We need to make the destructor of superclass **virtual**.  

# Abstract class

is to organise the subclasses, cannot create any instance of it. We need at least one **pure virtual** method in superclass.

```c++
class Student {
    virtual int fees() const = 0; // =0 make the method pure virtual. 
}
Student(); // will fail.
```
Any subclass is also virtual unless it implements all virtual methods.

# About Big 5
```c++
class Book {
    //define big 5;
};

class Text: public Book {
    string topic;
    public:
    Text(const Text&other): Book{other}, topic{other.topic} {}
    Text &operator=(const Text &other) {
        Book::operator=(other);
        topic = other.topic;
        return *this;
    }
    Text(Text &&other): Book{std::move(other)}, topic{std::move(other.topic)} {}
    Text &operatror=(Text &&other) {
        Book::operator=(std::move(other));
        topic=std::move(other.topic);
        return *this;
    }
}

Text t {"Algorithms","CLRS", 500, "CS"};
Text t2 = t;
```
**We cannot override operators because that will cause mix assignement. To Override, we should always define the super class abstract**  

# Abstract Type and template
In "list.h":
```c++
template <typename T> class List {
    struct Node {
        T data;
        Node *next;
        Node (T data, NOde *next):data{data}, next{next}{}
        ~Node() {delete next;}
    };
    Node *theList = nullptr;
public:
    class Iterator {
        Node *p;
        explicit Iterator(Node *p): p{p} {}
    public:
        T &opearator*() const {return p->data;}
        Iterator operator++() {
            p = p -> next;
            return *this;
        }
        bool operator==(const Iterator &other) const {
            return p == other.p;
        }
        bool operator!=(const Iterator &other) const {
            return !(*this  == other);
        }
        friend class List <T>;
    };
    Iterator begin() {return Iterator{theList};}
    Iterator end() {return Iterator{nullptr};}
    void addToFront(T n) {theList = new Node(n, theList);}
    T ith(int i) {
        Node *cur = theList;
        for(int j = 0; j < i && cur; ++j, cur = cur->next);
        return cur->data;
    }
    ~List() {delete theList;}
};
```

We can use the standart template library (STL), which includes a  dynamic allocated array:  
`vector <int> v {4,5}`  
Then  
```c++
v.emplace_back(6); // adds 6 to the end  
v.erase(v.begin());  //erase the first item
v.pop.back();    //remove the last item
v.(v.begin()+3);  //erase the 4th item 
v.at(i);      //return item at i  (differs to v[i]: 
//v[i] index out of bound error terminates the program 
//v.at(i): throws an exception, can catch)

```


[own]: https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg

[has]:
https://upload.wikimedia.org/wikipedia/commons/9/9f/AggregationAndComposition.svg



        