# [What is operator overloading?](#what-is-operator-overloading)

## **Operator Overloading in C++**

Operator overloading in C++ allows developers to redefine or "overload" the functionality of operators (e.g., `+`, `-`, `*`, `==`) for user-defined types. It provides a way to make objects of a class behave like built-in types when interacting with operators.

---

## **Why Use Operator Overloading?**
1. **Intuitive Code**: Enables natural syntax for custom types.
2. **Improves Readability**: Makes code easier to read and maintain.
3. **Custom Behavior**: Implements domain-specific logic for operators.

---

## **Syntax of Operator Overloading**
```cpp
class ClassName {
public:
    ReturnType operator OperatorSymbol (ArgumentList) {
        // Custom logic for the operator
    }
};
```

## **Rules for Operator Overloading**
1. You **cannot create new operators**; you can only overload existing ones.
2. Certain operators like `::`, `.*`, `sizeof`, and `?:` **cannot be overloaded**.
3. At least one operand must be a user-defined type (class or struct).
4. Overloaded operators **do not inherit default precedence or associativity**.

---

## **Examples of Operator Overloading**

### **1. Overloading the `+` Operator**

Consider a class `Complex` that represents complex numbers. Overloading the `+` operator allows adding two `Complex` objects intuitively.

```cpp
#include <iostream>
using namespace std;

class Complex {
private:
    double real, imag;

public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}

    // Overloading the + operator
    Complex operator+(const Complex &c) {
        return Complex(real + c.real, imag + c.imag);
    }

    void display() {
        cout << real << " + " << imag << "i" << endl;
    }
};

int main() {
    Complex c1(3, 4), c2(1, 2);
    Complex c3 = c1 + c2; // Using the overloaded + operator
    c3.display();         // Output: 4 + 6i
    return 0;
}
```

---

### **2. Overloading the `==` Operator**

This example demonstrates comparing two objects of a class `Point` using the `==` operator.

```cpp
#include <iostream>
using namespace std;

class Point {
private:
    int x, y;

public:
    Point(int x, int y) : x(x), y(y) {}

    // Overloading the == operator
    bool operator==(const Point &p) {
        return (x == p.x && y == p.y);
    }
};

int main() {
    Point p1(2, 3), p2(2, 3), p3(4, 5);
    cout << (p1 == p2) << endl; // Output: 1 (true)
    cout << (p1 == p3) << endl; // Output: 0 (false)
    return 0;
}
```

---

### **3. Overloading the `[]` Operator**

This is particularly useful for implementing custom container classes.

```cpp
#include <iostream>
using namespace std;

class Array {
private:
    int arr[10];

public:
    Array() {
        for (int i = 0; i < 10; ++i) arr[i] = i + 1;
    }

    // Overloading the [] operator
    int &operator[](int index) {
        if (index < 0 || index >= 10) {
            cout << "Index out of bounds" << endl;
            exit(1);
        }
        return arr[index];
    }
};

int main() {
    Array myArray;
    myArray[2] = 100;               // Assign value using []
    cout << myArray[2] << endl;     // Access value using []
    return 0;
}
```

---

## **Friend Function for Operator Overloading**

Sometimes, operator overloading requires access to private members of a class. For such cases, we can use friend functions.

### **Example: Overloading `<<` (Insertion Operator)**
```cpp
#include <iostream>
using namespace std;

class Complex {
private:
    double real, imag;

public:
    Complex(double r, double i) : real(r), imag(i) {}

    // Friend function for overloading <<
    friend ostream &operator<<(ostream &out, const Complex &c) {
        out << c.real << " + " << c.imag << "i";
        return out;
    }
};

int main() {
    Complex c(3, 4);
    cout << c << endl; // Output: 3 + 4i
    return 0;
}
```

---

## **Commonly Overloaded Operators**
1. **Arithmetic Operators**: `+`, `-`, `*`, `/`, `%`
2. **Comparison Operators**: `==`, `!=`, `<`, `>`, `<=`, `>=`
3. **Assignment Operators**: `=`, `+=`, `-=`, `*=`, `/=`
4. **Bitwise Operators**: `&`, `|`, `^`, `<<`, `>>`
5. **Increment/Decrement**: `++`, `--`
6. **Special Operators**: `[]`, `()`, `->`, `<<`, `>>`

---

## **Advantages of Operator Overloading**
1. Improves code readability and usability.
2. Allows natural interaction with custom types.
3. Provides intuitive ways to perform operations on objects.

---

## **Key Considerations**
1. Avoid overloading operators unnecessarily; use it only when it makes code clearer.
2. Always consider performance implications, as overloaded operators may introduce complexity.
3. Ensure operator behavior is intuitive and aligns with user expectations.

With operator overloading, C++ allows you to seamlessly extend the functionality of operators for your custom types, making the language both powerful and flexible.
