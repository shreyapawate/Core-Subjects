Absolutely. Since you're preparing for **technical interviews/placements**, I'll make this interview-focused rather than a textbook: **C++ language fundamentals + complete OOP + STL + memory/pointers + important interview questions**, with examples and common traps.

# C++ & OOP — Complete Technical Interview Notes

## 1. What is C++?

**C++** is a:

* General-purpose programming language
* Compiled language
* Statically typed language
* Object-oriented language
* Multi-paradigm language

It supports:

* Procedural programming
* Object-oriented programming
* Generic programming
* Functional-style programming

### C++ vs C

| C                       | C++                                             |
| ----------------------- | ----------------------------------------------- |
| Procedural              | Multi-paradigm                                  |
| No classes/objects      | Classes/objects                                 |
| `printf`, `scanf`       | `cout`, `cin`                                   |
| `malloc/free`           | `new/delete`                                    |
| No function overloading | Function overloading                            |
| No inheritance          | Inheritance                                     |
| No templates            | Templates                                       |
| `struct` mainly data    | `struct` can contain functions + access control |

---

# 2. Basic C++ Program

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello World";
    return 0;
}
```

### Important parts

```cpp
#include <iostream>
```

Includes input/output functionality.

```cpp
using namespace std;
```

Allows us to write `cout` instead of `std::cout`.

```cpp
int main()
```

Program execution starts from `main()`.

```cpp
return 0;
```

Indicates successful execution.

---

# 3. Compilation Process

C++ source code:

```text
.cpp
 ↓
Preprocessor
 ↓
Compiler
 ↓
Object Code
 ↓
Linker
 ↓
Executable
```

### Preprocessor

Handles:

```cpp
#include
#define
#ifdef
#ifndef
```

Example:

```cpp
#define PI 3.14
```

### Compiler

Converts C++ code into lower-level/object code and detects syntax/type errors.

### Linker

Combines object files and libraries.

Example:

```cpp
#include <iostream>
```

The linker resolves required library functionality.

---

# 4. Variables

```cpp
int age = 21;
float salary = 50000.5;
double pi = 3.14159;
char grade = 'A';
bool passed = true;
```

## Constants

```cpp
const int x = 10;
```

You cannot modify `x`.

```cpp
const int x = 10;
x = 20; // Error
```

---

# 5. Data Types

## Fundamental

```text
int
char
float
double
bool
void
```

## Derived

```text
array
pointer
reference
function
```

## User-defined

```text
class
struct
union
enum
```

---

# 6. Type Casting

### Implicit casting

```cpp
int x = 10;
double y = x;
```

Automatically converted.

### Explicit casting

```cpp
double x = 10.5;
int y = (int)x;
```

Modern C++:

```cpp
int y = static_cast<int>(x);
```

Important casts:

```text
static_cast
dynamic_cast
const_cast
reinterpret_cast
```

### `static_cast`

Compile-time conversion.

```cpp
double x = 10.5;
int y = static_cast<int>(x);
```

### `dynamic_cast`

Used mainly for safe downcasting in polymorphic class hierarchies.

```cpp
Base* b = new Derived();

Derived* d = dynamic_cast<Derived*>(b);
```

Requires a polymorphic base class, typically with a virtual function.

### `const_cast`

Adds/removes `const` qualification.

### `reinterpret_cast`

Low-level reinterpretation of memory/types. Use carefully.

---

# 7. Operators

## Arithmetic

```text
+  -  *  /  %
```

## Relational

```text
== != > < >= <=
```

## Logical

```text
&& || !
```

## Assignment

```text
= += -= *= /= %=
```

## Increment/decrement

```text
++ --
```

## Bitwise

```text
& | ^ ~ << >>
```

## Ternary

```cpp
condition ? value1 : value2;
```

---

# 8. `++i` vs `i++`

### Pre-increment

```cpp
++i;
```

Increment first, then use value.

### Post-increment

```cpp
i++;
```

Use value first, then increment.

Example:

```cpp
int i = 5;

cout << ++i; // 6
```

```cpp
int i = 5;

cout << i++; // 5
// i becomes 6
```

---

# 9. Conditional Statements

```cpp
if (age >= 18) {
    cout << "Adult";
}
else {
    cout << "Minor";
}
```

### Switch

```cpp
switch(choice) {
    case 1:
        cout << "One";
        break;

    case 2:
        cout << "Two";
        break;

    default:
        cout << "Invalid";
}
```

---

# 10. Loops

### for

```cpp
for(int i = 0; i < 5; i++) {
    cout << i;
}
```

### while

```cpp
while(condition) {
    // code
}
```

### do-while

```cpp
do {
    // code
} while(condition);
```

The `do-while` executes **at least once**.

---

# 11. Functions

```cpp
int add(int a, int b) {
    return a + b;
}
```

Calling:

```cpp
int result = add(2, 3);
```

---

# 12. Function Declaration vs Definition

### Declaration

```cpp
int add(int, int);
```

### Definition

```cpp
int add(int a, int b) {
    return a + b;
}
```

---

# 13. Pass by Value

Copy is passed.

```cpp
void change(int x) {
    x = 100;
}

int a = 10;
change(a);

cout << a; // 10
```

Original isn't changed.

---

# 14. Pass by Reference

```cpp
void change(int &x) {
    x = 100;
}

