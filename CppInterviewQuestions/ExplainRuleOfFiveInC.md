# [Explain rule of five in C++.](#explain-rule-of-five-in-c)

## **Rule of Five in C++**

The **Rule of Five** in C++ is an extension of the **Rule of Three**, introduced with **C++11**, to include move semantics. It states that if a class manages resources (like dynamic memory), it should explicitly define the following five special member functions:

1. **Destructor**  
2. **Copy Constructor**  
3. **Copy Assignment Operator**  
4. **Move Constructor**  
5. **Move Assignment Operator**

These functions ensure efficient and correct resource management, especially with temporary objects and move semantics.

---

## **The Five Special Member Functions**

1. **Destructor**  
   - Responsible for cleaning up resources when the object is destroyed.
   - Automatically called when the object goes out of scope or is explicitly deleted.
   - Example:
     ```cpp
     ~ClassName();
     ```

2. **Copy Constructor**  
   - Creates a new object as a copy of an existing object.
   - Example:
     ```cpp
     ClassName(const ClassName& other);
     ```

3. **Copy Assignment Operator**  
   - Assigns one existing object to another, creating a copy of the data.
   - Example:
     ```cpp
     ClassName& operator=(const ClassName& other);
     ```

4. **Move Constructor**  
   - Transfers resources from a temporary object to a new object without copying.
   - Avoids expensive deep copies by transferring ownership of resources.
   - Example:
     ```cpp
     ClassName(ClassName&& other) noexcept;
     ```

5. **Move Assignment Operator**  
   - Transfers resources from a temporary object to an existing object without copying.
   - Example:
     ```cpp
     ClassName& operator=(ClassName&& other) noexcept;
     ```

---

## **When to Use Rule of Five**

The Rule of Five is crucial when your class manages resources such as:
- Dynamic memory (using `new` and `delete`)
- File handles
- Network sockets
- Any other non-copyable or non-trivial resources

---

## **Example Without Rule of Five**

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
        if (this == &other) return *this; // Self-assignment check
        delete[] data;
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
        return *this;
    }

    // Display Function
    void display() const {
        std::cout << data << std::endl;
    }
};

int main() {
    MyString str1("Hello");
    MyString str2 = str1; // Copy Constructor
    MyString str3("World");
    str3 = str1;          // Copy Assignment Operator

    str1.display();
    str2.display();
    str3.display();

    return 0;
}
```

This implementation works fine but lacks **move semantics**, making it inefficient for temporary objects.

---

## **Adding Move Semantics: Rule of Five**

With **move semantics**, we add a **Move Constructor** and a **Move Assignment Operator**:

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
        if (this == &other) return *this; // Self-assignment check
        delete[] data;
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
        return *this;
    }

    // Move Constructor
    MyString(MyString&& other) noexcept {
        data = other.data;      // Transfer ownership
        other.data = nullptr;   // Nullify the source
    }

    // Move Assignment Operator
    MyString& operator=(MyString&& other) noexcept {
        if (this == &other) return *this; // Self-assignment check
        delete[] data;       // Free existing resource
        data = other.data;   // Transfer ownership
        other.data = nullptr; // Nullify the source
        return *this;
    }

    // Display Function
    void display() const {
        std::cout << (data ? data : "null") << std::endl;
    }
};

int main() {
    MyString str1("Hello");
    MyString str2 = std::move(str1); // Move Constructor

    MyString str3("World");
    str3 = std::move(str2);          // Move Assignment Operator

    str1.display(); // null (str1's resource was moved)
    str2.display(); // null (str2's resource was moved)
    str3.display(); // Hello

    return 0;
}
```

---

## **Benefits of Move Semantics**
1. **Efficiency:** 
   - Avoids unnecessary deep copies by transferring ownership of resources.
   - Particularly beneficial for large objects or containers.
   
2. **Safety:** 
   - Prevents resource leaks and dangling pointers by nullifying the source object after a move.

3. **Compatibility with Modern C++:** 
   - Works seamlessly with STL containers and algorithms that utilize move semantics (e.g., `std::vector`, `std::unique_ptr`).

---

## **Key Points**
1. The **Rule of Five** ensures efficient resource management in modern C++.
2. The **Move Constructor** and **Move Assignment Operator** are specifically designed for temporary objects, avoiding deep copies.
3. Always implement the Rule of Five when your class manages resources to ensure proper behavior and compatibility with C++11 and later.

---
