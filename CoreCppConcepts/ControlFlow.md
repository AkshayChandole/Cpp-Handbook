# [**3.3 Control Flow**](#33-control-flow)

Control flow determines the execution order of statements or blocks in a program. It is one of the most fundamental concepts in programming, allowing conditional execution, loops, and decisions based on logic.

Control flow in C++ can be categorized into the following:
1. **Conditional Statements**: Decide which block of code to execute.
2. **Looping Statements**: Repeat a block of code multiple times.
3. **Jump Statements**: Alter the flow of control explicitly.

---

## [**3.3.1 Conditional Statements**](#331-conditional-statements)

### [**3.3.1.1 if Statement**](#3311-if-statement)
The `if` statement executes a block of code if the condition evaluates to `true`.

**Syntax**:
```cpp
if (condition) {
    // Code to execute if condition is true
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;

    if (number > 5) {
        cout << "Number is greater than 5" << endl; // Output: Number is greater than 5
    }

    return 0;
}
```

---

### [**3.3.1.2 if-else Statement**](#3312-if-else-statement)
The `if-else` statement executes one block of code if the condition is `true` and another if it is `false`.

**Syntax**:
```cpp
if (condition) {
    // Code if condition is true
} else {
    // Code if condition is false
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 3;

    if (number % 2 == 0) {
        cout << "Number is even" << endl;
    } else {
        cout << "Number is odd" << endl; // Output: Number is odd
    }

    return 0;
}
```

---

### [**3.3.1.3 Nested if-else**](#3313-nested-if-else)
`if-else` blocks can be nested to check multiple conditions.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 15;

    if (number > 0) {
        if (number % 5 == 0) {
            cout << "Number is positive and divisible by 5" << endl; // Output: Number is positive and divisible by 5
        } else {
            cout << "Number is positive but not divisible by 5" << endl;
        }
    } else {
        cout << "Number is not positive" << endl;
    }

    return 0;
}
```

---

### [**3.3.1.4 else-if Ladder**](#3314-else-if-ladder)
The `else-if` ladder is used to test multiple conditions.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int marks = 85;

    if (marks >= 90) {
        cout << "Grade: A" << endl;
    } else if (marks >= 75) {
        cout << "Grade: B" << endl; // Output: Grade: B
    } else if (marks >= 50) {
        cout << "Grade: C" << endl;
    } else {
        cout << "Grade: F" << endl;
    }

    return 0;
}
```

---

### [**3.3.1.5 switch Statement**](#3315-switch-statement)
The `switch` statement selects one of many code blocks to execute based on the value of an expression.

**Syntax**:
```cpp
switch (expression) {
    case constant1:
        // Code for case 1
        break;
    case constant2:
        // Code for case 2
        break;
    // Add more cases as needed
    default:
        // Code if no case matches
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int day = 3;

    switch (day) {
        case 1:
            cout << "Monday" << endl;
            break;
        case 2:
            cout << "Tuesday" << endl;
            break;
        case 3:
            cout << "Wednesday" << endl; // Output: Wednesday
            break;
        default:
            cout << "Invalid day" << endl;
    }

    return 0;
}
```

---

## [**3.3.2 Looping Statements**](#332-looping-statements)

### [**3.3.2.1 while Loop**](#3321-while-loop)
Executes a block of code as long as the condition is `true`.

**Syntax**:
```cpp
while (condition) {
    // Code to execute
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 1;

    while (i <= 5) {
        cout << i << " "; // Output: 1 2 3 4 5
        i++;
    }

    return 0;
}
```

---

### [**3.3.2.2 do-while Loop**](#3322-do-while-loop)
Executes the block of code once before checking the condition.

**Syntax**:
```cpp
do {
    // Code to execute
} while (condition);
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 1;

    do {
        cout << i << " "; // Output: 1 2 3 4 5
        i++;
    } while (i <= 5);

    return 0;
}
```

---

### [**3.3.2.3 for Loop**](#3323-for-loop)
Executes a block of code a specific number of times.

**Syntax**:
```cpp
for (initialization; condition; increment/decrement) {
    // Code to execute
}
```

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        cout << i << " "; // Output: 1 2 3 4 5
    }

    return 0;
}
```

---

### [**3.3.2.4 Range-based for Loop**](#3324-range-based-for-loop)
Iterates over a range or collection.

**Example**:
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};

    for (int num : numbers) {
        cout << num << " "; // Output: 1 2 3 4 5
    }

    return 0;
}
```

---

## [**3.3.3 Jump Statements](#333-jump-statements)

### [**3.3.3.1 break**](#3331-break)
Exits the current loop or switch statement.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        if (i == 3) break;
        cout << i << " "; // Output: 1 2
    }

    return 0;
}
```

---

### [**3.3.3.2 continue**](#3332-continue)
Skips the current iteration of a loop.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        if (i == 3) continue;
        cout << i << " "; // Output: 1 2 4 5
    }

    return 0;
}
```

---

### [**3.3.3.3 goto**](#3333-goto)
Jumps to a labeled statement.

**Example**:
```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 1;

    start:
    if (i <= 5) {
        cout << i << " "; // Output: 1 2 3 4 5
        i++;
        goto start;
    }

    return 0;
}
```

---

## [**Best Practices**](#best-practices)
1. Use `switch` for multiple fixed conditions.
2. Avoid excessive use of `goto` as it can lead to spaghetti code.
3. Always include a `default` case in `switch`.
4. Choose the loop type based on the use case:
   - `for` for known iterations.
   - `while` for conditional loops.
   - `do-while` for at least one execution.
