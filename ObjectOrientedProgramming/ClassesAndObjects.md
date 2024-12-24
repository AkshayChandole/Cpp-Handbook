# **[4.2 Classes and Objects](#42-classes-and-objects)**

Classes and objects are the backbone of Object-Oriented Programming (OOP) in C++. A class serves as a blueprint for objects, while objects are the specific instances created based on that blueprint.

---

## **[4.2.1 Defining a Class](#421-defining-a-class)**
A class is a user-defined data type that encapsulates data members (attributes) and member functions (methods).

**Syntax**:
```cpp
class ClassName {
private:
    // Data members (attributes)
    int data;

public:
    // Member functions (methods)
    void display() {
        cout << "Data: " << data << endl;
    }
};
```

---

## **[4.2.2 Creating Objects](#422-creating-objects)**
Objects are instances of a class. You can create objects using the class name.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Person {
public:
    string name;
    int age;

    void display() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

int main() {
    Person p1;           // Create an object
    p1.name = "Alice";
    p1.age = 30;

    p1.display();        // Access member function
    return 0;
}
```

**Output**:
```
Name: Alice, Age: 30
```

---

## **[4.2.3 Access Modifiers](#423-access-modifiers)**
Access modifiers control the visibility of class members. There are three types in C++:

### **[4.2.3.1 Public](#4231-public)**
- Members are accessible from outside the class.
- Example:
    ```cpp
    class Example {
    public:
        int x;
    };

    int main() {
        Example obj;
        obj.x = 10; // Allowed
        cout << obj.x;
        return 0;
    }
    ```

### **[4.2.3.2 Private](#4232-private)**
- Members are only accessible within the class.
- Example:
    ```cpp
    class Example {
    private:
        int x;
    public:
        void setX(int val) { x = val; }
        int getX() { return x; }
    };

    int main() {
        Example obj;
        obj.setX(10);    // Allowed
        cout << obj.getX(); // Allowed
        return 0;
    }
    ```

### **[4.2.3.3 Protected](#4233-protected)**
- Members are accessible within the class and its derived classes.
- Example:
    ```cpp
    class Base {
    protected:
        int x;
    };

    class Derived : public Base {
    public:
        void setX(int val) { x = val; }
        int getX() { return x; }
    };

    int main() {
        Derived obj;
        obj.setX(20);    // Allowed
        cout << obj.getX();
        return 0;
    }
    ```

---

## **[4.2.4 Constructor and Destructor](#424-constructor-and-destructor)**

### **[4.2.4.1 Default Constructor](#4241-default-constructor)**
A default constructor is one with no arguments or all arguments having default values.

**Example**:
```cpp
class Person {
public:
    string name;
    int age;

    Person() {  // Default constructor
        name = "Unknown";
        age = 0;
    }

    void display() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};
```

---

### **[4.2.4.2 Parameterized Constructor](#4242-parameterized-constructor)**
A parameterized constructor allows passing arguments at the time of object creation.

**Example**:
```cpp
class Person {
public:
    string name;
    int age;

    Person(string n, int a) {  // Parameterized constructor
        name = n;
        age = a;
    }

    void display() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

int main() {
    Person p1("Alice", 30);   // Passing arguments
    p1.display();
    return 0;
}
```

---

### **[4.2.4.3 Copy Constructor](#4243-copy-constructor)**
A copy constructor creates a new object by copying the data from an existing object.

**Syntax**:
```cpp
ClassName(const ClassName &obj);
```

**Example**:
```cpp
class Person {
public:
    string name;
    int age;

    Person(string n, int a) {  // Parameterized constructor
        name = n;
        age = a;
    }

    Person(const Person &p) {  // Copy constructor
        name = p.name;
        age = p.age;
    }

    void display() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

int main() {
    Person p1("Alice", 30);
    Person p2 = p1;           // Copy constructor called

    p2.display();
    return 0;
}
```

---

### **[4.2.4.4 Destructor](#4244-destructor)**
A destructor is a special member function that is called automatically when an object goes out of scope. Its purpose is to release resources.

**Syntax**:
```cpp
~ClassName();
```

**Example**:
```cpp
class Person {
public:
    Person() { cout << "Constructor called" << endl; }
    ~Person() { cout << "Destructor called" << endl; }
};

int main() {
    Person p1;    // Constructor called
    return 0;     // Destructor called when object goes out of scope
}
```

**Output**:
```
Constructor called
Destructor called
```

---
