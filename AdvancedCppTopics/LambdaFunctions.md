# [5.5 Lambda Expressions](#55-lambda-expressions)

Lambda expressions, also known as anonymous functions, are a powerful feature in C++ that allow you to define and use functions inline without explicitly declaring them. They are especially useful for short, concise operations or as arguments to higher-order functions like `std::for_each` or `std::sort`.

---

## [5.5.1 Syntax of Lambda Expressions](#551-syntax-of-lambda-expressions)

The general syntax of a lambda expression is as follows:

```cpp
[ capture_list ] ( parameters ) -> return_type {
    // Function body
}
```

- **Capture List (`[]`)**: Defines which variables from the enclosing scope are accessible within the lambda.
- **Parameters (`()`)**: Specifies the input parameters, similar to a regular function.
- **Return Type (`->`)**: Optional. If omitted, the return type is deduced.
- **Body (`{}`)**: Contains the implementation of the lambda function.

---

## [5.5.2 Basic Lambda Examples](#552-basic-lambda-examples)

Here’s a simple example of a lambda expression:

```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = [](int a, int b) {
        return a + b;
    };
    cout << "Sum: " << add(3, 4) << endl;
    // Output: Sum: 7
    return 0;
}
```

---

## [5.5.3 Capture List](#553-capture-list)

The capture list allows lambdas to access variables from the enclosing scope. There are various ways to capture variables:

1. **By Value (`=`)**: Copies variables into the lambda.
2. **By Reference (`&`)**: Allows modification of variables in the enclosing scope.
3. **Mixed Capture**: Allows specific variables to be captured by value or reference.

```cpp
#include <iostream>
using namespace std;

int main() {
    int x = 10, y = 20;

    auto byValue = [=]() {
        cout << "Captured by value: " << x << ", " << y << endl;
    };

    auto byRef = [&]() {
        x += 10; // Modifies `x` in the enclosing scope
        cout << "Captured by reference: " << x << ", " << y << endl;
    };

    byValue();
    byRef();
    cout << "Value of x after byRef: " << x << endl;
    // Output:
    // Captured by value: 10, 20
    // Captured by reference: 20, 20
    // Value of x after byRef: 20

    return 0;
}
```

---

## [5.5.4 Generic Lambdas](#554-generic-lambdas)

C++14 introduced the ability to define generic lambdas using the `auto` keyword for parameters.

```cpp
#include <iostream>
using namespace std;

int main() {
    auto print = [](auto x) {
        cout << "Value: " << x << endl;
    };

    print(42);       // Output: Value: 42
    print(3.14);     // Output: Value: 3.14
    print("Hello");  // Output: Value: Hello

    return 0;
}
```

---

## [5.5.5 Lambdas with Standard Algorithms](#555-lambdas-with-standard-algorithms)

Lambdas are frequently used with STL algorithms for inline custom operations.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};

    // Double each element using for_each
    for_each(numbers.begin(), numbers.end(), [](int &n) {
        n *= 2;
    });

    cout << "Doubled numbers: ";
    for (int n : numbers) {
        cout << n << " ";
    }
    cout << endl;
    // Output: Doubled numbers: 2 4 6 8 10

    // Filter and remove odd numbers using remove_if
    numbers.erase(remove_if(numbers.begin(), numbers.end(), [](int n) {
        return n % 2 != 0;
    }), numbers.end());

    cout << "Even numbers: ";
    for (int n : numbers) {
        cout << n << " ";
    }
    cout << endl;
    // Output: Even numbers: 4 8

    return 0;
}
```

---

## [5.5.6 Lambdas with Mutable](#556-lambdas-with-mutable)

By default, lambdas capture variables as `const`. To modify a captured variable, use the `mutable` keyword.

```cpp
#include <iostream>
using namespace std;

int main() {
    int count = 0;

    auto increment = [count]() mutable {
        count++;
        return count;
    };

    cout << "First call: " << increment() << endl;
    // Output: First call: 1
    cout << "Second call: " << increment() << endl;
    // Output: Second call: 2

    cout << "Original count: " << count << endl;
    // Output: Original count: 0

    return 0;
}
```

---

## [5.5.7 Lambdas and Function Objects](#557-lambdas-and-function-objects)

Lambdas are syntactic sugar for defining function objects (functors). They enable concise and inline operations without explicitly defining a separate class or function.

```cpp
#include <iostream>
#include <functional>
using namespace std;

int main() {
    function<int(int, int)> multiply = [](int a, int b) {
        return a * b;
    };

    cout << "Product: " << multiply(3, 4) << endl;
    // Output: Product: 12

    return 0;
}
```

--- 

### Summary

Lambda expressions in C++ provide a powerful way to define inline functions, enhancing code readability and flexibility. They integrate seamlessly with modern C++ features, making them an essential tool for concise and expressive programming.

---
