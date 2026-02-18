# [delete](#delete)

`delete` has **two completely different meanings** in C++:

1️⃣ **Memory deallocation operator**
2️⃣ **Deleted functions (C++11 feature)**

We’ll cover both fully.

---

## 🧠 PART 1 — `delete` for Memory Deallocation

Used to free heap memory allocated using `new`.

---

### 🔹 1️⃣ Basic Usage

```cpp
int* ptr = new int(10);

delete ptr;   // frees memory
ptr = nullptr; // good practice
```

---

### 🔹 2️⃣ Array Delete

If allocated with `new[]`, must delete with `delete[]`.

```cpp
int* arr = new int[5];

delete[] arr;  // correct
```

❌ Wrong:

```cpp
delete arr; // undefined behavior
```

---

### 🔹 3️⃣ What Happens Internally?

When you call:

```cpp
delete ptr;
```

C++ does:

1. Calls destructor of object
2. Frees memory using operator delete

---

### 🔹 4️⃣ Deleting Null Pointer

```cpp
int* p = nullptr;
delete p;  // SAFE
```

Deleting nullptr does nothing.

---

### 🔹 5️⃣ Double Delete (Danger ⚠)

```cpp
int* p = new int(5);
delete p;
delete p; // undefined behavior (crash possible)
```

Solution:

```cpp
delete p;
p = nullptr;
```

---

### 🔹 6️⃣ Mismatch new/delete

❌ Wrong:

```cpp
int* p = new int[10];
delete p;   // must use delete[]
```

❌ Wrong:

```cpp
int* p = (int*)malloc(sizeof(int));
delete p;  // must use free()
```

Rule:

* new → delete
* new[] → delete[]
* malloc → free

---

### 🔹 7️⃣ Custom delete Operator

You can overload `operator delete`.

```cpp
class Test {
public:
    void* operator new(size_t size) {
        std::cout << "Custom new\n";
        return malloc(size);
    }

    void operator delete(void* ptr) {
        std::cout << "Custom delete\n";
        free(ptr);
    }
};
```

---

### 🔹 8️⃣ delete with Base Class Pointer (Important Interview Question)

```cpp
class Base {
public:
    ~Base() { std::cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    ~Derived() { std::cout << "Derived destructor\n"; }
};

Base* ptr = new Derived();
delete ptr;  // ❌ only Base destructor called!
```

#### Why?

Destructor not virtual.

Fix:

```cpp
virtual ~Base() {}
```

Rule:
👉 Always make base class destructor virtual if polymorphism is used.

---

### 🔹 9️⃣ Smart Pointer Alternative

Instead of manual delete:

```cpp
std::unique_ptr<int> p = std::make_unique<int>(10);
```

Memory automatically released.

---

---

## 🧠 PART 2 — `= delete` (Deleted Functions)

Introduced in C++11.

Used to **disable functions intentionally**.

---

### 🔹 1️⃣ Disable Copy Constructor

```cpp
class A {
public:
    A() {}
    A(const A&) = delete;
};
```

Now:

```cpp
A a1;
A a2 = a1;  // ❌ error
```

Used to make class non-copyable.

---

### 🔹 2️⃣ Disable Assignment Operator

```cpp
A& operator=(const A&) = delete;
```

---

### 🔹 3️⃣ Real Use Case — Singleton Pattern

```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

private:
    Singleton() {}
};
```

---

### 🔹 4️⃣ Prevent Certain Conversions

```cpp
class A {
public:
    A(int) {}
    A(double) = delete;
};
```

Now:

```cpp
A a1(5);    // OK
A a2(3.14); // ❌
```

---

### 🔹 5️⃣ Delete Specific Template Instantiations

```cpp
template<typename T>
void func(T) = delete;

template<>
void func<int>(int x) {
    std::cout << x;
}
```

Only int allowed.

---

### 🔹 6️⃣ delete vs private constructor (Old Way)

Before C++11:

```cpp
class A {
private:
    A(const A&);
};
```

But compiler still generates it.

Modern way:

```cpp
A(const A&) = delete;
```

Clearer + compile-time error.

---

## 🔥 Difference: delete operator vs = delete

| delete        | = delete                 |
| ------------- | ------------------------ |
| Frees memory  | Disables function        |
| Runtime       | Compile-time             |
| Used with new | Used in class definition |

---

## 🔥 Common Interview Questions

#### Q1: What happens if destructor is not virtual?

Only base destructor runs → memory leak.

---

#### Q2: Can you delete stack memory?

```cpp
int x;
delete &x; // ❌ undefined behavior
```

Only delete heap memory allocated with new.

---

#### Q3: Can delete be overloaded?

Yes, operator delete can be overloaded.

---

#### Q4: Is delete slow?

No, usually O(1). Depends on allocator.

---

## 🔥 Best Practices

✔ Prefer smart pointers
✔ Always match new/delete
✔ Make base destructors virtual
✔ Set pointer to nullptr after delete
✔ Use = delete for non-copyable classes

---

## 🔥 Senior-Level Insight

Modern C++ philosophy:

Avoid manual delete whenever possible.

Prefer:

* `std::unique_ptr`
* `std::shared_ptr`
* RAII

Manual delete is mainly needed in:

* Custom memory allocators
* Embedded systems
* Low-level libraries

---

## 🔥 Final Summary

The `delete` keyword in C++ serves:

1️⃣ Memory management (free heap memory)
2️⃣ Function disabling (`= delete`)

---

