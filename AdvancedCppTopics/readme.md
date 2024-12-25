# [5. Advanced C++ Topics](#5-advanced-c-topics)

The Advanced C++ Topics section delves into more sophisticated features of the language, helping developers to write optimized, scalable, and maintainable code.  

---

#### Table of Contents

[**5.1 Templates**](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#51-templates)
- [5.1.1 Function Templates](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#511-function-templates)  
- [5.1.2 Class Templates](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#512-class-templates)  
- [5.1.3 Variadic Templates](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#513-variadic-templates)  
- [5.1.4 Template Specialization](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#514-template-specialization)  
- [5.1.5 Template Metaprogramming](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/Templates.md#515-template-metaprogramming)  

[**5.2 Exception Handling**](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#52-exception-handling)
- [5.2.1 Try, Catch, and Throw](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#521-try-catch-and-throw)  
- [5.2.2 Standard Exceptions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#522-standard-exceptions)  
- [5.2.3 Custom Exceptions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#523-custom-exceptions)  
- [5.2.4 Exception Safety Levels](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#524-exception-safety-levels)  
- [5.2.5 noexcept Specifier](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/ExceptionHandling.md#525-noexcept-specifier)  

[**5.3 Type Casting**](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#53-type-casting)
- [5.3.1 C-Style Casts](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#531-c-style-cast)  
- [5.3.2 Static Cast](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#532-static-cast)  
- [5.3.3 Dynamic Cast](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#533-dynamic-cast)  
- [5.3.4 Const Cast](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#534-const-cast)  
- [5.3.5 Reinterpret Cast](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/TypeCasting.md#535-reinterpret-cast)  

[**5.4 Smart Pointers**](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SmartPointers.md#54-smart-pointers)
- [5.4.1 Unique Pointers (`std::unique_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SmartPointers.md#541-unique-pointer-stdunique_ptr)  
- [5.4.2 Shared Pointers (`std::shared_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SmartPointers.md#542-shared-pointer-stdshared_ptr)  
- [5.4.3 Weak Pointers (`std::weak_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SmartPointers.md#543-weak-pointer-stdweak_ptr)  
- [5.4.4 Comparison with Raw Pointers](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SmartPointers.md#544-auto-pointer-stdauto_ptr-deprecated)  

[**5.5 Lambda Expressions**]()
- [5.5.1 Syntax and Basics](#551-syntax-and-basics)  
- [5.5.2 Capturing Variables](#552-capturing-variables)  
- [5.5.3 Returning Values](#553-returning-values)  
- [5.5.4 Lambdas with Standard Algorithms](#554-lambdas-with-standard-algorithms)  

[**5.6 Multithreading and Concurrency**]()
- [5.6.1 Threads in C++ (`std::thread`)](#561-threads-in-c-stdthread)  
- [5.6.2 Mutex and Locking Mechanisms](#562-mutex-and-locking-mechanisms)  
- [5.6.3 Condition Variables](#563-condition-variables)  
- [5.6.4 Futures and Promises](#564-futures-and-promises)  
- [5.6.5 Thread Pool Implementation](#565-thread-pool-implementation)  

[**5.7 Standard Template Library (STL)**]()  
- [5.7.1 Containers](#571-containers)  
  - [5.7.1.1 Sequential Containers](#5711-sequential-containers)  
    - [5.7.1.1.1 `std::vector`](#57111-stdvector)  
    - [5.7.1.1.2 `std::deque`](#57112-stddeque)  
    - [5.7.1.1.3 `std::list`](#57113-stdlist)  
  - [5.7.1.2 Associative Containers](#5712-associative-containers)  
    - [5.7.1.2.1 `std::set`](#57121-stdset)  
    - [5.7.1.2.2 `std::map`](#57122-stdmap)  
    - [5.7.1.2.3 `std::multiset`](#57123-stdmultiset)  
    - [5.7.1.2.4 `std::multimap`](#57124-stdmultimap)  
  - [5.7.1.3 Unordered Containers](#5713-unordered-containers)  
    - [5.7.1.3.1 `std::unordered_set`](#57131-stdunordered_set)  
    - [5.7.1.3.2 `std::unordered_map`](#57132-stdunordered_map)  
    - [5.7.1.3.3 `std::unordered_multiset`](#57133-stdunordered_multiset)  
    - [5.7.1.3.4 `std::unordered_multimap`](#57134-stdunordered_multimap)  

- [5.7.2 Iterators](#572-iterators)  
  - [5.7.2.1 Input Iterators](#5721-input-iterators)  
  - [5.7.2.2 Output Iterators](#5722-output-iterators)  
  - [5.7.2.3 Forward Iterators](#5723-forward-iterators)  
  - [5.7.2.4 Bidirectional Iterators](#5724-bidirectional-iterators)  
  - [5.7.2.5 Random Access Iterators](#5725-random-access-iterators)  

- [5.7.3 Algorithms](#573-algorithms)  
  - [5.7.3.1 Searching Algorithms](#5731-searching-algorithms)  
  - [5.7.3.2 Sorting Algorithms](#5732-sorting-algorithms)  
  - [5.7.3.3 Manipulation Algorithms](#5733-manipulation-algorithms)  
  - [5.7.3.4 Numeric Algorithms](#5734-numeric-algorithms)  

- [5.7.4 Function Objects (Functors)](#574-function-objects-functors)  
  - [5.7.4.1 Built-in Functors](#5741-built-in-functors)  
  - [5.7.4.2 Custom Functors](#5742-custom-functors)  

[**5.8 Modern C++ Features**]()
- [5.8.1 Rvalue References and Move Semantics](#581-rvalue-references-and-move-semantics)  
- [5.8.2 `auto` and `decltype`](#582-auto-and-decltype)  
- [5.8.3 Range-Based Loops](#583-range-based-loops)  
- [5.8.4 `nullptr`](#584-nullptr)  
- [5.8.5 Structured Bindings](#585-structured-bindings)  

[**5.9 Debugging and Profiling**]()  
- [5.9.1 Debugging Tools](#591-debugging-tools)  
- [5.9.2 Profiling Techniques](#592-profiling-techniques)  
- [5.9.3 Performance Optimization](#593-performance-optimization)  

---
