# [Explain friend class and friend function.](#explain-friend-class-and-friend-function)

## **Friend Class and Friend Function in C++**

C++ provides a mechanism to allow one class or function to access the private and protected members of another class. This is achieved using the `friend` keyword. There are two main uses of `friend`: **friend functions** and **friend classes**.

---

## **Friend Function**

A **friend function** is a function that is not a member of a class but has access to the private and protected members of the class. It is declared using the `friend` keyword inside the class whose private or protected members it needs to access.

### **Syntax**
```cpp
class ClassName {
    private:
        // Private members
    protected:
        // Protected members
    public:
        friend ReturnType FunctionName(Parameters);
};
```

### **Example**

```cpp
#include <iostream>
using namespace std;

class Box {
private:
    double width;

public:
    // Constructor
    Box(double w) : width(w) {}

    // Declare friend function
    friend void printWidth(const Box &b);
};

// Definition of friend function
void printWidth(const Box &b) {
    cout << "Width of the box: " << b.width << endl;
}

int main() {
    Box b1(5.0);
    printWidth(b1); // Accessing private member width through friend function
    return 0;
}
```

**Output**:
```
Width of the box: 5
```

Here, the `printWidth` function is not a member of the `Box` class but can access its private member `width` because it is declared as a friend of the class.

---

## **Key Points About Friend Function**
1. **Not a Member Function**:
   - A friend function is not a member of the class, so it does not need to be called using an object of the class.
   
2. **Access to Private and Protected Members**:
   - It can access private and protected members of the class.

3. **Declared Inside the Class**:
   - The function is declared inside the class with the `friend` keyword but defined outside the class.

---

## **Friend Class**

A **friend class** is a class that is declared as a friend of another class. A friend class can access the private and protected members of the class that declares it as a friend.

### **Syntax**
```cpp
class ClassName1 {
    friend class ClassName2; // ClassName2 is a friend of ClassName1
};
```

### **Example**

```cpp
#include <iostream>
using namespace std;

class Engine {
private:
    int horsepower;

public:
    Engine(int hp) : horsepower(hp) {}

    friend class Car; // Declare Car as a friend class
};

class Car {
public:
    void showEngineDetails(const Engine &e) {
        cout << "Engine horsepower: " << e.horsepower << endl;
    }
};

int main() {
    Engine e1(300);
    Car c1;
    c1.showEngineDetails(e1); // Accessing private member of Engine through Car
    return 0;
}
```

**Output**:
```
Engine horsepower: 300
```

Here, `Car` is a friend of `Engine`, so it can access the private member `horsepower` of `Engine`.

---

## **Key Points About Friend Class**
1. **One-Way Friendship**:
   - Friendship is not mutual. If class `A` declares class `B` as a friend, `B` can access `A`'s private members, but `A` cannot access `B`'s private members unless explicitly declared as a friend.

2. **Access to Private and Protected Members**:
   - A friend class can access all private and protected members of the class that declares it as a friend.

3. **Declared Inside the Class**:
   - The friend class declaration must be inside the class definition.

---

## **Friendship and Encapsulation**

Using friends can seem like a violation of encapsulation, as it breaks the strict data hiding principle. However, friendship is intended to enhance encapsulation by:
- Allowing controlled access between closely related classes or functions.
- Reducing the need to expose private details unnecessarily.

---

## **Comparison: Friend Function vs Friend Class**

| Feature             | Friend Function                          | Friend Class                             |
|---------------------|------------------------------------------|------------------------------------------|
| Scope               | Applies to a single function.            | Applies to all members of a class.       |
| Usage               | Useful for one-off access needs.         | Useful for closely related classes.      |
| Granularity         | More granular access control.            | Broad access to all private members.     |

---

## **When to Use Friends**
- When two classes or a class and a function need to work together very closely.
- To allow non-member functions to access private data while maintaining encapsulation.
- For operator overloading when the operator function needs access to private members of both operands.

---

## **Caution with Friends**
- Overuse of friends can lead to tightly coupled code, making maintenance harder.
- Use friends only when it is necessary and logical for the design.

---