int a = 10;
change(a);

cout << a; // 100
```

No copy is created.

---

# 15. Pass by Pointer

```cpp
void change(int *x) {
    *x = 100;
}

int a = 10;

change(&a);
```

---

# 16. Reference

A reference is an alias for another variable.

```cpp
int x = 10;
int &ref = x;
```

Now:

```cpp
ref = 20;
```

means:

```cpp
x = 20;
```

### Important properties

A reference:

* Must be initialized
* Cannot normally be reseated to refer to another object
* Provides an alias

---

# 17. Pointer

A pointer stores an address.

```cpp
int x = 10;

int *ptr = &x;
```

```text
x       = 10
&x      = address
ptr     = address
*ptr    = 10
```

### Dereferencing

```cpp
cout << *ptr;
```

Outputs:

```text
10
```

---

# 18. Pointer vs Reference

| Pointer                      | Reference                    |
| ---------------------------- | ---------------------------- |
| Stores address               | Alias                        |
| Can be `nullptr`             | Normally cannot be null      |
| Can change what it points to | Cannot be reseated           |
| Uses `*`                     | No dereference syntax needed |
| Pointer arithmetic possible  | No pointer arithmetic        |

---

# 19. Null Pointer

Modern C++:

```cpp
int *ptr = nullptr;
```

Prefer:

```cpp
nullptr
```

over:

```cpp
NULL
```

because `nullptr` has a dedicated pointer type.

---

# 20. Dangling Pointer

Pointer pointing to invalid memory.

```cpp
int* ptr = new int(10);

delete ptr;

// ptr is now dangling
```

Safer:

```cpp
delete ptr;
ptr = nullptr;
```

---

# 21. Dynamic Memory

### Stack

```cpp
int x = 10;
```

### Heap

```cpp
int *p = new int(10);
```

Release:

```cpp
delete p;
```

Array:

```cpp
int *arr = new int[5];

delete[] arr;
```

---

# 22. Memory Leak

Memory allocated but never released.

```cpp
int *p = new int(10);

// forgot delete
```

This causes a memory leak.

Modern C++ prefers **RAII and smart pointers** instead of manually managing ownership with raw `new/delete`.

---

# 23. Stack vs Heap

| Stack                              | Heap                               |
| ---------------------------------- | ---------------------------------- |
| Automatic storage                  | Dynamic storage                    |
| Usually faster                     | Usually more overhead              |
| Limited                            | Larger/flexible                    |
| Automatically managed              | Historically manually managed      |
| Local variables commonly live here | Dynamic objects commonly live here |

---

# 24. Arrays

```cpp
int arr[5] = {1,2,3,4,5};
```

Access:

```cpp
arr[0];
```

Array indexing is zero-based.

---

# 25. Strings

### C-style string

```cpp
char name[] = "Shreya";
```

### C++ string

```cpp
string name = "Shreya";
```

Need:

```cpp
#include <string>
```

Useful functions:

```cpp
name.length();
name.size();
name.substr();
name.find();
name.append();
```

---

# 26. `cin`, `cout`, `getline`

```cpp
cin >> name;
```

Reads until whitespace.

```cpp
getline(cin, name);
```

Reads the entire line.

Common issue:

```cpp
cin >> age;
getline(cin, name);
```

The newline may remain in the input buffer.

Use:

```cpp
cin.ignore();
getline(cin, name);
```

---

# 27. Struct

```cpp
struct Student {
    string name;
    int age;
};
```

By default, members of a `struct` are **public**.

```cpp
Student s;
s.age = 21;
```

---

# 28. Class

```cpp
class Student {
private:
    int age;

public:
    void setAge(int a) {
        age = a;
    }

    int getAge() {
        return age;
    }
};
```

By default, class members are **private**.

---

# 29. Class vs Struct

| Class                               | Struct                                   |
| ----------------------------------- | ---------------------------------------- |
| Default access = private            | Default access = public                  |
| Commonly used for OOP/encapsulation | Commonly used for simple data aggregates |
| Supports all OOP features           | Supports all OOP features                |

Important: `struct` in C++ is much more capable than a C struct.

---

# 30. OOP

**Object-Oriented Programming** organizes software around objects containing:

```text
Data + Behavior
```

Main OOP concepts:

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

---

# 31. Class and Object

### Class

Blueprint/template.

```cpp
class Car {
public:
    string color;

    void drive() {
        cout << "Driving";
    }
};
```

### Object

Instance of a class.

```cpp
Car c1;
Car c2;
```

Here:

```text
Car = class
c1, c2 = objects
```

---

# 32. Encapsulation

Wrapping data and methods together inside a class and controlling access to the data.

```cpp
class BankAccount {
private:
    double balance;

public:
    void deposit(double amount) {
        if(amount > 0)
            balance += amount;
    }

