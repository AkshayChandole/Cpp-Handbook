# [5.2 Exception Handling](#52-exception-handling)

Exception handling in C++ allows programs to handle runtime errors in a structured and controlled way. It uses the mechanisms of **try**, **catch**, and **throw** to handle exceptions, ensuring that code remains robust and can recover gracefully from unexpected scenarios.

---

## [**5.2.1 Try, Catch, and Throw**](#521-try-catch-and-throw)

The **`try`** block contains code that might throw an exception.  
The **`throw`** keyword is used to raise an exception.  
The **`catch`** block handles exceptions of a specific type. Multiple `catch` blocks can handle different types of exceptions.

**Example: Basic Syntax**
```cpp
#include <iostream>

void divide(int a, int b) {
    if (b == 0) {
        throw std::invalid_argument("Division by zero!");
    }
    std::cout << "Result: " << a / b << std::endl;
}

int main() {
    try {
        divide(10, 0);
    } catch (const std::invalid_argument& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }
    return 0;
}
```

---

## [**5.2.2 Standard Exceptions**](#522-standard-exceptions)

The C++ Standard Library provides a hierarchy of exceptions defined in the `<stdexcept>` header. These exceptions inherit from `std::exception` and include:

1. **Logic Errors**: Errors in the logic of the program.
   - `std::logic_error`
   - `std::invalid_argument`
   - `std::length_error`
   - `std::domain_error`

2. **Runtime Errors**: Errors detected during program execution.
   - `std::runtime_error`
   - `std::range_error`
   - `std::overflow_error`
   - `std::underflow_error`

**Example: Using Standard Exceptions**
```cpp
#include <iostream>
#include <stdexcept>

int main() {
    try {
        throw std::out_of_range("Index is out of range");
    } catch (const std::out_of_range& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }
    return 0;
}
```

---

## [**5.2.3 Custom Exceptions**](#523-custom-exceptions)

C++ allows developers to define their own exceptions by creating custom exception classes. These classes typically inherit from `std::exception` and override the `what()` function to provide specific error messages.

**Example: Custom Exception Class**
```cpp
#include <iostream>
#include <exception>

class MyException : public std::exception {
public:
    const char* what() const noexcept override {
        return "Custom exception occurred!";
    }
};

int main() {
    try {
        throw MyException();
    } catch (const MyException& e) {
        std::cout << e.what() << std::endl;
    }
    return 0;
}
```

---

## [**5.2.4 Exception Safety Levels**](#524-exception-safety-levels)

C++ code can be designed to adhere to different levels of exception safety, which indicate how robust a function is when exceptions occur.

1. **No-Throw Guarantee (Strongest)**:
   - The function guarantees that it will not throw an exception.
   - Often achieved using the `noexcept` specifier (explained below).

2. **Strong Exception Safety**:
   - If an exception occurs, the program’s state is unchanged (transactional behavior).
   - Useful for operations like swapping or copying.

   **Example**: Implementing strong exception safety using RAII.
   ```cpp
   #include <iostream>
   #include <vector>

   class SafeVector {
   private:
       std::vector<int> data;
   public:
       void addElement(int value) {
           try {
               data.push_back(value);
           } catch (...) {
               std::cout << "Failed to add element, rolling back changes.\n";
               throw;
           }
       }
   };
   ```

3. **Basic Exception Safety**:
   - The program remains in a valid state, but some data might be modified.

4. **No Exception Safety**:
   - If an exception occurs, the program might leave objects in an invalid state.
   - Should be avoided.

---

## [**5.2.5 `noexcept` Specifier**](#525-noexcept-specifier)

The **`noexcept`** specifier in C++ indicates whether a function will throw exceptions. Using `noexcept` can help the compiler optimize code and make it more predictable.

**Syntax**
```cpp
void myFunction() noexcept;        // Declares that myFunction will not throw.
void myFunction() noexcept(false); // Declares that myFunction might throw.
```

**When to Use**
- Use `noexcept` for functions that you are certain will not throw exceptions.
- For example, destructors, move constructors, or simple utility functions.

**Example: Using `noexcept`**
```cpp
#include <iostream>

void noExceptionFunction() noexcept {
    std::cout << "This function will not throw an exception.\n";
}

void exceptionFunction() noexcept(false) {
    throw std::runtime_error("This function might throw an exception.");
}

int main() {
    noExceptionFunction();
    try {
        exceptionFunction();
    } catch (const std::exception& e) {
        std::cout << "Caught exception: " << e.what() << std::endl;
    }
    return 0;
}
```

**Benefits of `noexcept`**
1. Improves performance by allowing better compiler optimizations.
2. Indicates intent, making the code easier to read and maintain.
3. Helps the standard library handle exceptions efficiently, e.g., in containers like `std::vector`.

---

### Summary

Exception handling in C++ is a robust system for managing runtime errors. By leveraging the `try`, `catch`, and `throw` mechanism alongside standard and custom exceptions, developers can ensure that their programs handle unexpected scenarios gracefully. Combining exception safety principles with `noexcept` specifiers allows for writing efficient and error-resistant code.

---
