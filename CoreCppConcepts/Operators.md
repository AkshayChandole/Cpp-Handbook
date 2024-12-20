# [Operators](#operators)

Operators in C++ perform specific operations on operands. They are the backbone of expressions and logical statements.

---

## [**3.2.1 Arithmetic Operators**](#arithmetic-operators)

These operators perform basic arithmetic operations.

| Operator | Description       | Example        |
|----------|-------------------|----------------|
| `+`      | Addition          | `a + b`        |
| `-`      | Subtraction       | `a - b`        |
| `*`      | Multiplication    | `a * b`        |
| `/`      | Division          | `a / b`        |
| `%`      | Modulus (remainder)| `a % b`        |

### **Example:**

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 3;

    cout << "Addition: " << a + b << endl; // Output: 13
    cout << "Subtraction: " << a - b << endl; // Output: 7
    cout << "Multiplication: " << a * b << endl; // Output: 30
    cout << "Division: " << a / b << endl; // Output: 3
    cout << "Modulus: " << a % b << endl; // Output: 1

    return 0;
}
```

---

## [**3.2.2 Relational (Comparison) Operators**](#relational-comparison-operators)

These compare two values and return a boolean result (`true` or `false`).

| Operator | Description       | Example    |
|----------|-------------------|------------|
| `==`     | Equal to          | `a == b`   |
| `!=`     | Not equal to      | `a != b`   |
| `>`      | Greater than      | `a > b`    |
| `<`      | Less than         | `a < b`    |
| `>=`     | Greater than or equal to | `a >= b` |
| `<=`     | Less than or equal to   | `a <= b` |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 10;

    cout << "Is a equal to b? " << (a == b) << endl; // Output: 0 (false)
    cout << "Is a not equal to b? " << (a != b) << endl; // Output: 1 (true)
    cout << "Is a greater than b? " << (a > b) << endl; // Output: 0 (false)
    cout << "Is a less than b? " << (a < b) << endl; // Output: 1 (true)

    return 0;
}
```

---

## [**3.2.3 Logical Operators**](#logical-operators)

These are used to combine or invert boolean expressions.

| Operator | Description                | Example         |
|----------|----------------------------|-----------------|
| `&&`     | Logical AND                | `(a > b) && (b > c)` |
| `||`     | Logical OR                 | `(a > b) || (b > c)` |
| `!`      | Logical NOT                | `!(a > b)`      |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    bool a = true, b = false;

    cout << "Logical AND: " << (a && b) << endl; // Output: 0
    cout << "Logical OR: " << (a || b) << endl; // Output: 1
    cout << "Logical NOT: " << !a << endl; // Output: 0

    return 0;
}
```

---

## [**3.2.4 Bitwise Operators**](#bitwise-operators)

Operate at the bit level.

| Operator | Description       | Example      |
|----------|-------------------|--------------|
| `&`      | Bitwise AND       | `a & b`      |
| `|`      | Bitwise OR        | `a | b`      |
| `^`      | Bitwise XOR       | `a ^ b`      |
| `~`      | Bitwise NOT       | `~a`         |
| `<<`     | Left Shift        | `a << 1`     |
| `>>`     | Right Shift       | `a >> 1`     |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 3; // a = 0101, b = 0011 in binary

    cout << "Bitwise AND: " << (a & b) << endl; // Output: 1
    cout << "Bitwise OR: " << (a | b) << endl; // Output: 7
    cout << "Bitwise XOR: " << (a ^ b) << endl; // Output: 6
    cout << "Bitwise NOT: " << (~a) << endl; // Output: -6
    cout << "Left Shift: " << (a << 1) << endl; // Output: 10
    cout << "Right Shift: " << (a >> 1) << endl; // Output: 2

    return 0;
}
```

---

## [**3.2.5 Assignment Operators**](#assignment-operators)

These assign values to variables and can combine with arithmetic operators.

| Operator | Description       | Example   |
|----------|-------------------|-----------|
| `=`      | Assignment        | `a = b`   |
| `+=`     | Add and assign    | `a += b`  |
| `-=`     | Subtract and assign| `a -= b` |
| `*=`     | Multiply and assign| `a *= b` |
| `/=`     | Divide and assign | `a /= b`  |
| `%=`     | Modulus and assign| `a %= b`  |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5;

    a += 3;
    cout << "After += : " << a << endl; // Output: 8

    a *= 2;
    cout << "After *= : " << a << endl; // Output: 16

    return 0;
}
```

---

## [**3.2.6 Increment and Decrement Operators**](#increment-and-decrement-operators)

These increase or decrease a variable's value by 1.

| Operator | Description            | Example  |
|----------|------------------------|----------|
| `++`     | Increment by 1         | `a++` or `++a` |
| `--`     | Decrement by 1         | `a--` or `--a` |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5;

    cout << "Post-increment: " << a++ << endl; // Output: 5
    cout << "Value now: " << a << endl; // Output: 6
    cout << "Pre-increment: " << ++a << endl; // Output: 7

    return 0;
}
```

---

## [**3.2.7 Ternary Operator**](#ternary-operator)

A compact way to write an if-else condition.

| Syntax               | Description                  |
|-----------------------|------------------------------|
| `condition ? trueVal : falseVal;` | If `condition` is true, returns `trueVal`; otherwise, `falseVal`. |

### **Example:**
```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 10;

    int max = (a > b) ? a : b;
    cout << "Maximum: " << max << endl; // Output: 10

    return 0;
}
```

---

## [**3.2.8 Special Operators**](#special-operators)

1. **`sizeof`:** Returns the size of a data type or variable.
   ```cpp
   #include <iostream>
   using namespace std;

   int main() {
       cout << "Size of int: " << sizeof(int) << endl; // Output: 4 (platform-dependent)
       return 0;
   }
   ```

2. **`typeid`:** Provides type information (requires `<typeinfo>`).
   ```cpp
   #include <iostream>
   #include <typeinfo>
   using namespace std;

   int main() {
       int a = 10;
       cout << "Type: " << typeid(a).name() << endl; // Output: i (int)
       return 0;
   }
   ```

---
