

# [Command Design Pattern](#command-design-pattern)

## 📌 Intent (Canonical Definition)

> **Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undo/redo operations.**

In simple words:

> **Turn a request (action) into an object.**

---

## 🧠 What Problem Does Command Solve?

Without Command:

* Invoker directly calls receiver methods
* Tight coupling between UI / caller and business logic
* Hard to add undo/redo
* Hard to queue, log, or schedule actions

Command:

* Decouples **who invokes** an action from **who performs** it
* Makes requests first-class objects

---

## ❌ Problem Without Command

```cpp
class Light {
public:
    void on()  { std::cout << "Light ON\n"; }
    void off() { std::cout << "Light OFF\n"; }
};

class Remote {
public:
    void pressOn() {
        Light light;
        light.on();   // tight coupling
    }
};
```

Problems:

* `Remote` depends on `Light`
* No flexibility
* Cannot support undo/redo

---

## ✅ Command Pattern Solution

---

# 🧩 Key Participants

| Role                | Responsibility                             |
| ------------------- | ------------------------------------------ |
| **Command**         | Declares interface for executing a request |
| **ConcreteCommand** | Binds a receiver and implements execute    |
| **Receiver**        | Performs the actual action                 |
| **Invoker**         | Triggers the command                       |
| **Client**          | Creates and configures commands            |

---

# 💡 C++ Example: Remote Control

---

## 1️⃣ Command Interface

```cpp
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() { }
    virtual ~Command() = default;
};
```

---

## 2️⃣ Receiver

```cpp
class Light {
public:
    void on()  { std::cout << "Light ON\n"; }
    void off() { std::cout << "Light OFF\n"; }
};
```

---

## 3️⃣ Concrete Commands

```cpp
class LightOnCommand : public Command {
    Light& light;

public:
    LightOnCommand(Light& l) : light(l) {}

    void execute() override {
        light.on();
    }

    void undo() override {
        light.off();
    }
};

class LightOffCommand : public Command {
    Light& light;

public:
    LightOffCommand(Light& l) : light(l) {}

    void execute() override {
        light.off();
    }

    void undo() override {
        light.on();
    }
};
```

---

## 4️⃣ Invoker

```cpp
class RemoteControl {
    Command* command;

public:
    void setCommand(Command* c) {
        command = c;
    }

    void pressButton() {
        command->execute();
    }

    void pressUndo() {
        command->undo();
    }
};
```

---

## 5️⃣ Client Code

```cpp
int main() {
    Light livingRoomLight;

    LightOnCommand onCmd(livingRoomLight);
    LightOffCommand offCmd(livingRoomLight);

    RemoteControl remote;

    remote.setCommand(&onCmd);
    remote.pressButton();   // ON

    remote.pressUndo();     // OFF

    remote.setCommand(&offCmd);
    remote.pressButton();   // OFF
}
```

```cpp
// Output:
// Light ON
// Light OFF
// Light OFF
```

---

## 🔥 Why This Works

✔️ Invoker knows nothing about receiver
✔️ Commands are interchangeable
✔️ Easy undo/redo
✔️ Commands can be queued, logged, replayed

---

## 🔷 Undo / Redo (Very Important Use Case)

Each command stores:

* Enough state to reverse the action

Example:

```cpp
class VolumeCommand : public Command {
    int prevVolume;
};
```

---

## 🔷 Macro Commands (Composite Commands)

```cpp
class MacroCommand : public Command {
    std::vector<Command*> commands;

public:
    void execute() override {
        for (auto c : commands)
            c->execute();
    }
};
```

✔️ Execute multiple commands together
✔️ Very common in editors

---

## 🔷 Command Using `std::function` (Modern C++)

```cpp
class Command {
    std::function<void()> exec;

public:
    Command(std::function<void()> f) : exec(f) {}
    void execute() { exec(); }
};
```

✔️ Lightweight

✔️ No inheritance

❌ Harder to support undo

---

## 🔷 Real-World Use Cases

✔️ GUI buttons and menu items

✔️ Undo / Redo in editors

✔️ Transaction systems

✔️ Job queues

✔️ Macro recording

---

## 🔥 Command vs Strategy (Quick Clarification)

* **Command** → encapsulates a **request/action**
* **Strategy** → encapsulates an **algorithm**

---

## 🔥 Common Interview Traps

❌ Forgetting undo state

❌ Invoker knowing receiver

❌ Overengineering simple callbacks

❌ Confusing with Observer

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Command pattern encapsulates a request as an object, allowing parameterization, queuing, logging, and undoable operations.”

---


