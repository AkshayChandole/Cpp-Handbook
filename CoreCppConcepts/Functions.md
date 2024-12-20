# [**3.4 Functions in C++**](#34-functions-in-c)

In C++, functions are a fundamental building block that allows you to group a set of statements under a name, which can then be executed by calling the function. Functions enable code reuse, modularity, and can help break down complex problems into manageable pieces. Functions can be built-in (part of the C++ standard library) or user-defined (created by the programmer).

Let's dive deep into all aspects of functions in C++.

---

## [**3.4.1 Function Declaration and Definition**](#341-function-declaration-and-definition)

### **Function Declaration**  
A function declaration (or prototype) tells the compiler about the function's name, return type, and parameters, without providing the actual body of the function. It’s used to declare the existence of the function so it can be used before its definition.

**Syntax:**

```cpp
return_type function_name(parameter1_type parameter1, parameter2_type parameter2, ...);
```

**Example:**

```cpp
int add(int a, int b);  // Function declaration
```

Here, `int` is the return type, `add` is the function name, and it takes two integer parameters, `a` and `b`.

### **Function Definition**  
A function definition provides the actual body of the function, where the implementation resides. It specifies what the function does when it’s called.

**Syntax:**

```cpp
return_type function_name(parameter1_type parameter1, parameter2_type parameter2, ...) {
    // function body
    // statements
    return value;  // if return type is not void
}
```

**Example:**

```cpp
int add(int a, int b) {  // Function definition
    return a + b;
}
```

In this case, the function `add` takes two integers, adds them, and returns the sum.

---

## [**3.4.2 Types of Functions**](#342-types-of-functions)

### [**3.4.2.1 Built-in Functions**](#3421-built-in-functions)

Built-in functions are part of the C++ Standard Library and do not require you to define them. These functions are used for a variety of standard tasks such as input/output operations, mathematical calculations, etc.

**Examples:**

- `std::cout` (output stream)
- `std::cin` (input stream)
- `std::sqrt` (square root)
- `std::strlen` (string length)

```cpp
#include <iostream>
#include <cmath>

int main() {
    double result = std::sqrt(25.0);
    std::cout << "The square root of 25 is: " << result << std::endl;
    return 0;
}
```

Output:
```
The square root of 25 is: 5
```

### [**3.4.2.2 User-Defined Functions**](#3422-user-defined-functions)

These are functions created by the programmer to perform specific tasks. They are defined using the `return_type` and `function_name`.

**Example:**

```cpp
#include <iostream>

void greet() {
    std::cout << "Hello, world!" << std::endl;
}

int main() {
    greet();  // Calling user-defined function
    return 0;
}
```

Output:
```
Hello, world!
```

### [**3.4.2.3 Lambda Functions**](#3423-lambda-functions)

Lambda functions are anonymous functions defined at the point of use, and they can capture variables from the surrounding scope.

**Syntax:**

```cpp
[ capture_clause ] ( parameter_list ) -> return_type { function_body }
```

**Example:**

```cpp
#include <iostream>

int main() {
    auto multiply = [](int a, int b) { return a * b; };
    std::cout << "Product: " << multiply(5, 4) << std::endl;
    return 0;
}
```

Output:
```
Product: 20
```

---

## [**3.4.3 Parameter Passing**](#343-parameter-passing)

### [**3.4.3.1 Pass by Value**](#3431-pass-by-value)

In **pass by value**, the function receives a copy of the argument. Changes made to the parameter inside the function do not affect the actual argument.

**Example:**

```cpp
#include <iostream>

void increment(int x) {
    x = x + 1;
}

int main() {
    int num = 5;
    increment(num);
    std::cout << "Value of num: " << num << std::endl;  // Output will be 5
    return 0;
}
```

Output:
```
Value of num: 5
```

### [**3.4.3.2 Pass by Reference**](#3432-pass-by-reference)

In **pass by reference**, the function receives the actual reference to the argument. Changes made to the parameter will affect the original argument.

**Example:**

