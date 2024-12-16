
# [Data Types and Variables](#data-types-and-variables)
In C++, **data types** define the kind of data a variable can hold. Each variable has a type, and the type dictates what operations can be performed on that variable and how much memory it consumes. C++ is a statically-typed language, meaning the type of a variable is defined at compile time.

Variables are used to store data values in a program. Each variable has a specific data type that determines the kind of data it can hold, such as integers, floating-point numbers, characters, etc.

They are classified as -

(A) [**Primitive Data Types**](#primitive-data-types)

(B) [**User-Defined Data Types**](#user-defined-data-types)

---

## [Primitive Data Types](#primitive-data-types)  
In C++, **primitive data types** (also called **built-in types**) are the fundamental data types provided by the language. They serve as the basic building blocks for data storage and manipulation in any C++ program. These types are directly supported by the language and are essential for performing arithmetic, logical, and comparison operations. They form the foundation of more complex data structures like arrays, classes, and pointers.

The most commonly used primitive data types in C++ include:

1.  [**Integer Types**](#integer-types)
2.  [**Floating Point Types**](#floating-point-types)
3.  [**Character Types**](#character-types)
4.  [**Boolean Type**](#boolean-type)
5.  [**Void Type**](#void-type)

Each of these data types has a specific use case, size, and range. Understanding them is essential for memory management, performance optimization, and making appropriate design choices.

----------

### [**Integer Types**](#integer-types)

Integer types are used to store whole numbers, i.e., numbers without any decimal points.

#### Types:

-   **`int`**: The default integer type. It typically represents a 32-bit signed integer, but this can vary depending on the system.
-   **`short`**: A smaller integer type, usually 16-bit.
-   **`long`**: A larger integer type, typically 32-bit or 64-bit.
-   **`long long`**: An even larger integer type, typically 64-bit.
-   **`unsigned` variants**: These types store only positive numbers and zero.
    -   **`unsigned int`**
    -   **`unsigned short`**
    -   **`unsigned long`**
    -   **`unsigned long long`**

#### Example:

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;               // Integer type (32-bit on most systems)
    short b = 100;            // Short integer type (16-bit)
    long c = 100000L;         // Long integer type (typically 32-bit)
    long long d = 1000000000L; // Long long integer type (64-bit)

    unsigned int e = 50;      // Unsigned integer type (only positive values)
    unsigned long f = 200L;   // Unsigned long type

    cout << "int: " << a << endl;
    cout << "short: " << b << endl;
    cout << "long: " << c << endl;
    cout << "long long: " << d << endl;
    cout << "unsigned int: " << e << endl;
    cout << "unsigned long: " << f << endl;

    return 0;
}
```

#### Use Cases:

-   **`int`**: Used for general-purpose integer calculations, such as counting or indexing.
-   **`long`**: Used for storing larger values, such as the size of large datasets or arrays.
-   **`unsigned`**: Used when only non-negative values are expected (e.g., array indexing or memory size calculations).

----------

### [**Floating-Point Types**](#floating-point-types):

Floating-point types are used to represent numbers with decimal points, allowing for the storage of fractional values.

#### Types:

-   **`float`**: Single-precision floating-point type (typically 32 bits).
-   **`double`**: Double-precision floating-point type (typically 64 bits).
-   **`long double`**: Extended precision floating-point type (larger than `double`, size varies by compiler, typically 80, 96, or 128 bits).

#### Example:

```cpp
#include <iostream>
using namespace std;

int main() {
    float pi = 3.14f;         // Single precision floating-point (32-bit)
    double gravity = 9.81;    // Double precision floating-point (64-bit)
    long double speedOfLight = 3.0e8;  // Long double (extended precision)

    cout << "float: " << pi << endl;
    cout << "double: " << gravity << endl;
    cout << "long double: " << speedOfLight << endl;

    return 0;
}
```

#### Use Cases:

-   **`float`**: When memory is limited and precision is less critical, such as graphics calculations or simple mathematical operations.
-   **`double`**: Used when higher precision is needed, such as scientific calculations, financial applications, or physics simulations.
-   **`long double`**: Used for calculations that require extremely high precision (e.g., astronomical calculations, cryptography).

----------

### [**Character Types**](#character-types):

Character types are used to store individual characters, which are stored as integers according to the character encoding standard (typically ASCII or Unicode).

#### Types:

-   **`char`**: Represents a single character, typically 8 bits (1 byte).
-   **`wchar_t`**: Represents a wide character, typically 16 or 32 bits (used for international characters beyond ASCII).
-   **`char16_t` and `char32_t`**: Used for Unicode characters, typically 16-bit and 32-bit respectively.

#### Example:

```cpp
#include <iostream>
using namespace std;

int main() {
    char letter = 'A';         // Single character
    wchar_t wideLetter = L'Ω'; // Wide character (for international text)

    cout << "char: " << letter << endl;
    cout << "wchar_t: " << wideLetter << endl;

    return 0;
}
```

#### Use Cases:

-   **`char`**: Used for storing single characters in strings, user input processing, and text manipulation.
-   **`wchar_t`**: Used for handling non-ASCII characters in multilingual applications.

----------

### [**Boolean Type**](#boolean-type):

The **`bool`** type is used to store binary values, i.e., `true` or `false`. It's typically used for flags, conditions, and logical operations.

#### Example:

```cpp
#include <iostream>
using namespace std;

int main() {
    bool isActive = true;    // Boolean type (true or false)
    bool isValid = false;

    cout << "isActive: " << isActive << endl;
    cout << "isValid: " << isValid << endl;

    return 0;
}
```

#### Use Cases:

-   **`bool`**: Commonly used for conditions and loops (e.g., `if` statements, `while` loops), and representing flags in a program (e.g., whether a user is authenticated).

----------

### [**Void Type**](#void-type):

The **`void`** type represents the absence of a type. It is used primarily in two scenarios:

-   **Function Return Type**: Functions that do not return any value are declared with `void`.
-   **Pointers to Unknown Types**: `void*` is a generic pointer that can point to any data type.

#### Example:

```cpp
#include <iostream>
using namespace std;

void greet() {   // Function with no return value
    cout << "Hello, World!" << endl;
}

int main() {
    greet();   // Calling the void function
    return 0;
}
```

#### Use Cases:

-   **`void`**: Used in function signatures when no return value is needed (e.g., `void main()`, `void printMessage()`).
-   **`void*`**: Used in generic programming when you want to point to different data types (commonly used in libraries like `malloc()`).

----------

### Size and Range of Primitive Types:

The size and range of the primitive data types can vary depending on the system architecture (e.g., 32-bit vs 64-bit systems), but typical sizes and ranges are:

-   **`int`**: 4 bytes, range: -2,147,483,648 to 2,147,483,647 (signed)
-   **`short`**: 2 bytes, range: -32,768 to 32,767 (signed)
-   **`long`**: 4 or 8 bytes, range: varies depending on system (signed)
-   **`long long`**: 8 bytes, range: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
-   **`unsigned int`**: 4 bytes, range: 0 to 4,294,967,295
-   **`float`**: 4 bytes, range: ±3.4e−38 to ±3.4e+38 (approx. 7 digits precision)
-   **`double`**: 8 bytes, range: ±1.7e−308 to ±1.7e+308 (approx. 15 digits precision)
-   **`char`**: 1 byte, range: -128 to 127 (signed) or 0 to 255 (unsigned)
-   **`bool`**: 1 byte, range: `true` (1) or `false` (0)

----------

### Common Interview Questions Related to Primitive Data Types:

1.  **What is the difference between `int` and `long`?**
    
    -   The primary difference is the range of values that they can store. On most systems, `long` is larger than `int`, but the exact size can depend on the system's architecture.
2.  **When should I use `float` over `double`?**
    
    -   Use `float` when memory is a constraint and you don’t require high precision (e.g., graphics rendering or approximations). Use `double` for high-precision calculations, such as scientific computations.
3.  **What happens when you assign a `float` value to an `int` variable?**
    
    -   This will lead to **implicit type conversion**, where the fractional part is discarded, and only the integer portion is stored.
4.  **What is the range of a `char`?**
    
    -   A `char` is typically 1 byte in size and can represent either 256 characters (0–255 for unsigned) or 128 characters (-128 to 127 for signed).

-----

## [User-Defined Data Types](#user-defined-data-types)  

In C++, **user-defined data types** are data types that are created by the programmer to meet specific needs in the program. These types are used when the built-in primitive types do not adequately represent the data or the structure required for a particular application.

The key user-defined data types in C++ are:

1.  [**Structures (`struct`)**](#structures-struct)
2.  [**Classes (`class`)**](#classes-class)
3.  [**Unions (`union`)**](#unions-union)
4.  [**Enumerations (`enum`)**](#enumerations-enum)
5.  [**Typedefs and `using` (type aliases)**](#typedefs-and-using-type-aliases)

These types allow you to encapsulate multiple data elements into a single entity or create custom types that align more closely with the problem you're trying to solve. Understanding these types is crucial for writing modular, efficient, and maintainable code.

----------

### [**Structures (`struct`)**](#structures-struct):

A **structure** is a user-defined data type that allows the grouping of variables (called members or fields) of different data types under a single name. This is particularly useful when you need to store data that represents an entity with multiple attributes.

#### Syntax:

```cpp
struct StructureName {
    data_type member1;
    data_type member2;
    // ... other members
};
``` 

#### Example:

```cpp
#include <iostream>
using namespace std;

// Defining a structure for a point in a 2D space
struct Point {
    int x;   // x-coordinate
    int y;   // y-coordinate
};

int main() {
    // Creating a structure variable
    Point p1;
    
    // Accessing and modifying structure members
    p1.x = 10;
    p1.y = 20;

    cout << "Point p1: (" << p1.x << ", " << p1.y << ")" << endl;

    return 0;
}
```

#### Use Cases:

-   **`struct`** is used when you need to group heterogeneous data (of different types) together. For example, in graphical applications, you might use a structure to store the coordinates of a point (`Point`), the dimensions of a rectangle (`Rectangle`), or other related attributes.

----------

### [**Classes (`class`)**](#classes-class):

A **class** is a user-defined data type that is similar to a structure but with additional capabilities, such as **encapsulation**, **inheritance**, and **polymorphism**. Classes are the foundation of object-oriented programming (OOP) in C++. They allow you to model real-world entities with both data (attributes) and functions (methods) that operate on the data.

#### Syntax:

```cpp
class ClassName {
    access_specifier:
    data_type member1;
    data_type member2;
    // ... other members

    public:
    function_name() {
        // Method implementation
    }
};
```

#### Example:

```cpp
#include <iostream>
using namespace std;

class Rectangle {
    private:
        int width, height;  // Private attributes

    public:
        // Constructor to initialize the Rectangle
        Rectangle(int w, int h) : width(w), height(h) {}

        // Method to calculate the area of the rectangle
        int area() {
            return width * height;
        }

        // Method to display the dimensions
        void display() {
            cout << "Width: " << width << ", Height: " << height << endl;
        }
};

int main() {
    // Creating an object of the class Rectangle
    Rectangle rect(10, 20);

    // Accessing the class methods
    rect.display();
    cout << "Area: " << rect.area() << endl;

    return 0;
}
```

#### Use Cases:

-   **`class`** is used when you need to bundle both data and related functionality together. It's ideal for designing applications using object-oriented principles like encapsulation (hiding data) and abstraction.

----------

### [**Unions (`union`)**](#unions-union):

A **union** is a special data type that allows you to store different data types in the same memory location. Unlike a structure, where each member has its own memory location, all members of a union share the same memory space. The size of the union is the size of its largest member.

#### Syntax:

```cpp
union UnionName {
    data_type member1;
    data_type member2;
    // ... other members
};
```

#### Example:

```cpp
#include <iostream>
using namespace std;

union Data {
    int i;
    float f;
    char c;
};

int main() {
    Data data;
    
    data.i = 10;  // Assigning integer to the union
    cout << "data.i: " << data.i << endl;

    data.f = 3.14;  // Assigning float to the same memory location
    cout << "data.f: " << data.f << endl;

    data.c = 'A';  // Assigning character to the same memory location
    cout << "data.c: " << data.c << endl;

    return 0;
}
```

#### Use Cases:

-   **`union`** is used when you need to store different types of data in the same memory location, and only one member needs to be active at a time. This is useful in applications like memory-mapped hardware or when implementing certain data structures like a variant (a type that can store different data types).

----------

### [**Enumerations (`enum`)**](#enumerations-enum):

An **enumeration** is a user-defined data type that consists of a set of named integer constants. It is used to represent a collection of related constants in a more readable and maintainable way. Enums can make the code more expressive and easier to understand.

#### Syntax:

```cpp
enum EnumName {
    ENUM_VALUE_1,
    ENUM_VALUE_2,
    // ... other values
};
``` 

#### Example:

```cpp
#include <iostream>
using namespace std;

// Defining an enumeration for the days of the week
enum Day { Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };

int main() {
    Day today = Wednesday;

    if (today == Wednesday) {
        cout << "It's the middle of the week!" << endl;
    }

    return 0;
}
```

#### Use Cases:

-   **`enum`** is used to represent a set of related constants. For example, you could use an enum to represent days of the week, colors, states in a state machine, etc. Enums make the code more self-documenting and avoid magic numbers.

----------

### [**Typedefs and `using` (Type Aliases)**](typedefs-and-using-type-aliases):

**Typedef** and **`using`** are used to define new names (aliases) for existing types, making code more readable and easier to maintain. **Typedef** was the traditional way to create type aliases, but **`using`** is more modern and preferred.

#### Syntax:
```cpp
typedef existing_type new_name;
```

or


```cpp
using new_name = existing_type;
``` 

#### Example:


```cpp
#include <iostream>
using namespace std;

// Using typedef to create an alias for an int
typedef int Integer;

using Float = float;  // Using the 'using' keyword to create an alias for float

int main() {
    Integer num = 10;  // Using the alias 'Integer'
    Float pi = 3.14f;  // Using the alias 'Float'

    cout << "num: " << num << endl;
    cout << "pi: " << pi << endl;

    return 0;
}
```

#### Use Cases:

-   **Typedef/`using`** is used to create more meaningful type names for complex or long types (e.g., pointers, function pointers, template instantiations). For instance, using `typedef` or `using` makes the code more readable when dealing with pointers or function signatures like `int (*func_ptr)()`. You can alias this to something more understandable like `using FuncPtr = int(*)()`.

----------

### [Key Differences Between Structures and Classes](#key-differences-between-structures-and-classes):

 | Feature | `struct` | `class` |
 |--------------------|-------------------------------|-------------------------------|
 | **Access Control** | Default members are public. | Default members are private. |
 | **Inheritance** | Inherits public by default. | Inherits private by default. | 
 | **Encapsulation** | Lacks full encapsulation. | Fully supports encapsulation. | 
 | **Use Case** | Simple data groupings. | Complex data structures with functionality. |
