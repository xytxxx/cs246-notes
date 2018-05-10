# Intro to C++

This is "Hello World" in C.
```c
#include <stuio.h>
int main() {
    printf("Hello World\n");
    return 0;
}
```
This will compile in C++ complier but but real C++.

---

This is "Hello World" in C++:
```c++
#include <iostream>
using namespace std;

int main () {   
  cout << "Hello world" << endl;
  return 0;
}
```
**NOTE:**

* main must return int
* `using namespace std` &nbsp;&nbsp; lets you say `cout` instead of `std::cout`
* by default, main returns 0
---
## Input and Output
_We have 3 I/O streams:_
* cout: printing to stdout (default is screen)
* cin: reading from stdin (default is keyboard)
* cerr: pringing to stderr (defualt is screen)  

_I/O operations:_  

`<<` means "put to"
```c
cout << x  
cerr << "error ...."
```
`>>` means "get from"  

```c
cin >> i
```

If the read fails; `cin.fail()` will be true  
If EOF, `cin.eof()` and `cin.fail()` will be both true  
>There is an implicit conversion from cin to bool. `cin` is regarded as true unless it fails. Thus we can use `cin` as a condition.

**Example:**  
```c
#include <iostream>
using namespace std;

int main() {
  int i;
  while (!(cin >> i)) {
    cout << i << endl;
  }
}
```

---

## Loops:

* for loop is the same:
```c
#include <iostream>
using namespace std;
int main() {
  for (int i=1; i <= 10; ++i) {
    cout << i << endl;
  }
}
```
* while loop is the same:
```c
#include <iostream>
using namespace std;
int main() {
  int i;
  while (true) {
    if (!(cin >> i)) break;
    cout << i << endl;
  }
}
```
---

## Sizes of elements
bool: 8 bits  
char: 8 bits  
short: 16 bits  
int: 32 bits  
long: 64 bits   
long long: 64 bits  
float: 32 bits  
double: 64 bits  
long double: 128 bits  
unsigned char: 8 bits  
unsigned short: 16 bits  
unsigned int: 32 bits  
unsigned long: 64 bits   
unsigned long long: 64 bits 

---
## How to complie and run C++ codes?
`g++ -std=c++14 hello.cc -o hello`  
This means "complie hello.cc under c++14 standard and name the output(executable) "hello" 