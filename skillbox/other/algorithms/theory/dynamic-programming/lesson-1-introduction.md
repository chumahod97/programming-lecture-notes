# Lesson 1: Introduction to Dynamic Programming

## Part 1: Definition and Principles of Dynamic Programming

### Definition of Dynamic Programming

Dynamic Programming (DP) is an algorithmic technique used to solve complex
problems by breaking them down into more manageable subproblems. It is
especially effective for optimizing problems where a solution can be composed
of solutions to subproblems.

### Key Characteristics of Dynamic Programming

1. **Optimal Substructure**:
   * A problem exhibits optimal substructure if an optimal solution to the
     problem can be constructed from optimal solutions to its subproblems.
   * This means if you can determine the solution to a problem by using the
     solutions to smaller subproblems, then the problem has an optimal substructure.

2. **Overlapping Subproblems**:
   * A problem has overlapping subproblems if the same subproblems are solved
     multiple times.
   * Dynamic programming solves each subproblem only once, saving its answer
     in a table (usually an array or matrix) to avoid repeated work, which is
     especially useful when the same subproblems arise again.

### Principles of Dynamic Programming

Dynamic programming is built on two core principles: **Memoization** and **Tabulation**.

1. **Memoization (Top-Down Approach)**:
   * This approach involves solving problems recursively and storing the
     results of expensive function calls.
   * When a recursive function is called, it checks if the result for that
     input is already calculated and stored. If yes, it returns the stored
     result; otherwise, it computes the result, stores it, and then returns it.
   * **Example**: Calculating Fibonacci numbers using recursion and storing
     previously calculated results to avoid redundant calculations.

2. **Tabulation (Bottom-Up Approach)**:
   * This approach involves solving problems iteratively and building a table
     (often an array or matrix) from the smallest subproblem up to the desired
     solution.
   * Start solving the smallest subproblem and use its result to build
     solutions for bigger subproblems, eventually solving the original problem.
   * **Example**: Using an iterative loop to fill an array with Fibonacci
     numbers from the base cases up to the desired n-th Fibonacci number.

### Steps to Solve a Dynamic Programming Problem

1. **Characterize the Structure of an Optimal Solution**:
   * Identify the optimal substructure of the problem by defining a recursive
     solution.

2. **Define the Value of an Optimal Solution Recursively**:
   * Define the problem in terms of smaller subproblems, and express the
     solution to the problem as a recursive formula.

3. **Compute the Value of an Optimal Solution**:
   * Use either memoization or tabulation to compute the value of an optimal
     solution by storing results of subproblems to avoid redundant calculations.

4. **Construct an Optimal Solution from the Computed Information**:
   * Once the solution value is computed, reconstruct the solution from the
     stored subproblem results if needed.

### Example: Fibonacci Sequence

**Problem**: Calculate the n-th Fibonacci number.

* **Recursive Solution**:
  * \( F(n) = F(n-1) + F(n-2) \) with base cases \( F(0) = 0 \) and \( F(1) = 1 \).
* **Memoization**:
  * Use a dictionary or array to store the Fibonacci numbers that have already
    been computed.
* **Tabulation**:
  * Iteratively compute Fibonacci numbers, storing each result in an array
    from \( F(0) \) to \( F(n) \).

## Part 2: Differences Between Dynamic Programming and Other Techniques

In this section, we will explore how dynamic programming differs from other
common algorithmic techniques, such as recursion and divide-and-conquer.
Understanding these differences will help you identify when to apply each
technique most effectively.

### Recursion

**Definition**:
Recursion is a method of solving a problem where the solution depends on
solutions to smaller instances of the same problem. It involves a function
calling itself with a smaller input until it reaches a base case.

**Characteristics**:

* **Direct Function Calls**: A recursive function calls itself directly to
  solve subproblems.
* **Base and Recursive Cases**: Defined by a base case (termination condition)
  and a recursive case (problem-solving step).
* **Simple but Inefficient**: Can be simple to implement for problems with
  straightforward recursive relationships but may lead to inefficiencies due
  to repeated calculations of the same subproblems.

**Limitations**:

* **Redundant Computation**: Recursion can result in the same subproblem being
  solved multiple times, leading to increased time complexity.
* **Stack Overflow**: Deep recursive calls may lead to stack overflow due to
  excessive memory usage.

**Example**: Calculating Fibonacci numbers recursively without memoization
results in exponential time complexity due to repeated calculations.

### Divide-and-Conquer

**Definition**:
Divide-and-conquer is an algorithm design paradigm that solves a problem by
breaking it down into two or more smaller independent subproblems, solving each
subproblem recursively, and then combining their solutions to solve the
original problem.

**Characteristics**:

* **Independent Subproblems**: Subproblems are solved independently and do not
  overlap.
* **Divide, Conquer, Combine**: The problem is divided into smaller subproblems,
* each subproblem is solved (conquered), and solutions are combined to form
  the final solution.