```cpp
#include <iostream>

void increment(int& x) {
    x = x + 1;
}

int main() {
    int num = 5;
    increment(num);
    std::cout << "Value of num: " << num << std::endl;  // Output will be 6
    return 0;
}
```

Output:
```
Value of num: 6
```

### [**3.4.3.3 Default Parameters**](#3433-default-parameters)

In C++, functions can have default parameters that are used if no argument is provided by the caller.

**Example:**

```cpp
#include <iostream>

void greet(std::string name = "User") {
    std::cout << "Hello, " << name << "!" << std::endl;
}

int main() {
    greet();  // Calls with default parameter
    greet("John");  // Calls with provided parameter
    return 0;
}
```

Output:
```
Hello, User!
Hello, John!
```

---

## [**3.4.4 Function Overloading**](#344-function-overloading)

C++ supports **function overloading**, which allows multiple functions with the same name but different parameters. The correct function is chosen based on the number and type of arguments passed.

**Example:**

```cpp
#include <iostream>

int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int main() {
    std::cout << add(5, 10) << std::endl;  // Calls int version
    std::cout << add(5.5, 10.5) << std::endl;  // Calls double version
    return 0;
}
```

Output:
```
15
16
```

---

## [**3.4.5 Inline Functions**](#345-inline-functions)

An **inline function** is a function whose definition is directly inserted into the calling code. This can reduce function call overhead for small, frequently used functions.

**Syntax:**

```cpp
inline return_type function_name(parameter_list) {
    // function body
}
```

**Example:**

```cpp
#include <iostream>

inline int square(int x) {
    return x * x;
}

int main() {
    std::cout << square(5) << std::endl;
    return 0;
}
```

Output:
```
25
```

---

## [**3.4.6 Recursive Functions**](#346-recursive-functions)

A **recursive function** is a function that calls itself to solve a problem. Recursive functions are often used for problems that can be broken down into smaller subproblems, like calculating factorials or traversing data structures like trees.

**Example (Factorial):**

```cpp
#include <iostream>

int factorial(int n) {
    if (n == 0) {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}

int main() {
    std::cout << factorial(5) << std::endl;  // Output will be 120
    return 0;
}
```

Output:
```
120
```

---

## [**3.4.7 Scope and Lifetime of Variables**](#347-scope-and-lifetime-of-variables)

### [**3.4.7.1 Local Variables**](#3471-local-variables)

Local variables are declared inside a function and can only be accessed within that function. Their lifetime is limited to the duration of the function call.

**Example:**

```cpp
#include <iostream>

void function() {
    int x = 10;  // Local variable
    std::cout << "Inside function: " << x << std::endl;
}

int main() {
    function();
    // std::cout << x;  // Error! x is not accessible outside the function
    return 0;
}
```

### [**3.4.7.2 Global Variables**](#3472-global-variables)

Global variables are declared outside all functions and can be accessed anywhere within the program. Their lifetime is the entire execution time of the program.

**Example:**

```cpp
#include <iostream>

int globalVar = 10;  // Global variable

void function() {
    std::cout << "Global variable: " << globalVar << std::endl;
}

int main() {
    function();
    return 0;
}
```

---

## [**3.4.8 Best Practices**](#348-best-practices)

- **Do's:**
  - Always prefer passing by reference for large objects (e.g., arrays, objects) to avoid unnecessary copying.
  - Use `const` with parameters that shouldn't be modified to avoid accidental changes.
  - Give meaningful names to functions and parameters.
  
- **Don'ts:**
  - Avoid using global variables inside functions unnecessarily.
  - Don’t make functions too large. Break them into smaller functions for better readability and maintainability.

---

## [**3.4.9 Exceptional Cases and Edge Scenarios**](#349-exceptional-cases-and-edge-scenarios)

- **Exception Handling:**  
  When a function could potentially throw an exception (e.g., division by zero), use try-catch blocks or return error codes.
  
- **Stack Overflow in Recursion:**  
  Recursive functions should have a base case to prevent infinite recursion, which could lead to a stack overflow error.
  
---