    double getBalance() {
        return balance;
    }
};
```

The user cannot directly modify:

```cpp
balance
```

because it's private.

### Benefits

* Data protection
* Controlled access
* Maintainability
* Reduced coupling

---

# 33. Access Specifiers

Three main access specifiers:

```text
public
private
protected
```

### public

Accessible from outside.

```cpp
class A {
public:
    int x;
};
```

### private

Accessible only inside the class and its friends.

### protected

Accessible inside:

* class
* derived classes
* friends

But not normally through an ordinary outside object.

---

# 34. Abstraction

Showing only essential details while hiding implementation.

Example:

```cpp
car.start();
```

User doesn't need to know the internal engine-starting mechanism.

In C++, abstraction can be implemented using:

* Classes
* Access control
* Abstract classes
* Interfaces through pure virtual functions

---

# 35. Encapsulation vs Abstraction

Very common interview question.

| Encapsulation                             | Abstraction                                  |
| ----------------------------------------- | -------------------------------------------- |
| Bundles data + methods                    | Hides implementation complexity              |
| Controls access                           | Shows essential functionality                |
| Achieved through classes/access modifiers | Achieved through abstract classes/interfaces |
| Focus = data protection                   | Focus = complexity hiding                    |

---

# 36. Inheritance

A derived class acquires properties/behavior from a base class.

```cpp
class Animal {
public:
    void eat() {
        cout << "Eating";
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Barking";
    }
};
```

```cpp
Dog d;

d.eat();
d.bark();
```

---

# 37. Types of Inheritance

### Single

```text
A
|
B
```

### Multilevel

```text
A
|
B
|
C
```

### Multiple

```text
A     B
 \   /
   C
```

### Hierarchical

```text
    A
   / \
  B   C
```

### Hybrid

Combination of inheritance types.

---

# 38. Public / Protected / Private Inheritance

Example:

```cpp
class B : public A
```

### Public inheritance

```text
Base public    → Derived public
Base protected → Derived protected
Base private   → inaccessible directly
```

### Protected inheritance

```text
Base public    → Derived protected
Base protected → Derived protected
```

### Private inheritance

```text
Base public    → Derived private
Base protected → Derived private
```

The base class's private members are not directly accessible in the derived class.

---

# 39. Constructor

Special function automatically called when an object is created.

```cpp
class Student {
public:
    Student() {
        cout << "Constructor";
    }
};
```

```cpp
Student s;
```

Constructor executes automatically.

### Properties

* Same name as class
* No return type
* Called automatically
* Can be overloaded

---

# 40. Default Constructor

```cpp
Student() {
}
```

Constructor with no required parameters.

---

# 41. Parameterized Constructor

```cpp
class Student {
public:
    int age;

    Student(int a) {
        age = a;
    }
};
```

```cpp
Student s(21);
```

---

# 42. Constructor Initialization List

Preferred approach:

```cpp
Student(int a) : age(a) {
}
```

Useful/required for:

* `const` members
* reference members
* base-class construction
* member objects that need constructor arguments

---

# 43. Copy Constructor

Creates an object from another object.

```cpp
class Student {
public:
    int age;

    Student(int a) {
        age = a;
    }

    Student(const Student &s) {
        age = s.age;
    }
};
```

```cpp
Student s1(21);
Student s2 = s1;
```

---

# 44. Copy Constructor vs Assignment Operator

### Copy initialization

```cpp
Student s2 = s1;
```

Can invoke copy constructor.

### Assignment

```cpp
s2 = s1;
```

Uses copy assignment operator for already-existing `s2`.

---

# 45. Destructor

Called when object is destroyed.

```cpp
class A {
public:
    ~A() {
        cout << "Destructor";
    }
};
```

Syntax:

```cpp
~ClassName()
```

Destructor:

* Has same class name preceded by `~`
* No return type
* Takes no parameters
* Cannot be overloaded

---

# 46. Constructor vs Destructor

| Constructor                    | Destructor            |
| ------------------------------ | --------------------- |
| Initializes object             | Cleans up object      |
| Called on creation             | Called on destruction |
| Can be overloaded              | Cannot be overloaded  |
| Multiple constructors possible | One destructor        |
| `ClassName()`                  | `~ClassName()`        |

---

# 47. `this` Pointer

`this` points to the current object.

```cpp
class Student {
private:
    int age;

public:
    Student(int age) {
        this->age = age;
    }
};
```

Here:

```text
this->age
```

means object's member.

---

# 48. Static Data Member

Shared by all objects.

```cpp
class Student {
public:
    static int count;
};

int Student::count = 0;
```

There is one shared copy rather than one copy per ordinary object.

---

# 49. Static Member Function

```cpp
class A {
public:
    static void show() {
        cout << "Hello";
    }
};
```

Call:

```cpp
A::show();
```

A static member function does not have a normal object-specific `this` pointer.

---

# 50. Function Overloading

Same function name, different parameter list.

```cpp
int add(int a, int b) {
    return a+b;
}

double add(double a, double b) {
    return a+b;
}
```

This is **compile-time polymorphism**.

You cannot overload functions solely by changing the return type.

Invalid:

```cpp
int add(int a);
double add(int a);
```

---

# 51. Operator Overloading

Give operators behavior for user-defined types.

```cpp
class Complex {
public:
    int real, imag;

    Complex operator+(const Complex &c) {
        Complex temp;
        temp.real = real + c.real;
        temp.imag = imag + c.imag;
        return temp;
    }
};
```

Now:

```cpp
c3 = c1 + c2;
```

---

# 52. Operators That Cannot Be Overloaded

Important examples:

```text
::
.
.*
?:
sizeof
```

---

# 53. Compile-Time Polymorphism

Achieved mainly through:

```text
Function overloading
Operator overloading
Templates
```

Example:

```cpp
void print(int x);
void print(string x);
```

Compiler decides which function to call.

---

# 54. Runtime Polymorphism

Achieved through:

```text
Inheritance + virtual functions
```

Example:

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Animal sound";
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark";
    }
};
```