* **Efficient for Independent Subproblems**: Well-suited for problems like
  sorting (e.g., merge sort, quicksort) where the subproblems do not overlap.

**Limitations**:

* **Lack of Overlap Handling**: Does not handle overlapping subproblems, which
  can lead to inefficiencies if subproblems are not independent.

**Example**: Merge sort divides an array into halves, recursively sorts each
half, and then merges the sorted halves.

### Dynamic Programming

**Definition**:
Dynamic programming is an optimization technique that solves complex problems
by breaking them down into simpler subproblems, storing the results of these
subproblems to avoid redundancy, and constructing a final solution from these
cached results.

**Characteristics**:

* **Overlapping Subproblems**: Efficiently handles problems where subproblems
  overlap by storing computed results.
* **Memoization and Tabulation**: Uses either a top-down (memoization) or
  bottom-up (tabulation) approach to store intermediate results.
* **Optimal Substructure**: Suitable for problems where the optimal solution
  can be constructed from optimal subproblem solutions.

**Advantages**:

* **Efficiency**: Reduces time complexity by avoiding redundant
  calculations.
* **Memory Trade-off**: Uses additional memory to store subproblem solutions,
  which is a trade-off for time efficiency.

**Example**: The dynamic programming approach to calculating Fibonacci numbers
stores results in an array or table, resulting in linear time complexity.

### Comparison Summary

| Technique           | Subproblem Nature       | Solution Approach                  | Key Feature                        |
|---------------------|-------------------------|------------------------------------|------------------------------------|
| **Recursion**       | Repeated subproblems    | Direct self-calls                  | Simple but can be inefficient      |
| **Divide-and-Conquer** | Independent subproblems | Divide, conquer, and combine       | Solves independent subproblems     |
| **Dynamic Programming** | Overlapping subproblems | Memoization or tabulation          | Stores and reuses subproblem solutions |

## Part 3: Understanding Overlapping Subproblems and Optimal Substructure Properties

Dynamic programming is a powerful technique that leverages two key properties:
overlapping subproblems and optimal substructure. Understanding these
properties is crucial for effectively applying dynamic programming to solve
complex problems efficiently.

### Overlapping Subproblems

**Definition**:
A problem has overlapping subproblems if the same subproblems are solved
multiple times during the computation of the solution. Dynamic programming
takes advantage of this by storing the results of these subproblems to avoid
redundant calculations.

**Characteristics**:

* **Redundant Calculations**: Without optimization, recursive solutions may
  repeatedly compute the same subproblems, leading to inefficiency.
* **Memoization**: In a top-down approach, memoization involves storing the
  results of expensive function calls and returning the cached result when the
  same inputs occur again.
* **Tabulation**: In a bottom-up approach, tabulation involves filling out a
  table iteratively to avoid recursion altogether.

**Example**: Fibonacci Sequence

* **Problem**: Compute the n-th Fibonacci number, where each number is the sum
  of the two preceding ones.
* **Naive Recursive Approach**: Directly computing Fibonacci numbers
  recursively results in a tree of repeated calculations. For example,
  calculating F(5) involves calculating F(3) multiple times.
* **DP Approach**: Store previously computed Fibonacci numbers in an array, so
  each number is computed only once.

### Optimal Substructure

**Definition**:
A problem exhibits optimal substructure if the optimal solution to the problem
can be constructed from optimal solutions to its subproblems. This property
allows dynamic programming to build up solutions incrementally and efficiently.

**Characteristics**:

* **Recursive Definition**: Problems can be defined recursively in terms of
  smaller subproblems.
* **Combining Solutions**: The overall solution is obtained by combining
  optimal solutions of its subproblems.

**Example**: Shortest Path in a Graph

* **Problem**: Find the shortest path from a starting node to a destination
  node in a weighted graph.
* **Optimal Substructure**: If the shortest path from node A to node C passes
  through node B, then the path from A to B and the path from B to C must also
  be shortest paths.
* **Dynamic Programming Approach**: Algorithms like Dijkstra’s or Floyd-Warshall
  use this property to build up the shortest path solution.

### Identifying Problems Suitable for Dynamic Programming

1. **Check for Optimal Substructure**:
   * Formulate the problem recursively.
   * Determine if the optimal solution can be composed of optimal solutions of
     its subproblems.

2. **Check for Overlapping Subproblems**:
   * Identify if the problem involves solving the same subproblem multiple times.
   * Consider the potential for caching solutions to avoid redundant work.

3. **Design a Recursive Solution**:
   * Clearly define the recursive relation for the problem.
   * Identify base cases and ensure that recursive steps reduce the problem size.

4. **Implement Memoization or Tabulation**:
   * Use a data structure (like an array or hash map) to store results of subproblems.
   * Choose memoization (top-down) or tabulation (bottom-up) based on the
     problem context and personal preference.
