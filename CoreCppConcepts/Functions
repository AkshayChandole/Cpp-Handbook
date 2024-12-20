# **3.4 Functions**

Functions are the building blocks of C++ programs. They encapsulate a set of instructions that perform a specific task and can be reused throughout a program. Here’s an exhaustive explanation of C++ functions, including syntax, types, use cases, and best practices.

---

## **Topics Covered**
1. Definition and Syntax
2. **Types of Functions**
   - Built-in Functions
   - User-Defined Functions
   - Lambda Functions
3. **Function Parameters**
   - Pass by Value
   - Pass by Reference
   - Default Parameters
4. **Function Overloading**
5. Inline Functions
6. Recursive Functions
7. Scope and Lifetime of Variables
8. Best Practices
9. Exceptional Cases and Edge Scenarios

---

## **1. Definition and Syntax**
A function is a block of code designed to perform a specific task. Functions improve modularity and code reuse.

**Syntax**:
```cpp
returnType functionName(parameterList) {
    // Function body
    return value; // Optional
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

// Function Definition
int add(int a, int b) {
    return a + b;
}

int main() {
    cout << "Sum: " << add(10, 20); // Function Call
    return 0;
}
// Output: Sum: 30
```

---

## **2. Types of Functions**
### **2.1 Built-in Functions**
Provided by the C++ standard library. Examples include `sqrt()`, `abs()`, and `pow()`.

**Example**:
```cpp
#include <iostream>
#include <cmath> // Required for math functions

int main() {
    double result = sqrt(25.0);
    std::cout << "Square root of 25: " << result << endl;
    return 0;
}
// Output: Square root of 25: 5
```

### **2.2 User-Defined Functions**
Functions created by the programmer to perform specific tasks.

### **2.3 Lambda Functions**
Anonymous functions that can be defined inline.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    auto square = [](int x) { return x * x; }; // Lambda function
    cout << "Square of 5: " << square(5) << endl;
    return 0;
}
// Output: Square of 5: 25
```

---

## **3. Function Parameters**
### **3.1 Pass by Value**
A copy of the argument is passed to the function.

**Example**:
```cpp
void modifyValue(int x) {
    x = 20; // Modifies local copy
}

int main() {
    int num = 10;
    modifyValue(num);
    cout << "num: " << num << endl; // Original value unchanged
    return 0;
}
// Output: num: 10
```

### **3.2 Pass by Reference**
The actual argument is passed, allowing the function to modify the original value.

**Example**:
```cpp
void modifyValue(int &x) {
    x = 20; // Modifies original variable
}

int main() {
    int num = 10;
    modifyValue(num);
    cout << "num: " << num << endl;
    return 0;
}
// Output: num: 20
```

### **3.3 Default Parameters**
Provide default values for parameters.

**Example**:
```cpp
void greet(string name = "User") {
    cout << "Hello, " << name << "!" << endl;
}

int main() {
    greet();          // Uses default value
    greet("Alice");   // Uses provided value
    return 0;
}
// Output: Hello, User!
//         Hello, Alice!
```

---

## **4. Function Overloading**
Functions with the same name but different parameter lists.

**Example**:
```cpp
#include <iostream>
using namespace std;

void display(int x) {
    cout << "Integer: " << x << endl;
}

void display(double x) {
    cout << "Double: " << x << endl;
}

int main() {
    display(10);
    display(3.14);
    return 0;
}
// Output: Integer: 10
//         Double: 3.14
```

---

## **5. Inline Functions**
Request the compiler to expand the function inline to reduce function call overhead.

**Example**:
```cpp
inline int square(int x) {
    return x * x;
}

int main() {
    cout << "Square of 4: " << square(4) << endl;
    return 0;
}
// Output: Square of 4: 16
```

**Note**: Use inline functions for small, frequently used functions. Overuse may increase binary size.

---

## **6. Recursive Functions**
Functions that call themselves.

**Example**:
```cpp
int factorial(int n) {
    if (n == 0) return 1;
    return n * factorial(n - 1);
}

int main() {
    cout << "Factorial of 5: " << factorial(5) << endl;
    return 0;
}
// Output: Factorial of 5: 120
```

**Caution**: Ensure termination conditions to avoid infinite recursion.

---

## **7. Scope and Lifetime of Variables**
- **Local Variables**: Declared inside a function; not accessible outside.
- **Global Variables**: Declared outside functions; accessible to all functions.

**Example**:
```cpp
#include <iostream>
using namespace std;

int globalVar = 10; // Global variable

void display() {
    int localVar = 20; // Local variable
    cout << "Global: " << globalVar << ", Local: " << localVar << endl;
}

int main() {
    display();
    return 0;
}
// Output: Global: 10, Local: 20
```

---

## **8. Best Practices**
1. Use meaningful names for functions.
2. Keep functions small and focused on a single task.
3. Avoid overloading too many functions with the same name—it can lead to confusion.
4. Use `const` references for parameters that should not be modified.

---

## **9. Exceptional Cases**
1. **Infinite Recursion**: Ensure a base condition for recursive functions.
2. **Uninitialized Variables**: Always initialize variables before use.
3. **Default Parameters Conflict**: Avoid ambiguous calls with default parameters.

**Example of Ambiguity**:
```cpp
void display(int x = 10, int y = 20) {
    cout << x << ", " << y << endl;
}

int main() {
    display(30); // Which parameter does 30 refer to?
    return 0;
}
```

---