```cpp
Animal* a = new Dog();

a->sound();
```

Output:

```text
Bark
```

---

# 55. Virtual Function

A virtual function enables dynamic dispatch.

```cpp
class Base {
public:
    virtual void show() {
        cout << "Base";
    }
};
```

Derived:

```cpp
class Derived : public Base {
public:
    void show() override {
        cout << "Derived";
    }
};
```

---

# 56. Why Virtual Functions?

Consider:

```cpp
Base* ptr = new Derived();
ptr->show();
```

Without `virtual`:

```text
Base
```

With `virtual`:

```text
Derived
```

The actual object's overridden function is selected at runtime.

---

# 57. `override`

Use:

```cpp
void show() override
```

It tells the compiler that you intend to override a virtual function.

This catches mistakes such as mismatched signatures.

---

# 58. Pure Virtual Function

```cpp
virtual void show() = 0;
```

Example:

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
```

This makes `Shape` an abstract class.

---

# 59. Abstract Class

A class containing at least one pure virtual function.

```cpp
class Animal {
public:
    virtual void sound() = 0;
};
```

You cannot do:

```cpp
Animal a; // Error
```

But:

```cpp
Animal* a = new Dog();
```

is possible if `Dog` implements the required function.

---

# 60. Interface in C++

C++ does not have a dedicated `interface` keyword like Java.

An interface-like abstraction is commonly represented by a class containing pure virtual functions.

```cpp
class Payment {
public:
    virtual void pay() = 0;
    virtual ~Payment() = default;
};
```

---

# 61. Virtual Destructor

Very important interview question.

Consider:

```cpp
class Base {
public:
    virtual ~Base() {}
};
```

Why?

```cpp
Base* ptr = new Derived();
delete ptr;
```

A virtual destructor ensures the derived destructor is invoked correctly through the base pointer.

**Rule of thumb:** If a class is intended to be used polymorphically, its destructor should generally be virtual.

---

# 62. Early Binding vs Late Binding

### Early binding

Decision at compile time.

Examples:

```text
Function overloading
Operator overloading
```

### Late binding

Decision at runtime.

Example:

```text
Virtual function
```

---

# 63. Diamond Problem

Multiple inheritance:

```text
       A
      / \
     B   C
      \ /
       D
```

Both B and C inherit from A.

D may get two copies of A.

---

# 64. Virtual Inheritance

Solves the diamond duplication issue.

```cpp
class B : virtual public A {};
class C : virtual public A {};

class D : public B, public C {};
```

Now D has one shared A subobject.

---

# 65. Friend Function

A friend function can access private/protected members.

```cpp
class Box {
private:
    int value;

public:
    Box(int v) : value(v) {}

    friend void show(Box b);
};

void show(Box b) {
    cout << b.value;
}
```

Friendship is not automatically inherited or reciprocal.

---

# 66. Friend Class

```cpp
class A {
    friend class B;
private:
    int x;
};
```

B can access A's private members.

---

# 67. `const` Member Function

```cpp
class Student {
public:
    int getAge() const {
        return age;
    }

private:
    int age;
};
```

A const member function promises not to modify the object's ordinary state.

---

# 68. Const Object

```cpp
const Student s;
```

A const object can call only member functions that are themselves `const` (subject to special cases such as static functions).

---

# 69. `const` Pointer Concepts

### Pointer to constant

```cpp
const int *p;
```

Cannot modify value through `p`.

### Constant pointer

```cpp
int *const p = &x;
```

Pointer itself cannot point elsewhere.

### Constant pointer to constant

```cpp
const int *const p = &x;
```

Neither can be changed through `p`.

---

# 70. Namespace

Prevents name conflicts.

```cpp
namespace A {
    int x = 10;
}

namespace B {
    int x = 20;
}
```

Access:

```cpp
A::x;
B::x;
```

---

# 71. Templates

Templates enable generic programming.

### Function template

```cpp
template <typename T>
T add(T a, T b) {
    return a + b;
}
```

Works for:

```cpp
add(2, 3);
add(2.5, 3.5);
```

---

# 72. Class Template

```cpp
template <typename T>
class Box {
private:
    T value;

public:
    Box(T v) : value(v) {}

    T getValue() {
        return value;
    }
};
```

```cpp
Box<int> b1(10);
Box<string> b2("Hello");
```

---

# 73. Templates vs Function Overloading

### Overloading

Explicitly write multiple versions.

### Templates

Write a generic version that can work with multiple compatible types.

---

# 74. Exception Handling

C++ uses:

```text
try
throw
catch
```

Example:

```cpp
try {
    if(x == 0)
        throw runtime_error("Division by zero");

    cout << a / x;
}
catch(const exception& e) {
    cout << e.what();
}
```

Flow:

```text
try
 ↓
throw
 ↓
catch
```

---

# 75. STL

**STL = Standard Template Library**

Major components:

```text
Containers
Algorithms
Iterators
Function objects
```

---

# 76. Vector

Dynamic array.

```cpp
vector<int> v;

