# [What is difference between C and C++?](#what-is-difference-between-c-and-c)

C and C++ are both powerful programming languages widely used in software development. However, they differ significantly in terms of their paradigms, features, and applications. Below is a detailed comparison:

---

### **1. Paradigm**
- **C:** 
  - Procedural Programming Language.
  - Focuses on the procedure or steps involved in solving a problem.
  - Does not support Object-Oriented Programming (OOP).

- **C++:** 
  - Multi-Paradigm Programming Language (Procedural + Object-Oriented).
  - Supports OOP concepts like Classes, Objects, Inheritance, and Polymorphism.
  - Encourages reusable, modular, and extensible code design.

**Example:**  
C lacks classes, while C++ can define classes and objects:
```c
// C Example
#include <stdio.h>
struct Rectangle {
    int length, breadth;
};
int area(struct Rectangle r) {
    return r.length * r.breadth;
}
int main() {
    struct Rectangle r = {10, 5};
    printf("Area: %d\n", area(r));
    return 0;
}
```
```cpp
// C++ Example
#include <iostream>
using namespace std;
class Rectangle {
    int length, breadth;
public:
    Rectangle(int l, int b) : length(l), breadth(b) {}
    int area() {
        return length * breadth;
    }
};
int main() {
    Rectangle r(10, 5);
    cout << "Area: " << r.area() << endl;
    return 0;
}
```

---

### **2. Programming Approach**
- **C:** Follows a top-down approach.
- **C++:** Follows a bottom-up approach due to its object-oriented nature.

---

### **3. Memory Management**
- **C:**  
  - Manual memory management using `malloc`, `calloc`, `free`.
  - No built-in support for destructors.

- **C++:**  
  - Supports dynamic memory management with `new` and `delete`.
  - Constructors and destructors automate resource management.

**Example:**
```cpp
// Dynamic Memory Allocation in C++
#include <iostream>
using namespace std;
class Demo {
public:
    Demo() { cout << "Constructor called\n"; }
    ~Demo() { cout << "Destructor called\n"; }
};
int main() {
    Demo *obj = new Demo(); // Allocates memory
    delete obj;             // Frees memory and calls the destructor
    return 0;
}
```

---

### **4. Standard Libraries**
- **C:** Limited libraries for standard operations (e.g., `stdio.h`, `stdlib.h`).
- **C++:** Rich Standard Template Library (STL) providing containers, algorithms, and iterators.

---

### **5. Type Safety**
- **C:** Type casting is less strict and prone to errors.
- **C++:** Stricter type checking. Provides safe casts like `static_cast`, `dynamic_cast`.

---

### **6. Exception Handling**
- **C:** Does not have built-in exception handling. Errors are handled using return codes or `errno`.
- **C++:** Supports robust exception handling using `try`, `catch`, and `throw`.

**Example:**
```cpp
// Exception Handling in C++
#include <iostream>
using namespace std;
int divide(int a, int b) {
    if (b == 0) throw runtime_error("Division by zero!");
    return a / b;
}
int main() {
    try {
        cout << divide(10, 0) << endl;
    } catch (const exception &e) {
        cerr << "Error: " << e.what() << endl;
    }
    return 0;
}
```

---

### **7. Performance**
- **C:** Faster due to minimal abstractions and direct hardware access.
- **C++:** Slightly slower due to OOP features and additional abstractions but still highly performant.

---

### **8. Use Cases**
- **C:**  
  - Low-level system programming.
  - Operating systems (e.g., UNIX/Linux), embedded systems, and device drivers.
  
- **C++:**  
  - Applications requiring both high performance and complexity.
  - Game development, GUI applications, real-time systems, and competitive programming.

---

### **Key Takeaway**
C is ideal for low-level hardware interaction and system-level programming, while C++ excels in scenarios requiring complex software architectures and high-level abstractions. Both languages have their strengths and remain relevant in modern software engineering.

---
