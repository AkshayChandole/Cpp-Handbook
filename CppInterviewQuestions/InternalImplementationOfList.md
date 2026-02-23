# [How is std::list implemented internally?](#How-is-stdlist-implemented-internally)

### 🔹 std::list – Internal Implementation

`std::list` is implemented as a **doubly linked list**.

Unlike `vector` and `deque`:

* ❌ Not contiguous
* ❌ No random access
* ✅ Stable iterators
* ✅ O(1) insert/erase anywhere (given iterator)

Let’s break it down properly (interview-ready depth).

---

## 1️⃣ Core Node Structure

Internally, list stores elements inside nodes:

```cpp
template<typename T>
struct ListNode {
    ListNode* prev;
    ListNode* next;
    T data;
};
```

Each element has:

* 2 pointers (prev, next)
* Actual stored value

Memory layout:

```
[prev | data | next]  ->  [prev | data | next]  ->  ...
```

---

## 2️⃣ Sentinel (Dummy) Node – Important

Most STL implementations use a **circular list with a sentinel node**.

Why?

* Avoid null checks
* Simplify insert/erase logic
* Make empty list easier to handle

Structure:

```
          ┌───────────────┐
          │   sentinel    │
          └───────────────┘
             ↑         ↓
           first     last
```

In empty list:

```
sentinel->next = sentinel
sentinel->prev = sentinel
```

So list is circular internally.

---

## 3️⃣ Internal Data Members

Conceptually:

```cpp
template<typename T>
class List {
    ListNode<T>* sentinel;  // dummy node
    size_t size_;
};
```

Some implementations store size, some compute on demand.

---

## 4️⃣ push_back() Implementation

Steps:

1. Create new node
2. Link it before sentinel

Pseudo:

```cpp
void push_back(const T& value) {
    ListNode<T>* node = new ListNode<T>{nullptr, nullptr, value};

    node->prev = sentinel->prev;
    node->next = sentinel;

    sentinel->prev->next = node;
    sentinel->prev = node;
}
```

O(1)

---

## 5️⃣ push_front() Implementation

Insert after sentinel:

```cpp
void push_front(const T& value) {
    ListNode<T>* node = new ListNode<T>{nullptr, nullptr, value};

    node->next = sentinel->next;
    node->prev = sentinel;

    sentinel->next->prev = node;
    sentinel->next = node;
}
```

O(1)

---

## 6️⃣ erase(iterator) Internals

Suppose we want to erase node `cur`:

```cpp
cur->prev->next = cur->next;
cur->next->prev = cur->prev;

delete cur;
```

O(1)

No shifting required.

---

## 7️⃣ Why No Random Access?

To access nth element:

```cpp
iterator it = begin();
std::advance(it, n);
```

Requires traversing n nodes.

So:

```
operator[] does NOT exist
```

Because complexity would be O(n).

---

## 8️⃣ Iterator Implementation

Iterator usually wraps node pointer:

```cpp
template<typename T>
class ListIterator {
    ListNode<T>* node;

public:
    T& operator*() {
        return node->data;
    }

    ListIterator& operator++() {
        node = node->next;
        return *this;
    }
};
```

Bidirectional iterator:

* Supports ++
* Supports --

---

## 9️⃣ Memory Behavior

Each element is separately allocated:

```
new Node()
new Node()
new Node()
```

This means:

❌ Poor cache locality
❌ Higher memory overhead (2 pointers per element)
❌ Fragmented memory

But:

✅ Iterator stability
✅ No reallocation ever
✅ O(1) insert anywhere

---

## 🔟 Iterator Invalidation Rules

| Operation  | Invalidates         |
| ---------- | ------------------- |
| insert     | No                  |
| erase      | Only erased element |
| push_back  | No                  |
| push_front | No                  |

This is why list is used when stable iterators are needed.

---

## 1️⃣1️⃣ Complexity Summary

| Operation       | Complexity           |
| --------------- | -------------------- |
| push_front      | O(1)                 |
| push_back       | O(1)                 |
| insert          | O(1) (with iterator) |
| erase           | O(1)                 |
| random access   | O(n)                 |
| memory overhead | High                 |

---

## 1️⃣2️⃣ How splice() Works (Important Interview Topic)

One powerful feature:

```cpp
list1.splice(pos, list2);
```

Moves nodes from list2 to list1 in O(1).

How?

It just rewires pointers — no copying.

That’s a big advantage over vector.

---

## 💻 Minimal Custom List Implementation

```cpp
template<typename T>
class SimpleList {
    struct Node {
        T data;
        Node* prev;
        Node* next;
    };

    Node* sentinel;

public:
    SimpleList() {
        sentinel = new Node{};
        sentinel->next = sentinel;
        sentinel->prev = sentinel;
    }

    void push_back(const T& value) {
        Node* node = new Node{value, sentinel->prev, sentinel};
        sentinel->prev->next = node;
        sentinel->prev = node;
    }
};
```

Real STL version adds:

* Allocator support
* Exception safety
* Move semantics
* Iterator categories
* Size tracking

---