v.push_back(10);
v.push_back(20);
```

Access:

```cpp
v[0];
v.at(0);
```

Useful:

```cpp
v.size();
v.empty();
v.pop_back();
v.clear();
```

Complexities:

| Operation     | Complexity     |
| ------------- | -------------- |
| Random access | O(1)           |
| `push_back`   | Amortized O(1) |
| Insert middle | O(n)           |
| Delete middle | O(n)           |

---

# 77. Vector vs Array

| Array                | Vector           |
| -------------------- | ---------------- |
| Fixed size           | Dynamic size     |
| Low overhead         | Dynamic capacity |
| Built-in             | STL container    |
| Size generally fixed | Can grow/shrink  |

---

# 78. Deque

Double-ended queue.

```cpp
deque<int> dq;

dq.push_front(10);
dq.push_back(20);
dq.pop_front();
dq.pop_back();
```

Efficient insertion/removal at both ends.

---

# 79. List

Doubly linked list.

```cpp
list<int> l;

l.push_back(10);
l.push_front(20);
```

Good for insertion/deletion when you already have an iterator to the position.

No random access like:

```cpp
l[2]; // invalid
```

---

# 80. Stack

LIFO.

```text
Last In First Out
```

```cpp
stack<int> st;

st.push(10);
st.push(20);

st.top();
st.pop();
```

---

# 81. Queue

FIFO.

```text
First In First Out
```

```cpp
queue<int> q;

q.push(10);
q.push(20);

q.front();
q.pop();
```

---

# 82. Priority Queue

By default, max heap.

```cpp
priority_queue<int> pq;

pq.push(10);
pq.push(30);
pq.push(20);

cout << pq.top(); // 30
```

Min heap:

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

# 83. Set

Stores unique sorted values.

```cpp
set<int> s;

s.insert(10);
s.insert(5);
s.insert(10);
```

Contains:

```text
5, 10
```

Typical search/insert/delete:

```text
O(log n)
```

---

# 84. Unordered Set

```cpp
unordered_set<int> s;
```

Hash table based.

Average:

```text
insert = O(1)
search = O(1)
delete = O(1)
```

Worst case can be O(n).

Unlike `set`, elements are not maintained in sorted order.

---

# 85. Map

Stores key-value pairs sorted by key.

```cpp
map<string, int> mp;

mp["Alice"] = 90;
mp["Bob"] = 85;
```

Typical:

```text
insert = O(log n)
search = O(log n)
delete = O(log n)
```

---

# 86. Unordered Map

```cpp
unordered_map<string, int> mp;
```

Hash table.

Average:

```text
O(1)
```

Worst case:

```text
O(n)
```

Very useful in DSA for:

* Frequency counting
* Hashing
* Two Sum
* Sliding window
* Duplicate detection

---

# 87. Map vs Unordered Map

| map                                          | unordered_map       |
| -------------------------------------------- | ------------------- |
| Balanced tree-based implementation typically | Hash-table based    |
| Sorted keys                                  | No sorted order     |
| O(log n) typical                             | O(1) average        |
| Ordered traversal                            | Unordered traversal |

---

# 88. Pair

```cpp
pair<int, string> p;

p.first = 1;
p.second = "Shreya";
```

Create:

```cpp
pair<int,int> p = {10,20};
```

---

# 89. Iterators

Used to traverse STL containers.

```cpp
vector<int> v = {1,2,3};

for(auto it = v.begin(); it != v.end(); ++it) {
    cout << *it;
}
```

Modern C++:

```cpp
for(auto x : v) {
    cout << x;
}
```

---

# 90. Algorithms

Include:

```cpp
#include <algorithm>
```

Important:

```cpp
sort()
reverse()
find()
binary_search()
max()
min()
count()
```

Example:

```cpp
sort(v.begin(), v.end());
```

---

# 91. Lambda Function

Anonymous function.

```cpp
auto add = [](int a, int b) {
    return a + b;
};
```

Call:

```cpp
cout << add(2,3);
```

Useful with STL:

```cpp
sort(v.begin(), v.end(), [](int a, int b) {
    return a > b;
});
```

Sort descending.

---

# 92. `auto`

Compiler determines type.

```cpp
auto x = 10;
```

Equivalent type:

```cpp
int
```

Example:

```cpp
auto it = v.begin();
```

Useful with complex iterator types.

---

# 93. Range-Based For Loop

```cpp
for(int x : v) {
    cout << x;
}
```

To modify:

```cpp
for(int &x : v) {
    x++;
}
```

Read-only without copying:

```cpp
for(const int &x : v) {
    cout << x;
}
```

---

# 94. Smart Pointers

Modern C++ provides:

```text
unique_ptr
shared_ptr
weak_ptr
```

Include:

```cpp
#include <memory>
```

---

# 95. `unique_ptr`

Single ownership.

```cpp
unique_ptr<int> p = make_unique<int>(10);
```

Cannot be copied:

```cpp
unique_ptr<int> p2 = p; // Error
```

Can be moved:

```cpp
unique_ptr<int> p2 = move(p);
```

---

# 96. `shared_ptr`

Shared ownership.

```cpp
shared_ptr<int> p1 = make_shared<int>(10);
shared_ptr<int> p2 = p1;
```

Both share ownership.

Reference count increases.

---

# 97. `weak_ptr`

Non-owning reference to an object managed by `shared_ptr`.

```cpp
weak_ptr<int> w = p1;
```

Useful for avoiding reference cycles.

---

# 98. RAII

**Resource Acquisition Is Initialization**

Resource lifetime is tied to object lifetime.

Resources include:

* Memory
* Files
* Locks
* Sockets

Example:

```cpp
{
    lock_guard<mutex> lock(m);
    // protected code
}
```

When object goes out of scope, destructor releases the resource.

RAII is one of the most important principles in modern C++.

---

# 99. Rule of 3

If a class manages a resource and needs one of these:

```text
Destructor
Copy constructor
Copy assignment operator
```

it often needs all three.

Example:

```cpp
~A();
A(const A&);
A& operator=(const A&);
```

---

# 100. Rule of 5

Modern C++ adds:

```text
Move constructor
Move assignment operator
```

So:

```text
Destructor
Copy constructor
Copy assignment
Move constructor
Move assignment
```

---

# 101. Move Constructor

Transfers resources rather than performing an expensive deep copy.

```cpp
class A {
public:
    A(A&& other) {
        // transfer resources
    }
};
```

Uses rvalue reference:

```cpp
&&
```

---

# 102. Lvalue vs Rvalue

### Lvalue

Has an identifiable persistent location.

```cpp
int x = 10;
```

`x` is an lvalue.

### Rvalue

Usually a temporary/value expression.

```cpp
10
x + 5
```

Rvalue references:

```cpp
int&& r = 10;
```

---

# 103. Move Semantics

Suppose an object owns a large dynamically allocated resource.

Copying:

```text
Object A
   ↓
