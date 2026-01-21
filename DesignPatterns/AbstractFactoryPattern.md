# [Abstract Factory Pattern](#abstract-factory-pattern)

## 📌 Intent (Canonical Definition)

> **Provide an interface for creating families of related or dependent objects without specifying their concrete classes.**

In simple words:

> **Create multiple related objects together, ensuring they are compatible, without hard-coding their concrete types.**

---

## 🧠 What Problem Does Abstract Factory Solve?

When:

* You need to create **multiple objects**
* Those objects are **related** and must work together
* You want to **switch entire families** of products at once

Abstract Factory:

* Groups object creation
* Ensures consistency across products
* Decouples client from concrete implementations

---

## ❌ Problem Without Abstract Factory

### BAD DESIGN

```cpp
Button* button;
Checkbox* checkbox;

if (os == "Windows") {
    button = new WindowsButton();
    checkbox = new LinuxCheckbox(); // ❌ incompatible mix
}
```

Problems:

* Client creates incompatible products
* Lots of conditionals
* Hard to maintain consistency

---

## ✅ Abstract Factory Solution

---

# 🧩 Structure Overview

### Participants:

1. **AbstractFactory**
2. **ConcreteFactory**
3. **AbstractProduct**
4. **ConcreteProduct**
5. **Client**

---

# 🛠 Example: Cross-Platform UI Toolkit

---

## 1️⃣ Abstract Products

```cpp
class Button {
public:
    virtual void render() = 0;
    virtual ~Button() = default;
};

class Checkbox {
public:
    virtual void render() = 0;
    virtual ~Checkbox() = default;
};
```

---

## 2️⃣ Concrete Products (Windows)

```cpp
class WindowsButton : public Button {
public:
    void render() override {
        std::cout << "Windows Button\n";
    }
};

class WindowsCheckbox : public Checkbox {
public:
    void render() override {
        std::cout << "Windows Checkbox\n";
    }
};
```

---

## 3️⃣ Concrete Products (Mac)

```cpp
class MacButton : public Button {
public:
    void render() override {
        std::cout << "Mac Button\n";
    }
};

class MacCheckbox : public Checkbox {
public:
    void render() override {
        std::cout << "Mac Checkbox\n";
    }
};
```

---

## 4️⃣ Abstract Factory

```cpp
class GUIFactory {
public:
    virtual std::unique_ptr<Button> createButton() = 0;
    virtual std::unique_ptr<Checkbox> createCheckbox() = 0;
    virtual ~GUIFactory() = default;
};
```

---

## 5️⃣ Concrete Factories

```cpp
class WindowsFactory : public GUIFactory {
public:
    std::unique_ptr<Button> createButton() override {
        return std::make_unique<WindowsButton>();
    }

    std::unique_ptr<Checkbox> createCheckbox() override {
        return std::make_unique<WindowsCheckbox>();
    }
};

class MacFactory : public GUIFactory {
public:
    std::unique_ptr<Button> createButton() override {
        return std::make_unique<MacButton>();
    }

    std::unique_ptr<Checkbox> createCheckbox() override {
        return std::make_unique<MacCheckbox>();
    }
};
```

---

## 6️⃣ Client Code

```cpp
void renderUI(GUIFactory& factory) {
    auto button = factory.createButton();
    auto checkbox = factory.createCheckbox();

    button->render();
    checkbox->render();
}

int main() {
    WindowsFactory windows;
    MacFactory mac;

    renderUI(windows);
    renderUI(mac);
}
```

---

### Output

```cpp
// Windows Button
// Windows Checkbox
// Mac Button
// Mac Checkbox
```

---

## 🔥 Why This Works

✔️ Products are **compatible by design**
✔️ Client is completely decoupled
✔️ Switching product families is easy
✔️ No conditional logic in client

---

## 🔷 Key Difference from Factory Method (INTERVIEW FOCUS)

Abstract Factory:

* Creates **multiple related products**
* Ensures **family consistency**

Factory Method:

* Creates **one product at a time**

---

## 🔷 When to Use Abstract Factory

✔️ Families of related objects
✔️ Need consistency across products
✔️ Platform-specific implementations
✔️ Plugin architectures

---

## 🔷 When NOT to Use It

❌ Only one product
❌ No family consistency needed
❌ Simple object creation

---

## 🔥 Common Interview Traps

❌ Confusing Abstract Factory with Factory Method
❌ Returning raw pointers
❌ Hard-coding concrete classes in client
❌ Using strings for family selection

---

## 🔷 Abstract Factory Variations (Advanced)

### 1️⃣ Factory as Singleton

```cpp
GUIFactory& factory = WindowsFactory::instance();
```

### 2️⃣ Abstract Factory with Configuration

```cpp
std::unique_ptr<GUIFactory> factory = createFactoryFromConfig();
```

### 3️⃣ Template-based Abstract Factory (Compile-time)

---

## 🔥 Real-World Examples

* GUI toolkits (Qt, wxWidgets)
* Database drivers
* Rendering engines
* CAD feature families (very Autodesk-relevant)

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Abstract Factory pattern provides an interface for creating families of related objects without specifying their concrete classes.”

---

