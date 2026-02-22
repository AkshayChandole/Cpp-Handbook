# [How To Stop Someone To Inherit From Your Class In C++?](#how-to-stop-someone-to-inherit-from-your-class-in-c)


There are multiple ways to prevent inheritance depending on language standard and design goals.

---

## ✅ 1️⃣ Use `final` Keyword (C++11 and above) — Recommended

This is the cleanest and modern way.

```cpp
class Base final {
public:
    void show() {
        std::cout << "Base class\n";
    }
};
```

Now this will fail:

```cpp
class Derived : public Base {};  // ❌ Compile-time error
```

Compiler error:

```text
error: cannot derive from ‘final’ base ‘Base’
```

---

### 🔹 Why `final` Is Best?

* Compile-time enforced
* Clear intent
* Zero runtime overhead
* Standard and readable

---

## ✅ 2️⃣ Make Constructor Private (Pre-C++11 Trick)

Before `final` existed, people used this technique.

```cpp
class Base {
private:
    Base() {}

public:
    static Base create() {
        return Base();
    }
};
```

Now:

```cpp
class Derived : public Base {};  // ❌ Error
```

Why?

Because derived constructor must call Base constructor — but it’s private.

---

⚠️ Limitation:

If you make constructor private, you also prevent object creation unless you provide factory methods.

---

## ✅ 3️⃣ Make Destructor Private or Deleted

```cpp
class Base {
private:
    ~Base() {}
};
```

This prevents inheritance because derived class destructor must call base destructor.

But this also makes stack allocation tricky.

Better variant:

```cpp
class Base {
public:
    ~Base() = delete;
};
```

But this blocks object creation entirely.

So this is rarely practical.

---

## 🔥 4️⃣ Prevent Further Inheritance Only (Allow One Level)

You can combine inheritance + final:

```cpp
class Base {
};

class Derived final : public Base {
};
```

Now:

```cpp
class Child : public Derived {};  // ❌ Error
```

This allows only one inheritance level.

---

## 🔹 5️⃣ Prevent Overriding Specific Virtual Function

You can mark function as `final`:

```cpp
class Base {
public:
    virtual void foo() final {}
};
```

Now derived class cannot override `foo()`.

---

## 🔥 Memory-Level Understanding

Why does private constructor stop inheritance?

Because derived class constructor automatically calls:

```cpp
Base::Base();
```

If it’s inaccessible, compilation fails.

---

## 🔹 Interview-Level Answer

If interviewer asks:

> How do you prevent inheritance?

Answer:

> In modern C++, we use the `final` keyword after the class declaration. This prevents any class from inheriting from it at compile time. Before C++11, inheritance could be prevented by making constructors private so derived classes could not call them.

---

## 🔹 Best Practice

Use:

```cpp
class MyClass final {};
```

It clearly expresses intent.

---

## 🔹 Real-World Use Cases

Prevent inheritance when:

* Class represents value type (e.g., utility class)
* Security-sensitive logic
* Immutable design
* You don’t want polymorphic misuse
* You want stable ABI

---

## 🔥 Advanced: Why Java Needed final but C++ Didn’t Initially?

C++ historically allowed full control.
C++11 introduced `final` for clearer intent and safer design.

---
