# [Class Relations](#class-relations)

<a id="association-weakest-relationship"></a>
## 1️⃣ [Association (Weakest Relationship)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/DesignPatterns/ClassRelations.md#1%EF%B8%8F%E2%83%A3-association-weakest-relationship)

### 🔹 Definition

> **Association** means *one class uses or knows about another class*,
> but **does NOT own it** and **does NOT control its lifetime**.

* Relationship: *uses-a*
* Lifetime: **independent**
* Ownership: ❌ none

---

### 🔹 Real-world example

> A **Teacher** teaches a **Student**
> Both exist independently.

---

### 🔹 C++ Example

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/98934b1f-f706-45fd-b463-791c10d1f339" />


```cpp
#include <iostream>
#include <string>

class Student {
public:
    std::string name;

    Student(const std::string& n) : name(n) {}
};

class Teacher {
public:
    void teach(Student& s) {   // association
        std::cout << "Teaching " << s.name << "\n";
    }
};

int main() {
    Student s("Akshay");
    Teacher t;

    t.teach(s);
}
```

```cpp
// Output:
// Teaching Akshay
```

---

### 🔹 Key Observations

* `Teacher` does NOT store `Student`
* `Teacher` does NOT delete `Student`
* `Student` exists even if `Teacher` doesn’t

✔️ This is **pure association**

---

### 🔹 When to use Association

* Temporary interaction
* Function parameters
* Callbacks
* Utility/helper usage

---

<a id="aggregation-has-a-but-weak-ownership"></a>
## 2️⃣ [Aggregation (Has-a, but weak ownership)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/DesignPatterns/ClassRelations.md#2%EF%B8%8F%E2%83%A3-aggregation-has-a-but-weak-ownership)

### 🔹 Definition

> **Aggregation** is a *has-a* relationship where:

* One object **contains a reference/pointer** to another

* But **does NOT own its lifetime**

* Relationship: *has-a*

* Lifetime: **independent**

* Ownership: ❌ weak / shared

---

### 🔹 Real-world example

> A **Team** has **Players**
> Players can exist without the team.

---

### 🔹 C++ Example (Pointer-based)

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/eddede00-c6e2-499a-b77f-d9a4fbbf6b40" />


```cpp
#include <iostream>
#include <vector>
#include <string>

class Player {
public:
    std::string name;

    Player(const std::string& n) : name(n) {}
};

class Team {
    std::vector<Player*> players;   // aggregation

public:
    void addPlayer(Player* p) {
        players.push_back(p);
    }

    void showPlayers() const {
        for (auto p : players)
            std::cout << p->name << "\n";
    }
};

int main() {
    Player p1("Virat");
    Player p2("Rohit");

    Team india;
    india.addPlayer(&p1);
    india.addPlayer(&p2);

    india.showPlayers();
}
```

```cpp
// Output:
// Virat
// Rohit
```

---

### 🔹 Key Observations

* `Team` stores pointers to `Player`
* `Team` does NOT delete `Player`
* `Player` objects live independently
* Removing `Team` does NOT destroy `Player`

✔️ This is **aggregation**

---

### 🔹 Using smart pointers (preferred in real systems)

```cpp
std::vector<std::shared_ptr<Player>> players;
```

This makes ownership **explicitly shared**.

---

### 🔹 When to use Aggregation

* Shared resources
* Objects reused in multiple owners
* Observer patterns
* Graph-like structures

---

<a id="composition-strongest-relationship"></a>
## 3️⃣ [Composition (Strongest Relationship)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/DesignPatterns/ClassRelations.md#3%EF%B8%8F%E2%83%A3-composition-strongest-relationship)

### 🔹 Definition

> **Composition** is a *has-a* relationship where:

* One object **owns** another

* Contained object **cannot exist without owner**

* Lifetime is **strictly bound**

* Relationship: *owns-a*

* Lifetime: **dependent**

* Ownership: ✅ strong

---

### 🔹 Real-world example

> A **Car** has an **Engine**
> If the car is destroyed, the engine is destroyed.

---

### 🔹 C++ Example (Value member)

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/bbdc108f-63be-4ef9-8aaf-88eb7c57d4e3" />


```cpp
#include <iostream>

class Engine {
public:
    Engine() {
        std::cout << "Engine created\n";
    }

    ~Engine() {
        std::cout << "Engine destroyed\n";
    }
};

class Car {
    Engine engine;   // composition

public:
    Car() {
        std::cout << "Car created\n";
    }

    ~Car() {
        std::cout << "Car destroyed\n";
    }
};

int main() {
    Car c;
}
```

```cpp
// Output:
// Engine created
// Car created
// Car destroyed
// Engine destroyed
```

---

### 🔹 Key Observations

* `Engine` is a **member object**
* Engine is created **with** Car
* Engine is destroyed **with** Car
* No external sharing possible

✔️ This is **composition**

---

### 🔹 Composition with `unique_ptr`

Sometimes composition is dynamic:

```cpp
#include <memory>

class Car {
    std::unique_ptr<Engine> engine;  // still composition

public:
    Car() : engine(std::make_unique<Engine>()) {}
};
```

Ownership is still **exclusive and strict**.

---

### 🔥 Side-by-Side Comparison (INTERVIEW MUST)

| Aspect            | Association | Aggregation     | Composition   |
| ----------------- | ----------- | --------------- | ------------- |
| Relationship      | uses-a      | has-a           | owns-a        |
| Ownership         | ❌           | ❌ / shared      | ✅             |
| Lifetime tied     | ❌           | ❌               | ✅             |
| Pointer/reference | Optional    | Usually pointer | Usually value |
| Destruction       | Independent | Independent     | Dependent     |
| Strength          | Weak        | Medium          | Strong        |

---
