# [What Is Return Value Of printf And scanf In C/C++?](#what-is-return-value-of-printf-and-scanf-in-cc)


## 🔹 1️⃣ Return Value of `printf`

#### ✅ `printf` returns:

> The number of characters successfully printed.

#### ❌ If an error occurs:

It returns a **negative value**.

---

### 🔹 Example

```cpp
#include <stdio.h>

int main() {
    int count = printf("Hello World\n");
    printf("Characters printed: %d\n", count);
    return 0;
}
```

#### Output:

```
Hello World
Characters printed: 12
```

Explanation:

```
"Hello World\n"
```

Count includes:

* 11 letters
* 1 newline
  = 12 characters

---

### 🔹 Important Notes

```c
int printf(const char *format, ...);
```

* Return type → `int`
* Includes all printed characters (including spaces, newline, etc.)
* Useful for error checking

Example error check:

```c
if (printf("Test") < 0) {
    perror("Print failed");
}
```

---

## 🔹 2️⃣ Return Value of `scanf`

#### ✅ `scanf` returns:

> The number of input items successfully assigned.

⚠️ Important:
It returns **how many variables were successfully filled**, NOT how many characters were read.

---

### 🔹 Example 1

```cpp
#include <stdio.h>

int main() {
    int a, b;

    int result = scanf("%d %d", &a, &b);

    printf("scanf returned: %d\n", result);
    return 0;
}
```

#### Case 1: User enters

```
10 20
```

Return value:

```
2
```

Because:

* Two integers successfully assigned.

---

#### Case 2: User enters

```
10 abc
```

Return value:

```
1
```

Because:

* First integer read successfully
* Second failed

---

#### Case 3: Invalid input immediately

```
abc
```

Return value:

```
0
```

Because:

* No variable assigned

---

#### Case 4: End of file (Ctrl+D / Ctrl+Z)

Return value:

```
EOF  (-1)
```

---

## 🔹 scanf Signature

```c
int scanf(const char *format, ...);
```

---

## 🔥 Important Difference

| Function | Return Value                     |
| -------- | -------------------------------- |
| printf   | Number of characters printed     |
| scanf    | Number of successful assignments |

---

## 🔹 Why This Matters (Interview Insight)

Proper input validation:

```c
int x;
if (scanf("%d", &x) != 1) {
    printf("Invalid input\n");
}
```

Never ignore `scanf` return value in production code.

---

## 🔹 Common Interview Trick Question

Q:
What does this return?

```c
printf("%d", printf("Hello"));
```

Answer:

Step 1:
Inner printf prints "Hello"
Returns 5

Step 2:
Outer printf prints 5
Returns 1

Output:

```
Hello5
```

---

## 🔹 Final Interview-Ready Answer

If interviewer asks:

> What is the return value of printf and scanf?

You answer:

> printf returns the number of characters printed, or a negative value on error. scanf returns the number of input items successfully assigned. If no input is assigned it returns 0, and if end-of-file is encountered before any assignment, it returns EOF.

---
* How formatted I/O works internally

Tell me how deep you want 🚀
