# Input and Output
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
```c++
#include <iostream>
using namespace std;

int main () { //reads all integers and echo to stdout until EOF, skip non-integer inputs 
  int i;
  while (true) {
    if (!(cin >> i)) {
      if (cin.eof()) break;
      else {
        cin.clear(); //clear error history
        cin.ignore(); //ignore the current character
      }
    }
    else {
      cout << i << endl;
    }
  }
}
```

# I/O manipulation

`<iomanip>`   
* `setw(min)` specifies the minmimum number of spaces for next value to print.  
```c++
int x = 5;
int y = 192;
cout << x <<y << end1; //prints 5192
cout << x << " " << y << end1; //prints 5 192
cout << setw(4) << x << setw(6) << y << end1; //prints "   5   192"
```
* `hex, dec, oct`: causes all the subsequent integer to be printed/
```c++
cout << hex << 95;  //prints 5f
cout << dec << 95;  //prints 9s 
```

# C++ Strings
* they grow as needed
* safe to manipulate strings. `string s = "hellp"`
* `using namespace std` saves you from writting `std::cin`, etc.

## Srting operations:
* `include <strings>`  
* can use `==  != > < >= <=` 
* `s.length()` works, so does `s[0], s[1]....`
* can concatenate using `+`
* `cin >> s` Reads a string, skip leading spaces, stop at next white space.
* `cout << s` Prints the string
* `getline(cin,s)` Reads in one line

# Reading from a file
`std::ifstream` to read from a file  
`std::ofstream` to write to a file   
`include <fstream>`  
```c++
#include <iostream>
#include <fstream>
using namespace std;

int main () {
  ifstream file{"suite.txt"};
  string s;
  while (file >> s) {
    cout << s << endl;
  }
} //file is closed when it goes out of scope
```
# Outout string stream
Allows user to write strings to it without printing.  
```c++
include <sstream>
ostringstream ss;  //output string stream
int hi; int lo;
ss << "Enter a value between " << hi << "and" << lo << end;
cout << ss.str();
```

# Default function parameters
Let's define a function:
```c++
void print(string filename = "default.txt"){
  .........
}
int main() {
  print("xxx.txt");  //prints xxx.txt
  print(); //prints default.txt
}
```
When multiple parameters, put the ones with no default values first.

# Overloading
- same as in JAVA  

**BUT WHEN USE DEFAULT PARAMETERS**
```c++
int f (int a, int n = 1){}
int f (int a){}
```
**WILL BE WRONG**

