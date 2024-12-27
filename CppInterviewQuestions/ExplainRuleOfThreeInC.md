# [Explain rule of three in C++.](#explain-rule-of-three-in-c)

## **Rule of Three in C++**

The **Rule of Three** in C++ is a guideline for managing resources in classes. It states that if a class explicitly defines any one of the following three special member functions, it should also explicitly define the other two:

1. **Destructor**
2. **Copy Constructor**
3. **Copy Assignment Operator**

This rule ensures proper resource management and prevents resource leaks or undefined behavior when working with dynamic memory or other resources.

---

## **The Three Functions**

1. **Destructor**  
   - Responsible for releasing resources held by the object.  
   - Automatically called when the object goes out of scope or is explicitly deleted.  
   - Example:
     ```cpp
     ~ClassName();
     ```

2. **Copy Constructor**  
   - Creates a new object as a copy of an existing object.  
   - Used when an object is passed by value or returned by value.  
   - Example:
     ```cpp
     ClassName(const ClassName& other);
     ```

3. **Copy Assignment Operator**  
   - Assigns one object to another, copying its data and resources.  
   - Example:
     ```cpp
     ClassName& operator=(const ClassName& other);
     ```

---

## **Why is the Rule of Three Necessary?**

The Rule of Three arises from the need to properly manage resources when a class handles **dynamic memory** or other non-copyable resources (e.g., file handles, network connections). 

If any of the three special functions are implemented, it typically means the class owns a resource that needs custom handling for proper copying, assignment, and destruction. Omitting one of these functions could lead to:
1. **Resource leaks:** When resources are not released properly.
2. **Double deletion:** When the same resource is deleted more than once.
3. **Undefined behavior:** When shallow copying leads to issues like dangling pointers.

---

## **Example Without Rule of Three**

```cpp
#include <iostream>
#include <cstring>

class MyString {
private:
    char* data;

public:
    MyString(const char* str) { // Constructor
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    ~MyString() { // Destructor
        delete[] data;
    }
};

int main() {
    MyString str1("Hello");
    MyString str2 = str1; // Implicit copy constructor (shallow copy)
    return 0;
}
```

**Problem:**  
The default copy constructor performs a shallow copy. Both `str1` and `str2` point to the same memory. When `str1` and `str2` are destroyed, `delete[]` is called twice on the same memory, leading to **undefined behavior**.

---

## **Correct Implementation Following Rule of Three**

```cpp
#include <iostream>
#include <cstring>

class MyString {
private:
    char* data;

public:
    // Constructor
    MyString(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    // Destructor
    ~MyString() {
        delete[] data;
    }

    // Copy Constructor
    MyString(const MyString& other) {
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
    }

    // Copy Assignment Operator
    MyString& operator=(const MyString& other) {
        if (this == &other) { // Check for self-assignment
            return *this;
        }
        delete[] data; // Release old resource
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
        return *this;
    }

    // Function to display string
    void display() const {
        std::cout << data << std::endl;
    }
};

int main() {
    MyString str1("Hello");
    MyString str2 = str1; // Uses copy constructor
    MyString str3("World");
    str3 = str1;          // Uses copy assignment operator

    str1.display();
    str2.display();
    str3.display();

    return 0;
}
```

**Output:**
```
Hello
Hello
Hello
```

**Explanation:**
1. The copy constructor ensures `str2` has its own copy of the data.
2. The copy assignment operator ensures `str3` releases its old data and takes a new copy from `str1`.

---

## **Key Points**
1. **Avoiding Shallow Copy:**  
   Implementing the Rule of Three ensures that each object has its own copy of resources, avoiding issues like double deletion or shared ownership.

2. **Self-Assignment Check:**  
   The copy assignment operator must handle the case where an object is assigned to itself.

3. **Encapsulation of Resource Management:**  
   The Rule of Three encapsulates all resource handling logic, making code more maintainable and safer.

---

## **Rule of Five (Modern C++)**

With the introduction of **C++11**, two additional special member functions were added to the Rule of Three, forming the **Rule of Five**:
1. **Move Constructor**
2. **Move Assignment Operator**

These allow for efficient transfer of resources instead of copying them, especially useful when working with temporary objects.

Example of move semantics:
```cpp
MyString(MyString&& other) noexcept {
    data = other.data; // Transfer ownership
    other.data = nullptr; // Nullify the source
}
```

Using Rule of Three ensures a solid foundation in managing resources, while adopting the Rule of Five optimizes modern C++ applications.

---
