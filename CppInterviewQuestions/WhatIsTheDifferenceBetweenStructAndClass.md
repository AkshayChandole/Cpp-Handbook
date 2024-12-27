# [What is the difference between struct and class?](#what-is-the-difference-between-struct-and-class)

## **Difference Between `struct` and `class` in C++**

In C++, both `struct` and `class` are used to define user-defined data types. While they are similar, there are some key differences between them, primarily in terms of default access control and their typical usage.

---

## **1. Default Access Specifier**
- **`struct`**: Members of a `struct` are `public` by default.
- **`class`**: Members of a `class` are `private` by default.

### **Example:**
```cpp
#include <iostream>
using namespace std;

struct MyStruct {
    int a; // public by default
};

class MyClass {
    int a; // private by default
};

int main() {
    MyStruct s;
    s.a = 10; // Accessible directly

    MyClass c;
    // c.a = 10; // Error: 'a' is private
    return 0;
}
```

---

## **2. Intended Usage**
- **`struct`**: Traditionally used for simple data structures without complex functionality, primarily to group related variables.
- **`class`**: Used for creating objects with both data (attributes) and behavior (methods).

### Example:
**Struct Example:**
```cpp
struct Point {
    int x;
    int y;
};
```

**Class Example:**
```cpp
class Rectangle {
private:
    int length;
    int breadth;

public:
    void setDimensions(int l, int b) {
        length = l;
        breadth = b;
    }
    int area() {
        return length * breadth;
    }
};
```

---

## **3. Support for Object-Oriented Features**
- Both `struct` and `class` support object-oriented features like inheritance, polymorphism, encapsulation, and abstraction.
- However, **`class`** is more commonly associated with object-oriented programming because it is more intuitive for defining complex objects.

### Example:
```cpp
struct Animal {
    virtual void speak() { cout << "Animal speaks" << endl; }
};

class Dog : public Animal {
    void speak() override { cout << "Dog barks" << endl; }
};
```

---

## **4. Inheritance**
- **`struct`**: Inheritance in a `struct` is `public` by default.
- **`class`**: Inheritance in a `class` is `private` by default.

### Example:
```cpp
struct BaseStruct {
    int x = 10;
};

class BaseClass {
    int x = 20;
};

struct DerivedStruct : BaseStruct {}; // Public inheritance
class DerivedClass : BaseClass {};   // Private inheritance
```

---

## **5. Typical Conventions**
- **`struct`**:
  - Used for plain data structures or POD (Plain Old Data) types.
  - Members are usually public, and the focus is on grouping variables.

- **`class`**:
  - Used for encapsulating data and behavior.
  - Encourages data hiding (private/protected members) and functionality through methods.

---

## **Similarities**
Despite their differences, both `struct` and `class` share many features:
1. Can have constructors, destructors, and member functions.
2. Can define static members.
3. Can use access specifiers (`public`, `private`, `protected`).
4. Can be inherited and support polymorphism.

---

## **Choosing Between `struct` and `class`**
- Use **`struct`**:
  - For simple data structures where all members are public by design.
  - When you want to mimic C-style structures in C++.

- Use **`class`**:
  - For complex objects with private/protected data and public methods.
  - When you need encapsulation and object-oriented design principles.

---

## **Example Demonstrating Differences**

```cpp
#include <iostream>
using namespace std;

struct StructExample {
    int publicMember; // Public by default
    StructExample(int val) : publicMember(val) {}
};

class ClassExample {
private:
    int privateMember; // Private by default
public:
    ClassExample(int val) : privateMember(val) {}
    int getPrivateMember() { return privateMember; }
};

int main() {
    StructExample s(10);
    cout << "Struct public member: " << s.publicMember << endl;

    ClassExample c(20);
    cout << "Class private member accessed via getter: " << c.getPrivateMember() << endl;

    return 0;
}
```

**Output:**
```
Struct public member: 10
Class private member accessed via getter: 20
```

This example highlights the key distinction between the two: access control and intended usage.

---
