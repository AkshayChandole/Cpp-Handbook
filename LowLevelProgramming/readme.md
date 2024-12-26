# [**6. Low-Level Programming**](#6-low-level-programming)

This section dives into critical concepts for managing system resources, ensuring performance optimization, and utilizing multithreading effectively in C++. Understanding these topics is fundamental for writing high-performance and efficient applications.

---

## [**6.1 Memory Management**](#61-memory-management)

Memory management in C++ deals with how memory is allocated, used, and freed. It is crucial for ensuring efficient use of system resources and preventing issues like memory leaks or crashes.

### [**6.1.1 Stack vs Heap**](#611-stack-vs-heap)

**1. Stack:**
- **Definition:** Stack memory is a region of memory that stores local variables and function call data.
- **Characteristics:**
  - Memory is managed automatically by the compiler.
  - Fast allocation and deallocation.
  - Limited size, usually predefined by the system.
  - Data is stored in a Last-In-First-Out (LIFO) manner.
- **Use Cases:**
  - Storing function-local variables.
  - Fixed-size objects.
- **Limitations:**
  - Cannot allocate dynamic memory.
  - Limited lifetime: data is valid only within the function scope.
  
**2. Heap:**
- **Definition:** Heap memory is a region used for dynamic memory allocation.
- **Characteristics:**
  - Managed manually by the programmer using `new`, `delete`, or functions like `malloc` and `free`.
  - Slower allocation and deallocation compared to the stack.
  - Much larger size than the stack.
- **Use Cases:**
  - Objects that need to persist beyond the scope of a function.
  - Large objects that exceed stack limits.
- **Limitations:**
  - Improper management leads to memory leaks or dangling pointers.

---

### [**6.1.2 Allocating and Deallocating Memory**](#612-allocating-and-deallocating-memory)

**1. Dynamic Memory Allocation:**
- **Using `new` and `delete`:**
  ```cpp
  int* ptr = new int(10);  // Allocates memory on the heap
  delete ptr;              // Frees the memory
  ```
  - `new`: Allocates memory and calls the constructor for objects.
  - `delete`: Frees memory and calls the destructor for objects.

- **Using `malloc` and `free`:**
  ```cpp
  int* ptr = (int*)malloc(sizeof(int));  // Allocates memory
  free(ptr);                             // Frees memory
  ```
  - `malloc`/`free` are C-style functions and do not call constructors or destructors.

**2. Memory Leaks:**
- Occur when allocated memory is not freed.
- **Example of a memory leak:**
  ```cpp
  int* ptr = new int(10);
  // No delete statement -> memory leak
  ```

**3. Best Practices:**
- Always pair `new` with `delete` and `malloc` with `free`.
- Use smart pointers (`std::unique_ptr`, `std::shared_ptr`) to manage memory automatically.
- Avoid dangling pointers by setting them to `nullptr` after freeing memory.

---

## [**6.2 Multithreading**](#62-multithreading)

Multithreading allows programs to perform multiple operations concurrently, improving performance on multi-core systems. Modern C++ provides robust support for multithreading through the `<thread>` library.

### [**6.2.1 Threads in C++11**](#621-threads-in-c11)

**1. Creating Threads:**
- Threads can be created using the `std::thread` class.
- **Example:**
  ```cpp
  #include <iostream>
  #include <thread>

  void printMessage() {
      std::cout << "Hello from a thread!" << std::endl;
  }

  int main() {
      std::thread t1(printMessage); // Create a thread
      t1.join();                   // Wait for the thread to finish
      return 0;
  }
  ```

**2. Thread Lifecycle:**
- **join():** Blocks the calling thread until the created thread finishes.
- **detach():** Separates the created thread, allowing it to run independently.

**3. Passing Arguments to Threads:**
- **Example:**
  ```cpp
  void printNumber(int num) {
      std::cout << "Number: " << num << std::endl;
  }

  std::thread t1(printNumber, 42); // Pass argument to thread
  t1.join();
  ```

**4. Lambda Functions with Threads:**
- **Example:**
  ```cpp
  std::thread t([]() {
      std::cout << "Hello from a lambda thread!" << std::endl;
  });
  t.join();
  ```

---

### [**6.2.2 Thread Synchronization**](#622-thread-synchronization)

Synchronization ensures that multiple threads access shared resources safely.

**1. Mutexes:**
- `std::mutex` prevents simultaneous access to shared resources.
- **Example:**
  ```cpp
  #include <iostream>
  #include <thread>
  #include <mutex>

  std::mutex mtx;

  void printSafe(int id) {
      mtx.lock();
      std::cout << "Thread " << id << " is running." << std::endl;
      mtx.unlock();
  }

  int main() {
      std::thread t1(printSafe, 1);
      std::thread t2(printSafe, 2);

      t1.join();
      t2.join();
      return 0;
  }
  ```

**2. RAII with `std::lock_guard`:**
- Automatically locks and unlocks a mutex.
- **Example:**
  ```cpp
  void printSafe(int id) {
      std::lock_guard<std::mutex> lock(mtx);
      std::cout << "Thread " << id << " is running safely." << std::endl;
  }
  ```

**3. Condition Variables:**
- Used for thread communication.
- **Example:**
  ```cpp
  #include <iostream>
  #include <thread>
  #include <mutex>
  #include <condition_variable>

  std::mutex mtx;
  std::condition_variable cv;
  bool ready = false;

  void worker() {
      std::unique_lock<std::mutex> lock(mtx);
      cv.wait(lock, [] { return ready; });
      std::cout << "Worker thread proceeding." << std::endl;
  }

  int main() {
      std::thread t(worker);

      {
          std::lock_guard<std::mutex> lock(mtx);
          ready = true;
      }
      cv.notify_one();
      t.join();
      return 0;
  }
  ```

**4. Atomic Operations:**
- `std::atomic` provides lock-free operations for shared variables.
- **Example:**
  ```cpp
  #include <atomic>

  std::atomic<int> counter(0);

  void increment() {
      counter.fetch_add(1);
  }
  ```

**5. Deadlocks and Prevention:**
- Occur when two or more threads wait indefinitely for each other to release locks.
- **Prevention Strategies:**
  - Lock resources in a consistent order.
  - Use `std::try_lock` to avoid blocking.

---

## **Conclusion**
Low-level programming in C++ provides developers with fine control over memory and concurrency, enabling high-performance applications. Mastery of these concepts is essential for tackling system-level programming and optimizing resource-intensive operations.

---
