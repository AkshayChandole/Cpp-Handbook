# [**3.5 Arrays and Strings in C++**](#35-arrays-and-strings-in-c)

Arrays and strings are fundamental data structures in C++ that store collections of elements. Arrays are used to store fixed-size sequential data of the same type, while strings (as part of the Standard Template Library or STL) handle sequences of characters efficiently.

---

## [**3.5.1 Arrays**](#351-arrays)

An array is a collection of variables of the same type stored at contiguous memory locations.

### [**3.5.1.1 Declaring and Initializing Arrays**](#3511-declaring-and-initializing-arrays)
You can declare and initialize arrays in C++ using the following syntax:

```cpp
#include <iostream>

int main() {
    // Declaration
    int arr1[5];

    // Initialization
    int arr2[5] = {1, 2, 3, 4, 5};

    // Partial initialization
    int arr3[5] = {1, 2};

    // Implicit size determination
    int arr4[] = {10, 20, 30};

    // Printing array elements
    for (int i = 0; i < 5; i++) {
        std::cout << arr2[i] << " ";
    }
    // Output: 1 2 3 4 5

    return 0;
}
```

**Key Points:**
- Uninitialized arrays may contain garbage values.
- If fewer values are provided during initialization, the remaining elements are set to `0`.

---

### [**3.5.1.2 Multidimensional Arrays**](#3512-multidimensional-arrays)
C++ supports arrays with multiple dimensions, such as 2D and 3D arrays.

**Example:**

```cpp
#include <iostream>

int main() {
    // 2D Array Declaration
    int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};

    // Accessing elements
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            std::cout << matrix[i][j] << " ";
        }
        std::cout << std::endl;
    }
    // Output:
    // 1 2 3
    // 4 5 6

    return 0;
}
```

---

### [**3.5.1.3 Passing Arrays to Functions**](#3513-passing-arrays-to-functions)
You can pass arrays to functions using pointers or directly by reference.

**Example:**

```cpp
#include <iostream>

void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int size = sizeof(arr) / sizeof(arr[0]);

    printArray(arr, size); // Output: 10 20 30 40 50
    return 0;
}
```

---

### [**3.5.1.4 Limitations of Arrays**](#3514-limitations-of-arrays)
1. Fixed size: The size of the array must be known at compile time.
2. No bounds checking: Accessing elements out of bounds leads to undefined behavior.
3. Static memory allocation: Arrays are allocated on the stack, limiting their size.

---

## [**3.5.2 Strings**](#352-strings)

C++ provides robust string handling through the STL class `std::string`, which overcomes the limitations of character arrays.

---

### [**3.5.2.1 Character Arrays vs. `std::string`**](#3521-character-arrays-vs-std-string)
1. **Character Array**: Fixed-size, manually managed null-terminated strings.
2. **`std::string`**: Dynamically-sized, safer, and feature-rich.

**Example:**

```cpp
#include <iostream>
#include <string>

int main() {
    // Character Array
    char charArray[] = "Hello";

    // std::string
    std::string str = "Hello, World!";

    std::cout << charArray << std::endl; // Output: Hello
    std::cout << str << std::endl;       // Output: Hello, World!

    return 0;
}
```

---

### [**3.5.2.2 Common Operations on `std::string`**](#3522-common-operations-on-std-string)
The `std::string` class provides a variety of useful methods for string manipulation.

**Example:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello";

    // Append
    str += ", World!";
    std::cout << "Appended: " << str << std::endl; // Output: Hello, World!

    // Length
    std::cout << "Length: " << str.length() << std::endl; // Output: 13

    // Substring
    std::string sub = str.substr(7, 5);
    std::cout << "Substring: " << sub << std::endl; // Output: World

    // Find
    size_t pos = str.find("World");
    std::cout << "Position: " << pos << std::endl; // Output: 7

    // Replace
    str.replace(7, 5, "C++");
    std::cout << "Replaced: " << str << std::endl; // Output: Hello, C++!

    return 0;
}
```

---

### [**3.5.2.3 Iterating Through Strings**](#3523-iterating-through-strings)

You can iterate through `std::string` using a `for` loop or an iterator.

**Example:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "C++";

    for (char ch : str) {
        std::cout << ch << " ";
    }
    // Output: C + +

    return 0;
}
```

---

### [**3.5.2.4 Strings as Input**](#3524-strings-as-input)
Handling strings with `std::cin` and `std::getline`.

**Example:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;

    std::cout << "Enter your name: ";
    std::getline(std::cin, name); // Reads the entire line
    std::cout << "Hello, " << name << "!" << std::endl;

    return 0;
}
```

---

### [**3.5.2.5 Comparing Strings**](#3525-comparing-strings)

String comparison using relational operators (`==`, `<`, `>`, etc.).

**Example:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str1 = "Apple";
    std::string str2 = "Banana";

    if (str1 < str2) {
        std::cout << str1 << " comes before " << str2 << std::endl;
    } else {
        std::cout << str1 << " does not come before " << str2 << std::endl;
    }
    // Output: Apple comes before Banana

    return 0;
}
```

---

## [**Best Practices**](best-practices)

### **What to Do:**
1. Use `std::string` over character arrays for safer and more flexible string manipulation.
2. Always check bounds when accessing arrays.
3. Prefer STL containers like `std::vector` over raw arrays for dynamic storage.

### **What Not to Do:**
1. Avoid using uninitialized arrays or strings.
2. Do not rely on null-termination for string manipulation in character arrays.
3. Avoid accessing array elements beyond their bounds, as it leads to undefined behavior.

---

## [**Summary**](#summary)
- Arrays are static, fixed-size structures, while `std::string` is dynamic and flexible.
- C++ provides robust string handling with the `std::string` class and supports rich methods for manipulation and comparison.
- Understanding arrays and strings is crucial for solving many problems efficiently in C++.

---

