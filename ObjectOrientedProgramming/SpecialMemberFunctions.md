# [4.5 **Special Member Functions**](#45-special-member-functions)

In C++, special member functions are functions that the compiler automatically generates for you if you don't define them yourself. These functions are typically associated with the management of an object's resources, particularly when it comes to object construction, destruction, and copying.

The following are the special member functions in C++:

1. [**Default Constructor**](#451-default-constructor)
2. [**Copy Constructor**](#452-copy-constructor)
3. [**Copy Assignment Operator**](#453-copy-assignment-operator)
4. [**Move Constructor**](#454-move-constructor)
5. [**Move Assignment Operator**](#455-move-assignment-operator)
6. [**Destructor**](#456-destructor)

These functions play an essential role in the lifecycle of an object. If you don't define them, the compiler will provide default implementations, but you can override them as needed.

## [4.5.1 **Default Constructor**](#451-default-constructor)

A **default constructor** is a constructor that takes no parameters or only default arguments. If you don't define any constructor, the compiler will automatically provide a default constructor that initializes an object with default values.

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int x;
    Example() {  // Default constructor
        x = 0;
    }
};

int main() {
    Example obj;
    cout << "x = " << obj.x << endl;  // Output: x = 0
    return 0;
}
```

**Explanation**:
- The compiler automatically calls the default constructor when an object of `Example` is created.
- The member variable `x` is initialized to `0`.

## [4.5.2 **Copy Constructor**](#452-copy-constructor)

The **copy constructor** is used to create a new object as a copy of an existing object. It is called when an object is passed by value, returned by value, or explicitly copied.

### Syntax:
```cpp
ClassName(const ClassName &obj);
```

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int x;
    Example(int val) : x(val) {}
    
    // Copy Constructor
    Example(const Example &obj) {
        x = obj.x;
        cout << "Copy Constructor called!" << endl;
    }
};

int main() {
    Example obj1(10);  // Calls the constructor
    Example obj2 = obj1;  // Calls the copy constructor
    cout << "obj2.x = " << obj2.x << endl;  // Output: obj2.x = 10
    return 0;
}
```

**Explanation**:
- The copy constructor is called when an object is copied from another object (`obj2 = obj1`).
- It initializes `obj2.x` with the value of `obj1.x`.

## [4.5.3 **Copy Assignment Operator**](#453-copy-assignment-operator)

The **copy assignment operator** is used to assign one object to another of the same type after both objects have already been constructed. It is invoked when an already constructed object is assigned the value of another object.

### Syntax:
```cpp
ClassName& operator=(const ClassName &obj);
```

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int x;
    Example(int val) : x(val) {}
    
    // Copy Assignment Operator
    Example& operator=(const Example &obj) {
        if (this != &obj) {  // Check for self-assignment
            x = obj.x;
        }
        return *this;
    }
};

int main() {
    Example obj1(10);
    Example obj2(20);
    obj2 = obj1;  // Calls the copy assignment operator
    cout << "obj2.x = " << obj2.x << endl;  // Output: obj2.x = 10
    return 0;
}
```

**Explanation**:
- The copy assignment operator is used to assign one object (`obj1`) to another (`obj2`).
- The `if (this != &obj)` check prevents self-assignment (i.e., `obj2 = obj2`).

## [4.5.4 **Move Constructor**](#454-move-constructor)

The **move constructor** is used to transfer resources from a temporary object (an rvalue) to a new object. It allows the new object to take ownership of the resources, avoiding deep copies and improving performance in certain situations.

### Syntax:
```cpp
ClassName(ClassName &&obj);
```

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int *ptr;
    
    // Constructor
    Example(int val) {
        ptr = new int(val);
    }
    
    // Move Constructor
    Example(Example &&obj) noexcept {
        ptr = obj.ptr;
        obj.ptr = nullptr;
    }

    ~Example() {
        delete ptr;
    }
};

int main() {
    Example obj1(10);
    Example obj2 = std::move(obj1);  // Calls the move constructor
    cout << *obj2.ptr << endl;  // Output: 10
    cout << (obj1.ptr == nullptr) << endl;  // Output: 1 (obj1.ptr is null after move)
    return 0;
}
```

**Explanation**:
- The move constructor transfers ownership of the dynamically allocated memory from `obj1` to `obj2`.
- After the move, `obj1.ptr` is set to `nullptr` to avoid double deletion.

## [4.5.5 **Move Assignment Operator**](#455-move-assignment-operator)

The **move assignment operator** is used to transfer ownership of resources from one object to another, and it is called when an existing object is assigned a temporary object (rvalue).

### Syntax:
```cpp
ClassName& operator=(ClassName &&obj);
```

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int *ptr;
    
    Example(int val) {
        ptr = new int(val);
    }
    
    // Move Assignment Operator
    Example& operator=(Example &&obj) noexcept {
        if (this != &obj) {
            delete ptr;  // Free existing resources
            ptr = obj.ptr;
            obj.ptr = nullptr;
        }
        return *this;
    }

    ~Example() {
        delete ptr;
    }
};

int main() {
    Example obj1(10);
    Example obj2(20);
    obj2 = std::move(obj1);  // Calls the move assignment operator
    cout << *obj2.ptr << endl;  // Output: 10
    cout << (obj1.ptr == nullptr) << endl;  // Output: 1 (obj1.ptr is null after move)
    return 0;
}
```

**Explanation**:
- The move assignment operator transfers ownership of `obj1`'s resources to `obj2`.
- After the move, `obj1.ptr` is set to `nullptr` to avoid double deletion.

## [4.5.6 **Destructor**](#456-destructor)

The **destructor** is called when an object goes out of scope or is explicitly deleted. It is used to clean up any resources that the object may have acquired during its lifetime.

### Syntax:
```cpp
~ClassName();
```

### Example:
```cpp
#include <iostream>
using namespace std;

class Example {
public:
    int *ptr;
    
    Example(int val) {
        ptr = new int(val);
    }
    
    ~Example() {  // Destructor
        delete ptr;  // Free dynamically allocated memory
    }
};

int main() {
    Example obj(10);  // Destructor called automatically at the end of the scope
    cout << *obj.ptr << endl;  // Output: 10
    return 0;
}
```

**Explanation**:
- The destructor is called when the object `obj` goes out of scope.
- It ensures that the dynamically allocated memory is properly deallocated.

## Summary:
Special member functions help manage resources during the lifetime of an object. They allow for proper initialization, copying, and destruction of objects, and can improve performance through move semantics. The key special functions are the **default constructor**, **copy constructor**, **copy assignment operator**, **move constructor**, **move assignment operator**, and **destructor**.

By carefully implementing these functions, you ensure efficient and safe handling of resources, particularly when dealing with dynamic memory.

---