copy entire resource
   ↓
Object B
```

Moving:

```text
Object A
   ↓
transfer ownership
   ↓
Object B
```

This can improve performance by avoiding unnecessary deep copies.

---

# 104. Shallow Copy vs Deep Copy

### Shallow copy

Copies pointer/address.

```text
A ──→ Resource
B ──→ Resource
```

Both point to same resource.

Potential issue:

```text
double delete
```

### Deep copy

Creates independent resource.

```text
A ──→ Resource 1
B ──→ Resource 2
```

---

# 105. Composition

A class contains another object.

```cpp
class Engine {};

class Car {
private:
    Engine engine;
};
```

Relationship:

```text
Car HAS-A Engine
```

---

# 106. Aggregation

A weaker HAS-A relationship.

Objects can exist independently.

Example:

```text
Department HAS-A Professor
```

A professor can exist independently of a particular department.

---

# 107. Composition vs Inheritance

### Inheritance

```text
IS-A
```

Example:

```text
Dog IS-A Animal
```

### Composition

```text
HAS-A
```

Example:

```text
Car HAS-A Engine
```

In software design, composition is often preferred when the relationship is containment rather than a genuine subtype relationship.

---

# 108. Important OOP Relationships

```text
Inheritance → IS-A

Composition → HAS-A

Aggregation → HAS-A, weaker ownership

Association → uses/knows-about relationship
```

---

# 109. Virtual Function Internals

A class with virtual functions typically has implementation machinery involving a:

```text
vtable
```

Objects of such classes commonly carry a hidden:

```text
vptr
```

The exact object layout is compiler/ABI dependent.

Conceptually:

```text
Base pointer
     ↓
actual object
     ↓
vptr
     ↓
vtable
     ↓
overridden function
```

This supports dynamic dispatch.

---

# 110. Overloading vs Overriding

Very common interview question.

### Overloading

Same name, different parameter list.

```cpp
void add(int a);
void add(int a, int b);
```

Compile-time.

### Overriding

Derived class provides a new implementation of a base virtual function.

```cpp
class Base {
public:
    virtual void show();
};

class Derived : public Base {
public:
    void show() override;
};
```

Runtime polymorphism.

---

# 111. Overloading vs Overriding vs Hiding

### Overloading

Same scope/name, different parameter lists.

### Overriding

Derived class replaces inherited virtual function implementation.

### Name hiding

Derived declaration can hide base overloads with the same name.

You can expose base overloads using:

```cpp
using Base::show;
```

---

# 112. Virtual vs Pure Virtual

### Virtual

Can have implementation.

```cpp
virtual void show() {
    cout << "Base";
}
```

### Pure virtual

```cpp
virtual void show() = 0;
```

Makes the class abstract if it is not otherwise already abstract.

---

# 113. Can Constructor Be Virtual?

**No.**

Reason:

A constructor creates the object, so runtime virtual dispatch cannot be used to invoke a constructor for an object that does not yet fully exist.

### Can destructor be virtual?

**Yes.**

And it should generally be virtual for polymorphic base classes.

---

# 114. Can Static Function Be Virtual?

**No.**

Virtual dispatch requires an object/dynamic type, while a static member function belongs to the class rather than a particular object.

---

# 115. Can Private Function Be Overridden?

A derived class cannot directly override a base private member because it is not accessible to the derived class.

However, virtual dispatch and access control are separate concepts; a private virtual function can exist in a base class and participate in virtual dispatch.

---

# 116. Multiple Inheritance

```cpp
class A {};
class B {};

