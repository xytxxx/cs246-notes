# Strings in C++
* In C++, there is a string type to replace C-style character arrays.
* `#includ <string>`
* Note: In general, string in this course refers to C++-style strings. Any time that C-style
character arrays is used will be referred to explicitly.
* Common supported operations include (some as member functions of string):  
– indexed access using [] or at().  
– concatenation using +, += (both with string and with C-style strings). For +, at least
one side must be a string. For +=, it must have a string on its left.  
– lexicographical comparison using ==, !=, <, >, <=, >= (also supports C-style strings)  
– others: length, clear, substr, find
* Use the c_str() member function to access a C-style version (const char * 1 ) of the string.

```c++
string str{"Hello World"};
str[5];
str+="!";
str.length();
str.clear();
str.substr(pos, len); //pos = first char (include) len = length
str.find(...);
```

# Streams

there are two I/O streams in C++: **istream** and **ostream**.  
> `cin(stdin)    cout(stdout)   cerr(stderr)`

\<fstream>: **ifstream** and **ofstream**  
\<strings>: **istreangstream** and **ostringstream**

```c++
ifstream ifs{"file.txt"};
string str;
ifs >> str;
int i;
ifs >> i; //can fail if next char is not int
```
[see reading errors](io.md)
```c++
istringstream iss{"4 + 3"};
int l, r;
string op;
iss >> l >> op >> r;
iss.str(); //returns a C style string 
```
