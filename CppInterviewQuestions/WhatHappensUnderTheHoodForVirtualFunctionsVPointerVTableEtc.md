# [What happens under the hood for virtual functions? (vPointer, vTable, etc)](#what-happens-under-the-hood-for-virtual-functions-vpointer-vtable-etc)

## **How Virtual Functions Work Under the Hood**

In C++, virtual functions enable runtime polymorphism, allowing the correct function to be called based on the actual type of the object, rather than the type of the pointer or reference. This is achieved using **vTables** (Virtual Tables) and **vPointers** (Virtual Pointers).

---

## **Key Concepts**

1. **Virtual Function Table (vTable):**
   - A table maintained per class, containing pointers to the virtual functions that objects of that class can call.
   - Each entry in the vTable corresponds to a virtual function defined or overridden in the class.

2. **Virtual Pointer (vPointer):**
   - A hidden pointer added to objects of classes with virtual functions.
   - It points to the vTable of the class corresponding to the object's actual type.

3. **Dynamic Dispatch:**
   - When a virtual function is called through a base class pointer or reference, the compiler uses the vPointer to locate the vTable and calls the appropriate function.

---

## **How It Works**

### **1. Class with Virtual Functions**
When a class contains virtual functions:
- The compiler generates a vTable for the class.
- The vTable contains pointers to the virtual functions defined in or overridden by the class.

### **2. Object of the Class**
When an object of the class is created:
- A hidden vPointer is added to the object.
- The vPointer is initialized to point to the vTable of the class.

### **3. Calling a Virtual Function**
When a virtual function is called:
1. The compiler generates code to access the vPointer in the object.
2. The vPointer is used to look up the vTable of the class.
3. The function pointer in the vTable is dereferenced, and the correct function is invoked.

---

## **Example: Virtual Function and vTable**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void func() { cout << "Base::func" << endl; }
    virtual ~Base() {}
};

class Derived : public Base {
public:
    void func() override { cout << "Derived::func" << endl; }
};

int main() {
    Base* basePtr;
    Derived derivedObj;
    basePtr = &derivedObj;

    basePtr->func(); // Calls Derived::func at runtime
    return 0;
}
```

---

## **What Happens Under the Hood**

1. **Class Layout and vTable:**
   - `Base` class has a vTable pointing to `Base::func`.
   - `Derived` class has a vTable pointing to `Derived::func`.

   ```
   Base vTable:    [ &Base::func ]
   Derived vTable: [ &Derived::func ]
   ```

2. **Object Layout:**
   - Both `Base` and `Derived` objects contain a vPointer.
   - The vPointer of a `Derived` object points to the `Derived` vTable.

3. **Function Call:**
   - `basePtr->func()` accesses the vPointer in `derivedObj`.
   - The vPointer points to the `Derived` vTable.
   - The function pointer in the vTable is dereferenced, invoking `Derived::func`.

---

## **Memory Layout**

Consider the following memory layout for a `Derived` object:

```
+-------------------------+
| vPointer -> Derived vTable |
+-------------------------+
| Base class members       |
+-------------------------+
| Derived class members    |
+-------------------------+
```

---

## **Advantages of Using vTables**

1. **Runtime Polymorphism:**
   - Allows functions to be overridden and resolved at runtime, enabling dynamic behavior.

2. **Efficient Dispatch:**
   - Accessing a function pointer via the vTable is efficient (essentially one level of indirection).

---

## **Overhead of Virtual Functions**

1. **Memory Overhead:**
   - Each class with virtual functions has a vTable.
   - Each object of such classes has a vPointer.

2. **Runtime Overhead:**
   - Function calls involve an additional indirection to look up the vTable and dereference the function pointer.

---

## **Example with Multiple Levels of Inheritance**

```cpp
#include <iostream>
using namespace std;

class A {
public:
    virtual void show() { cout << "A::show" << endl; }
};

class B : public A {
public:
    void show() override { cout << "B::show" << endl; }
};

class C : public B {
public:
    void show() override { cout << "C::show" << endl; }
};

int main() {
    A* ptr = new C(); // Pointer to base class, object of derived class
    ptr->show();      // Calls C::show at runtime
    delete ptr;
    return 0;
}
```

**vTable Resolution:**
- `A`'s vTable points to `A::show`.
- `B`'s vTable points to `B::show`.
- `C`'s vTable points to `C::show`.

The `ptr->show()` call resolves to `C::show` via the vTable mechanism.

---

## **Common Scenarios with vTable**

1. **Non-Virtual Functions:**
   - These are resolved at compile time (static dispatch) and do not use vTable.

2. **Pure Virtual Functions:**
   - Pure virtual functions are placeholders in the vTable that must be implemented by derived classes.

3. **Virtual Destructor:**
   - Ensures proper cleanup in polymorphic hierarchies using the vTable.

---

## **Summary of vTable and vPointer**

| Feature          | Description                                      |
|-------------------|--------------------------------------------------|
| **vTable**        | Per-class table holding pointers to virtual functions. |
| **vPointer**      | Per-object pointer to the vTable of the class.   |
| **Dynamic Dispatch** | Resolves virtual function calls at runtime.    |
| **Overhead**      | Memory for vTable and vPointer; additional indirection. |

This mechanism underpins the flexibility and power of runtime polymorphism in C++.

---