class C : public A, public B {};
```

C inherits from both A and B.

Potential issue:

```text
Diamond problem
```

Virtual inheritance can address duplicated base subobjects.

---

# 117. Object Slicing

Occurs when a derived object is copied into a base object by value.

```cpp
class Base {};
class Derived : public Base {};

Derived d;

Base b = d;
```

The derived-specific portion is sliced off.

Prefer references/pointers for polymorphic behavior:

```cpp
Base& b = d;
```

or:

```cpp
Base* b = &d;
```

---

# 118. `nullptr` vs `NULL` vs `0`

Prefer:

```cpp
nullptr
```

because it is a dedicated null pointer literal.

```cpp
int *p = nullptr;
```

---

# 119. `delete` vs `delete[]`

For one object:

```cpp
delete p;
```

For an array:

```cpp
delete[] arr;
```

Matching allocation/deallocation is important.

---

# 120. `new` vs `malloc`

| new                           | malloc                                 |
| ----------------------------- | -------------------------------------- |
| C++                           | C                                      |
| Calls constructor for objects | Does not                               |
| Type-aware                    | Returns `void*`                        |
| `delete`                      | `free`                                 |
| Can be overloaded             | Cannot be overloaded in same C++ sense |

Modern C++ generally prefers containers and smart pointers over direct dynamic allocation.

---

# 121. Header Files

Example:

```cpp
#include <iostream>
#include <vector>
#include <string>
```

User-defined header:

```cpp
#include "Student.h"
```

---

# 122. Header Guards

Traditional:

```cpp
#ifndef STUDENT_H
#define STUDENT_H

class Student {
};

#endif
```

Modern common alternative:

```cpp
#pragma once
```

---

# 123. `const` vs `constexpr`

### const

Value cannot be modified after initialization.

```cpp
const int x = 10;
```

### constexpr

Intended to be usable as a compile-time constant when initialized with a constant expression.

```cpp
constexpr int x = 10;
```

---

# 124. `inline`

`inline` allows a function to be defined in multiple translation units under the language's ODR rules and historically suggests inlining.

```cpp
inline int square(int x) {
    return x*x;
}
```

Important: `inline` does **not guarantee** that the compiler physically inlines the function.

---

# 125. `static` Local Variable

```cpp
void counter() {
    static int count = 0;
    count++;

    cout << count;
}
```

Unlike a normal local variable, its lifetime lasts until program termination.

Multiple calls:

```text
1
2
3
```

---

# 126. Scope

Types of scope:

```text
Global
Local
Class
Namespace
Block
```

Example:

```cpp
int x = 10;

int main() {
    int x = 20;
}
```

Local `x` hides global `x`.

---

# 127. Scope Resolution Operator

```cpp
::
```

Used for:

```text
Namespace
Class static members
Out-of-class member definitions
Global scope
```

Example:

```cpp
class Student {
public:
    void show();
};

void Student::show() {
}
```

---

# 128. Constructor Overloading

```cpp
class Student {
public:

    Student() {}

    Student(int age) {}

    Student(string name, int age) {}
};
```

Multiple constructors with different parameter lists.

---

# 129. Default Arguments

```cpp
void greet(string name = "User") {
    cout << name;
}
```

Calling:

```cpp
greet();
```

uses `"User"`.

---

# 130. Recursion

Function calls itself.

```cpp
int factorial(int n) {
    if(n == 0)
        return 1;

    return n * factorial(n-1);
}
```

Must have a base condition.

---

# 131. Important C++ STL Complexity

| Container      |   Access |   Search |                Insert |
| -------------- | -------: | -------: | --------------------: |
| vector         |     O(1) |     O(n) | O(1) amortized at end |
| deque          |     O(1) |     O(n) |          O(1) at ends |
| list           |     O(n) |     O(n) |    O(1) with iterator |
| set            |        — | O(log n) |              O(log n) |
| unordered_set  |        — | O(1) avg |              O(1) avg |
| map            |        — | O(log n) |              O(log n) |
| unordered_map  |        — | O(1) avg |              O(1) avg |
| priority_queue | top O(1) |        — |              O(log n) |

---

# 132. Most Important Interview Differences

## `vector` vs `list`

```text
vector → contiguous memory, random access
list   → linked nodes, no random access
```

## `map` vs `unordered_map`

```text
map → ordered, O(log n)
unordered_map → unordered, O(1) average
```

## `set` vs `unordered_set`

```text
set → sorted unique values
unordered_set → unique values, no sorted order
```

## Stack vs Queue

```text
stack → LIFO
queue → FIFO
```

---

# 133. C++ OOP Four Pillars — Interview Answer

If interviewer asks:

> What are the four pillars of OOP?

Answer:

> **Encapsulation, abstraction, inheritance, and polymorphism. Encapsulation bundles data and behavior and controls access; abstraction hides implementation complexity; inheritance allows a derived class to reuse/extend a base class; and polymorphism allows the same interface to represent different implementations.**

---

# 134. Runtime Polymorphism — Interview Answer

> Runtime polymorphism in C++ is mainly achieved through inheritance and virtual functions. A base-class pointer or reference can refer to a derived object, and when a virtual function is called, the derived implementation is selected at runtime.

Example:

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Animal";
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark";
    }
};

Animal* a = new Dog();
a->sound();
delete a;
```

Output:

