
#  Core C++ Concepts
The **Core C++ Concepts** section provides the foundational building blocks for understanding and working with C++. 

These concepts are crucial for both day-to-day programming and acing technical interviews. 

Let’s dive into each subtopic in detail:

---

## Table of Contents

   - [3.1 Data Types and Variables](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/DataTypesAndVariables.md#data-types-and-variables)  
     - [3.1.1 Primitive Data Types](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/DataTypesAndVariables.md#primitive-data-types)  
     - [3.1.2 User-Defined Data Types](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/DataTypesAndVariables.md#user-defined-data-types)  
   - [3.2 Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#operators)
     - [3.2.1 Arithmetic Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#321-arithmetic-operators)  
     - [3.2.2 Relational Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#322-relational-comparison-operators)  
     - [3.2.3 Logical Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#323-logical-operators)  
     - [3.2.4 Bitwise Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#324-bitwise-operators)  
     - [3.2.5 Assignment Operators](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Operators.md#325-assignment-operators)  
   - [3.3 Control Flow](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#33-control-flow)  
     - [3.3.1 Conditional Statements](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#331-conditional-statements)
        - [3.3.1.1 `if` statement](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3311-if-statement)
        - [3.3.1.2 `if-else` statement](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3312-if-else-statement)
        - [3.3.1.3 `nested-if-else`](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3313-nested-if-else)
        - [3.3.1.4 `else-if` ladder](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3314-else-if-ladder)
        - [3.3.1.5 `switch` statement](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3315-switch-statement)
     - [3.3.2 Loops](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#332-looping-statements)  
       - [3.3.2.1 `while` Loop](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3321-while-loop)  
       - [3.3.2.2 `do-while` Loop](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3322-do-while-loop)  
       - [3.3.2.3 `for` Loop](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3323-for-loop)
       - [3.3.2.4 Range based for loop](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3324-range-based-for-loop)
     - [3.3.3 Jump Statements](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#333-jump-statements)
        - [3.3.3.1 `break`](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3331-break)
        - [3.3.3.2 `continue`](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3332-continue)
        - [3.3.3.3 `goto`](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/ControlFlow.md#3333-goto)
      
   - [3.4 Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#34-functions-in-c)
      - [3.4.1 Function Declaration and Definition](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#341-function-declaration-and-definition)
      - [3.4.2 Types of Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#342-types-of-functions)
         - [3.4.2.1 Built-in Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3421-built-in-functions)
         - [3.4.2.2 User-Defined Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3422-user-defined-functions)
         - [3.4.2.3 Lambda Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3423-lambda-functions)
      - [3.4.3 Parameter Passing](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#343-parameter-passing)
         - [3.4.3.1 Pass by Value](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3431-pass-by-value)
         - [3.4.3.2 Pass by Reference](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3432-pass-by-reference)
         - [3.4.3.3 Default Parameters](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3433-default-parameters)
      - [3.4.4 Function Overloading](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#344-function-overloading)
      - [3.4.5 Inline Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#345-inline-functions)
      - [3.4.6 Recursive Functions](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#346-recursive-functions)
      - [3.4.7 Scope and Lifetime of Variables](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#347-scope-and-lifetime-of-variables)
         - [3.4.7.1 Local Variables](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3471-local-variables)
         - [3.4.7.2 Global Variables](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#3472-global-variables)
      - [3.4.8 Best Practices](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#348-best-practices)
      - [3.4.9 Exceptional Cases and Edge Scenarios](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/CoreCppConcepts/Functions.md#349-exceptional-cases-and-edge-scenarios)
    
   - [3.5 Arrays and Strings](#arrays-and-strings)  
     - [3.5.1 Single-Dimensional Arrays](#single-dimensional-arrays)  
     - [3.5.2 Multi-Dimensional Arrays](#multi-dimensional-arrays)  
     - [3.5.3 String Manipulation](#string-manipulation)  
   - [3.6 Pointers and References](#pointers-and-references)  
     - [3.6.1 Basics of Pointers](#basics-of-pointers)  
     - [3.6.2 Dynamic Memory Allocation](#dynamic-memory-allocation)  
     - [3.6.3 Pointer Arithmetic](#pointer-arithmetic)  

---
