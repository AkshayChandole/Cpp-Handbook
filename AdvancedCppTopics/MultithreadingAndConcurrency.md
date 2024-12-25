
# [5.6 Multithreading and Concurrency](#56-multithreading-and-concurrency)

Multithreading is the ability of a program to perform multiple tasks simultaneously by dividing them into smaller units called threads. Concurrency is the concept of managing multiple threads to execute in a coordinated manner, ensuring data safety and program stability.

---

## [5.6.1 Threads in C++](#561-threads-in-c)

A thread is the smallest unit of execution within a program. In C++, you can create threads using the `std::thread` class. Threads can run functions or callable objects concurrently.

**Detailed Explanation**:
- **Creating Threads**: You pass a function or callable object to a thread constructor.
- **Joining Threads**: The `join()` method ensures the main thread waits for the spawned thread to complete.
- **Detaching Threads**: The `detach()` method lets a thread run independently, but you lose control over it.

**Example**:

```cpp
#include <iostream>
#include <thread>
using namespace std;

void printMessage() {
    cout << "Hello from a thread!" << endl;
}

int main() {
    thread t1(printMessage); // Create a thread to run printMessage
    t1.join(); // Main thread waits for t1 to finish
    cout << "Main thread done." << endl;
    return 0;
}

// Output:
// Hello from a thread!
// Main thread done.
```

---

## [5.6.2 Mutexes and Synchronization](#562-mutexes-and-synchronization)

When multiple threads access shared resources, they might conflict, leading to **data races**. A `std::mutex` is used to prevent this by ensuring only one thread can access a critical section at a time.

**Detailed Explanation**:
- **Locking the Mutex**: Use `mtx.lock()` to acquire a lock and `mtx.unlock()` to release it.
- **Scoped Locking**: Prefer `std::lock_guard` for automatic locking and unlocking.

**Example**:

```cpp
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;

mutex mtx;

void printNumbers(int id) {
    mtx.lock(); // Acquire the lock
    for (int i = 0; i < 5; ++i) {
        cout << "Thread " << id << ": " << i << endl;
    }
    mtx.unlock(); // Release the lock
}

int main() {
    thread t1(printNumbers, 1);
    thread t2(printNumbers, 2);

    t1.join();
    t2.join();
    return 0;
}

// Output (order may vary):
// Thread 1: 0
// Thread 1: 1
// ...
// Thread 2: 0
```

---

## [5.6.3 Condition Variables](#563-condition-variables)

Condition variables are used to make threads wait until certain conditions are met. This is useful for coordinating threads.

**Detailed Explanation**:
- **`wait` Method**: Makes a thread wait for a condition.
- **`notify_one`/`notify_all`**: Signals one or all waiting threads to proceed.

**Example**:

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
using namespace std;

mutex mtx;
condition_variable cv;
bool ready = false;

void worker() {
    unique_lock<mutex> lock(mtx);
    cv.wait(lock, [] { return ready; }); // Wait until ready is true
    cout << "Worker thread proceeding." << endl;
}

void notifier() {
    {
        lock_guard<mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one(); // Notify waiting thread
}

int main() {
    thread t1(worker);
    thread t2(notifier);

    t1.join();
    t2.join();
    return 0;
}

// Output:
// Worker thread proceeding.
```

---

## [5.6.4 Thread Safety and Atomic Operations](#564-thread-safety-and-atomic-operations)

Atomic operations provide lightweight synchronization by guaranteeing that operations on shared data are performed atomically (without interruption).

**Detailed Explanation**:
- Use the `std::atomic` class for thread-safe shared variables.
- Atomic operations are faster than locking with mutexes for simple tasks.

**Example**:

```cpp
#include <iostream>
#include <thread>
#include <atomic>
using namespace std;

atomic<int> counter(0);

void increment() {
    for (int i = 0; i < 1000; ++i) {
        counter++; // Atomic increment
    }
}

int main() {
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Final counter value: " << counter << endl;
    return 0;
}

// Output:
// Final counter value: 2000
```

---

## [5.6.5 Thread Pools](#565-thread-pools)

A thread pool is a collection of reusable threads used to execute tasks. Instead of creating and destroying threads repeatedly, you can reuse threads for multiple tasks.

**Detailed Explanation**:
- Thread pools reduce overhead by reusing threads.
- Tasks are queued and executed by idle threads in the pool.

**Example**:

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <queue>
#include <mutex>
#include <condition_variable>
#include <functional>
using namespace std;

class ThreadPool {
    vector<thread> workers;
    queue<function<void()>> tasks;
    mutex mtx;
    condition_variable cv;
    bool stop = false;

public:
    ThreadPool(size_t threads) {
        for (size_t i = 0; i < threads; ++i) {
            workers.emplace_back([this] {
                while (true) {
                    function<void()> task;
                    {
                        unique_lock<mutex> lock(mtx);
                        cv.wait(lock, [this] { return stop || !tasks.empty(); });
                        if (stop && tasks.empty()) return;
                        task = move(tasks.front());
                        tasks.pop();
                    }
                    task();
                }
            });
        }
    }

    void enqueue(function<void()> task) {
        {
            lock_guard<mutex> lock(mtx);
            tasks.push(task);
        }
        cv.notify_one();
    }

    ~ThreadPool() {
        {
            lock_guard<mutex> lock(mtx);
            stop = true;
        }
        cv.notify_all();
        for (thread &worker : workers) {
            worker.join();
        }
    }
};

int main() {
    ThreadPool pool(4);

    for (int i = 0; i < 8; ++i) {
        pool.enqueue([i] {
            cout << "Task " << i << " is running on thread " << this_thread::get_id() << endl;
        });
    }
    return 0;
}

// Output:
// Task outputs may vary, depending on scheduling.
```

---
