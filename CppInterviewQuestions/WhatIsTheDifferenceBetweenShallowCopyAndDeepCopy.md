# [What is the difference between shallow copy and deep copy?](#what-is-the-difference-between-shallow-copy-and-deep-copy)

## **Shallow Copy vs Deep Copy in C++**

Both shallow and deep copies are used to duplicate an object, but they differ significantly in how they handle dynamically allocated memory or references within the object.

---

## **1. Shallow Copy**
A **shallow copy** duplicates an object's attributes **as is**, without creating new memory for dynamically allocated resources. It simply copies the pointer's address, making both objects share the same resource.

### **Characteristics:**
- Only the memory address is copied for pointers.
- Changes to the dynamically allocated memory in one object affect the other.
- Faster to execute since it doesn't allocate new memory.

### **Example:**
```cpp
#include <iostream>
using namespace std;

class Shallow {
private:
    int *data;

public:
    // Constructor
    Shallow(int val) {
        data = new int(val);
    }

    // Default Copy Constructor (Shallow Copy)
    Shallow(const Shallow &obj) {
        data = obj.data; // Copies the pointer, not the value
    }

    void setData(int val) {
        *data = val;
    }

    void display() const {
        cout << "Data: " << *data << endl;
    }

    // Destructor
    ~Shallow() {
        delete data; // Deleting shared resource
    }
};

int main() {
    Shallow obj1(10);
    Shallow obj2 = obj1; // Shallow copy

    obj1.setData(20);
    obj1.display(); // Data: 20
    obj2.display(); // Data: 20 (both share the same memory)

    return 0;
}
```

### **Output:**
```
Data: 20
Data: 20
```

**Issue:** When `obj2` is destroyed, it deletes the shared resource. This leads to a dangling pointer when `obj1` tries to access or delete the same memory.

---

## **2. Deep Copy**
A **deep copy** creates a new object and duplicates all the attributes, including **allocating new memory** for dynamically allocated resources. The two objects are completely independent.

### **Characteristics:**
- Allocates new memory for dynamically allocated members.
- Changes in one object don't affect the other.
- Safer but slightly slower than shallow copy due to memory allocation.

### **Example:**
```cpp
#include <iostream>
using namespace std;

class Deep {
private:
    int *data;

public:
    // Constructor
    Deep(int val) {
        data = new int(val);
    }

    // Deep Copy Constructor
    Deep(const Deep &obj) {
        data = new int(*obj.data); // Allocates new memory and copies value
    }

    void setData(int val) {
        *data = val;
    }

    void display() const {
        cout << "Data: " << *data << endl;
    }

    // Destructor
    ~Deep() {
        delete data; // Deletes only its own resource
    }
};

int main() {
    Deep obj1(10);
    Deep obj2 = obj1; // Deep copy

    obj1.setData(20);
    obj1.display(); // Data: 20
    obj2.display(); // Data: 10 (independent objects)

    return 0;
}
```

### **Output:**
```
Data: 20
Data: 10
```

**Advantage:** The two objects are independent, so deleting one does not affect the other.

---

## **Comparison Table**

| **Aspect**               | **Shallow Copy**                                      | **Deep Copy**                                          |
|--------------------------|-------------------------------------------------------|-------------------------------------------------------|
| **Memory Allocation**    | Copies the pointer (same memory location).            | Allocates new memory for the copied resource.         |
| **Performance**          | Faster, as no new memory allocation is done.          | Slightly slower due to memory allocation.             |
| **Independence**         | Objects share resources; changes affect both objects. | Objects are independent; changes do not affect each other. |
| **Use Case**             | Suitable for objects without dynamically allocated memory. | Necessary for objects with dynamically allocated memory or resource ownership. |
| **Destructor Issues**    | Double deletion can cause undefined behavior.         | No double deletion issues, as each object manages its own resource. |

---

## **When to Use**
- **Shallow Copy:**
  - When your object doesn't involve dynamic memory allocation.
  - When you are sure that shared memory will not cause problems.

- **Deep Copy:**
  - When your object involves dynamic memory allocation or resource ownership.
  - When you want each object to manage its resources independently.

---

## **Key Points to Remember**
1. If your class involves dynamic memory or unique resource management, always implement a **deep copy constructor**.
2. Use the **Rule of Three** in C++:
   - Implement a custom **copy constructor**, **assignment operator**, and **destructor** if your class requires special resource handling.
3. Modern C++ encourages the use of **smart pointers** (e.g., `std::shared_ptr` or `std::unique_ptr`) to avoid manual memory management and copying issues.

---
