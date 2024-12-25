# [5.1 Templates](#51-templates)

Templates in C++ provide a mechanism for writing generic and reusable code. They allow you to define functions, classes, and algorithms that work with any data type, thus promoting code reusability and flexibility.

---

## [5.1.1 Function Templates](#511-function-templates)

Function templates enable the creation of functions that can operate on any data type.

**Example:**
```cpp
#include <iostream>
using namespace std;

template <typename T>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    cout << "Integers: " << multiply(2, 3) << endl;       // Outputs 6
    cout << "Floats: " << multiply(2.5, 4.5) << endl;    // Outputs 11.25
    return 0;
}
```

**Key Points:**
- The `template <typename T>` statement declares a template where `T` is a placeholder for the data type.
- The compiler generates specific versions of the function for each type used.

---

## [5.1.2 Class Templates](#512-class-templates)

Class templates allow the creation of generic classes that can handle any type of data.

**Example:**
```cpp
#include <iostream>
using namespace std;

template <typename T>
class Stack {
    T data[100];
    int top;
public:
    Stack() : top(-1) {}
    void push(T value) { data[++top] = value; }
    T pop() { return data[top--]; }
    bool isEmpty() { return top == -1; }
};

int main() {
    Stack<int> intStack;
    Stack<string> stringStack;

    intStack.push(10);
    intStack.push(20);
    cout << "Popped from intStack: " << intStack.pop() << endl;

    stringStack.push("Hello");
    stringStack.push("Templates");
    cout << "Popped from stringStack: " << stringStack.pop() << endl;

    return 0;
}
```

**Key Points:**
- `Stack<int>` creates an integer stack, while `Stack<string>` creates a stack for strings.
- Class templates are particularly useful for container classes, like those in the Standard Template Library (STL).

---

## [5.1.3 Variadic Templates](#513-variadic-templates)

Variadic templates allow templates to take a variable number of arguments.

**Example:**
```cpp
#include <iostream>
using namespace std;

// Base case: single value
template <typename T>
T sum(T value) {
    return value;
}

// Recursive case: multiple values
template <typename T, typename... Args>
T sum(T first, Args... rest) {
    return first + sum(rest...); // Add the first argument and recursively call sum for the rest
}

int main() {
    cout << "Sum of 1, 2, 3: " << sum(1, 2, 3) << endl;               // Outputs 6
    cout << "Sum of 10, 20, 30, 40: " << sum(10, 20, 30, 40) << endl; // Outputs 100
    cout << "Sum of 5.5, 3.3, 1.2: " << sum(5.5, 3.3, 1.2) << endl;   // Outputs 10
    return 0;
}

```

**Key Points:**
- `typename... Args` represents a parameter pack that can hold multiple types.

---

## [5.1.4 Template Specialization](#514-template-specialization)

Template specialization allows tailoring a template for a specific data type.

**Example:**
```cpp
#include <iostream>
using namespace std;

template <typename T>
class Printer {
public:
    void print(T value) {
        cout << "Generic: " << value << endl;
    }
};

template <>
class Printer<string> {
public:
    void print(string value) {
        cout << "String: " << value << endl;
    }
};

int main() {
    Printer<int> intPrinter;
    Printer<string> stringPrinter;

    intPrinter.print(42);           // Outputs: Generic: 42
    stringPrinter.print("Hello!");  // Outputs: String: Hello!

    return 0;
}
```

**Key Points:**
- The generic template works for all types.
- The specialized version provides a custom implementation for a specific type (e.g., `string`).

---

## [5.1.5 Template Metaprogramming](#515-template-metaprogramming)

Template metaprogramming (TMP) uses templates to perform computations at compile time. It enables writing highly optimized and type-safe code.

**Example:**
```cpp
#include <iostream>
using namespace std;

template <int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value;
};

template <>
struct Factorial<0> {
    static const int value = 1; // Base case
};

int main() {
    cout << "Factorial of 5: " << Factorial<5>::value << endl; // Outputs 120
    return 0;
}
```

**Key Points:**
- TMP leverages recursive templates for compile-time computation.
- The `Factorial` template calculates factorials at compile time, reducing runtime overhead.

---

Templates are integral to writing generic and efficient C++ programs. They provide flexibility while maintaining type safety, and their advanced features like specialization, variadic templates, and metaprogramming extend their capabilities significantly.
