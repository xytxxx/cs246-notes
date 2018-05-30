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
The following code defines a **constructor** for Student:
```c++
Student (int assign, int mt, int final) { //same name with the class
    this -> assign = assign;
    this -> mt = mt;
    this -> final = final;
}
```
After puting the constructor into the struct, we can do this in main:  
`Student s1 = Student {12,31,112};`  