```text
Bark
```

---

# 135. Compile-Time vs Runtime Polymorphism

| Compile-time         | Runtime                        |
| -------------------- | ------------------------------ |
| Function overloading | Virtual functions              |
| Operator overloading | Function overriding            |
| Templates            | Inheritance + dynamic dispatch |
| Early binding        | Late binding                   |

---

# 136. Why Use `override`?

Instead of:

```cpp
void sound();
```

write:

```cpp
void sound() override;
```

This lets the compiler verify that a base virtual function is actually being overridden.

---

# 137. Why Virtual Destructor?

Interview answer:

> If a base class is used polymorphically, its destructor should generally be virtual so that deleting a derived object through a base pointer invokes the derived destructor correctly.

```cpp
Base* ptr = new Derived();
delete ptr;
```

Without a virtual destructor, this can result in incorrect destruction and undefined behavior.

---

# 138. Why Use Smart Pointers?

Raw pointers don't express ownership clearly and can lead to:

```text
Memory leaks
Dangling pointers
Double deletion
Exception-safety problems
```

Smart pointers use RAII.

```cpp
unique_ptr
shared_ptr
weak_ptr
```

---

# 139. `unique_ptr` vs `shared_ptr`

### unique_ptr

```text
One owner
Moveable
Not copyable
Low overhead
```

### shared_ptr

```text
Multiple owners
Reference counted
Copyable
More overhead
```

Use `weak_ptr` for a non-owning relationship to an object managed by `shared_ptr`, especially to break ownership cycles.

---

# 140. Important Interview Question: Why C++?

Possible technical answer:

> C++ provides object-oriented and generic programming features while also giving low-level control over memory and system resources. It supports efficient abstractions, STL, templates, RAII, and modern memory-management tools, which makes it useful for performance-sensitive software, systems, competitive programming, and many large-scale applications.

---

# 141. Most Important C++ Topics for Your Interview

For placement preparation, prioritize these:

### Tier 1 — Must Know

```text
Classes & Objects
Encapsulation
Inheritance
Polymorphism
Abstraction
Constructors
Destructors
Function Overloading
Function Overriding
Virtual Functions
Pure Virtual Functions
Abstract Classes
Pointers
References
Pass by value/reference
STL
vector
map
unordered_map
set
stack
queue
priority_queue
```

### Tier 2 — Very Important

```text
this pointer
static
const
friend
operator overloading
templates
exception handling
smart pointers
RAII
copy constructor
copy assignment
shallow vs deep copy
```

### Tier 3 — Strong Interview Knowledge

```text
Rule of 3
Rule of 5
Move semantics
lvalue/rvalue
virtual inheritance
diamond problem
object slicing
vtable/vptr
multiple inheritance
composition vs aggregation
```

---

# 142. Top 25 C++ Interview Questions

You should be able to answer these **without looking at notes**:

1. What is C++?
2. C vs C++?
3. What are the four pillars of OOP?
4. Class vs object?
5. Encapsulation vs abstraction?
6. Inheritance and its types?
7. What is polymorphism?
8. Compile-time vs runtime polymorphism?
9. Overloading vs overriding?
10. What is a virtual function?
11. What is a pure virtual function?
12. What is an abstract class?
13. Why should a polymorphic base class have a virtual destructor?
14. Can constructors be virtual?
15. What is a constructor?
16. What is a copy constructor?
17. Copy constructor vs assignment operator?
18. Shallow copy vs deep copy?
19. Pointer vs reference?
20. Stack vs heap?
21. `new/delete` vs `malloc/free`?
22. `map` vs `unordered_map`?
23. `vector` vs `list`?
24. `unique_ptr` vs `shared_ptr`?
25. What are Rule of 3 and Rule of 5?

---

# 143. Ultra-Short Revision Sheet

```text
C++
│
├── Basics
│   ├── Data types
│   ├── Operators
│   ├── Conditions
│   ├── Loops
│   └── Functions
│
├── Memory
│   ├── Stack
│   ├── Heap
│   ├── Pointer
│   ├── Reference
│   ├── new/delete
│   └── Smart pointers
│
├── OOP
│   ├── Class/Object
│   ├── Encapsulation
│   ├── Abstraction
│   ├── Inheritance
│   └── Polymorphism
│
├── Polymorphism
│   ├── Overloading
│   ├── Operator overloading
│   ├── Virtual function
│   ├── Overriding
│   └── Pure virtual function
│
├── Object lifecycle
│   ├── Constructor
│   ├── Destructor
│   ├── Copy constructor
│   ├── Copy assignment
│   ├── Move constructor
│   └── Move assignment
│
├── Generic Programming
│   └── Templates
│
└── STL
    ├── vector
    ├── list
    ├── deque
    ├── stack
    ├── queue
    ├── priority_queue
    ├── set
    ├── unordered_set
    ├── map
    ├── unordered_map
    ├── pair
    ├── iterators
    └── algorithms
```

### For your placement interview, the **highest-return C++ chain** to master is:

**Pointers & references → classes/objects → constructors/destructors → encapsulation → inheritance → overloading/overriding → virtual functions → runtime polymorphism → abstract classes → copy/deep copy → smart pointers → STL.**

That sequence will also make your **OOP interview questions much easier**, because most interviewers build questions progressively from these concepts.
