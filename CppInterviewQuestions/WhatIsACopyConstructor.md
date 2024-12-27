# [What is a copy constructor?](#what-is-a-copy-constructor)

### **Copy Constructor in C++**

A **copy constructor** is a special constructor in C++ used to create a new object as a copy of an existing object. It initializes the new object by copying the values from another object of the same class.

---

### **Syntax**
The copy constructor is defined as:
```cpp
ClassName(const ClassName &object);
```

Where:
- `const ClassName &object` is a reference to the object to be copied.
- The `const` ensures the original object is not modified during copying.

---

### **Default Copy Constructor**
If a copy constructor is not explicitly defined, the compiler provides a **default copy constructor** that performs a **shallow copy** of the object, copying all members as they are.

---

### **When is a Copy Constructor Called?**
1. **Object Initialization:**
   ```cpp
   ClassName obj1;
   ClassName obj2 = obj1; // Calls copy constructor
   ```

2. **Passing an Object by Value:**
   ```cpp
   void func(ClassName obj) { }
   func(obj1); // Calls copy constructor
   ```

3. **Returning an Object by Value:**
   ```cpp
   ClassName func() {
       ClassName obj;
       return obj; // Calls copy constructor
   }
   ```

---

### **Shallow Copy vs Deep Copy**
- **Shallow Copy**: Copies only the pointer's address, leading to shared memory.
- **Deep Copy**: Copies the actual data pointed to by the pointer, ensuring independent memory.

---

### **Example: Copy Constructor**
```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Student {
private:
    char *name;
    int age;

public:
    // Parameterized Constructor
    Student(const char *n, int a) {
        name = new char[strlen(n) + 1];
        strcpy(name, n);
        age = a;
    }

    // Copy Constructor
    Student(const Student &s) {
        name = new char[strlen(s.name) + 1];
        strcpy(name, s.name);
        age = s.age;
        cout << "Copy Constructor Called!" << endl;
    }

    // Destructor
    ~Student() {
        delete[] name;
    }

    void display() const {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

int main() {
    Student s1("John", 20);
    Student s2 = s1; // Calls Copy Constructor

    s1.display();
    s2.display();

    return 0;
}
```

**Output:**
```
Copy Constructor Called!
Name: John, Age: 20
Name: John, Age: 20
```

---

### **Default vs Custom Copy Constructor**

1. **Default Copy Constructor (Shallow Copy):**
   The compiler-provided copy constructor simply copies all members bit-by-bit, which can cause issues for dynamically allocated memory (e.g., double deletion of the same memory).

2. **Custom Copy Constructor (Deep Copy):**
   A user-defined copy constructor ensures proper duplication of dynamically allocated memory or other resources.

---

### **Example: Shallow Copy Issue**
```cpp
class Shallow {
private:
    int *data;

public:
    Shallow(int val) {
        data = new int(val);
    }

    ~Shallow() {
        delete data; // Memory deallocated
    }

    void display() const {
        cout << "Value: " << *data << endl;
    }
};

int main() {
    Shallow obj1(10);
    Shallow obj2 = obj1; // Default Copy Constructor

    obj1.display();
    obj2.display(); // Dangling pointer issue: data deleted twice

    return 0;
}
```

This code leads to **undefined behavior** because both objects share the same pointer, and the destructor tries to deallocate the same memory twice.

---

### **Advantages of a Copy Constructor**
1. Ensures correct and safe duplication of objects.
2. Prevents **shallow copy issues** by implementing a **deep copy**.
3. Facilitates object management when working with dynamically allocated resources.

---

### **Disadvantages**
1. Requires additional effort to implement when managing dynamic memory.
2. Might have a performance overhead compared to the default copy constructor.

---

### **Key Points to Remember**
1. If your class handles **dynamic memory**, always implement a custom copy constructor.
2. Pass the parameter to the copy constructor by **reference** to avoid infinite recursion.
3. For classes with simple data types, the default copy constructor is usually sufficient.

   ---
