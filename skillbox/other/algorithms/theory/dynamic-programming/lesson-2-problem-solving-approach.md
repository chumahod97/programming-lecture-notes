# Lesson 2: Problem Solving Approach

## Explanation of the Bottom-Up Approach

The bottom-up approach is a method used in dynamic programming to solve
problems by solving smaller subproblems first and using their solutions to
build up to the solution of the larger problem. This approach is often
implemented iteratively and involves filling out a table (usually an array)
from the smallest subproblem to the largest, ensuring that each subproblem is
solved only once. This is in contrast to the top-down approach, which often
involves recursion and memoization.

### Key Characteristics of the Bottom-Up Approach

1. **Iterative Process**: The bottom-up approach typically uses loops instead of recursion, which makes it efficient in terms of space, as it avoids the overhead of recursive function calls.

2. **Table Filling**: Solutions to subproblems are stored in a table. This table is filled from the smallest subproblem to the largest, ensuring that all dependencies are resolved before solving a larger problem.

3. **Space Complexity**: By using an iterative approach and a table, the bottom-up approach often has a lower space complexity compared to a recursive approach with memoization. The space complexity is generally determined by the size of the table.

4. **Avoids Redundant Calculations**: Each subproblem is solved once, and its result is stored in the table for future use, which avoids redundant calculations and improves efficiency.

### Steps to Implement the Bottom-Up Approach

1. **Identify Subproblems**: Break down the main problem into smaller subproblems. These should be problems that are simpler instances of the larger problem.

2. **Determine the Order of Solving Subproblems**: Decide the order in which subproblems should be solved. Typically, this involves solving the smallest subproblems first and using their results to solve larger subproblems.

3. **Define the Table Structure**: Create a table (usually a one-dimensional or two-dimensional array) that will store the solutions to subproblems.

4. **Fill the Table**: Use loops to fill the table, starting with the base cases (the smallest subproblems) and working your way up to the main problem.

5. **Extract the Solution**: Once the table is fully populated, the solution to the original problem can be found at the appropriate index.

### Example: Fibonacci Sequence

To illustrate the bottom-up approach, let's consider the problem of computing the Fibonacci sequence. The Fibonacci sequence is defined as follows:

- \( F(0) = 0 \)
- \( F(1) = 1 \)
- \( F(n) = F(n - 1) + F(n - 2) \) for \( n \geq 2 \)

**Bottom-Up Implementation**:

1. **Identify Subproblems**: Each Fibonacci number \( F(n) \) is dependent on the two preceding numbers \( F(n-1) \) and \( F(n-2) \).

2. **Determine the Order**: Compute \( F(0) \), then \( F(1) \), and continue up to \( F(n) \).

3. **Define the Table Structure**: Create an array `fib` where `fib[i]` will store the value of \( F(i) \).

4. **Fill the Table**:
   - Initialize the base cases: `fib[0] = 0`, `fib[1] = 1`.
   - Use a loop to fill the table for all \( i \) from 2 to \( n \): 
     \[
     \text{for } i = 2 \text{ to } n:
     \]
     \[
     \quad \text{fib}[i] = \text{fib}[i-1] + \text{fib}[i-2]
     \]

5. **Extract the Solution**: The solution to the problem, \( F(n) \), is stored in `fib[n]`.

By following these steps, we can efficiently compute the Fibonacci sequence up to the \( n \)-th number using the bottom-up approach without recalculating subproblems multiple times. This method is both time-efficient (as each subproblem is solved once) and space-efficient (especially if we optimize to use only two variables rather than an array).

## Explanation of the Top-Down Approach with Memoization

The top-down approach with memoization is another strategy used in dynamic programming to solve complex problems by breaking them down into simpler subproblems. This method involves recursively solving each subproblem and storing the results to avoid redundant calculations. Memoization refers to the technique of storing the results of expensive function calls and returning the cached result when the same inputs occur again.

### Key Characteristics of the Top-Down Approach with Memoization

1. **Recursive Process**: The top-down approach is typically implemented using recursion. The problem is solved by recursively breaking it down into smaller subproblems.

2. **Memoization**: Memoization is used to store the results of subproblems in a data structure, usually a hash table or an array, to prevent redundant computations. This significantly optimizes the performance by ensuring each subproblem is solved only once.

3. **Space Complexity**: While recursion can lead to higher space usage due to the call stack, memoization helps reduce time complexity by eliminating redundant calculations. The space complexity is generally determined by the size of the memoization table.

4. **Ease of Implementation**: For many, the top-down approach can be more intuitive to implement, especially for problems naturally expressed recursively.

### Steps to Implement the Top-Down Approach with Memoization

1. **Define the Recursive Function**: Write a recursive function that breaks the problem into smaller subproblems.

2. **Identify Base Cases**: Determine the simplest instances of the problem, which will stop further recursion.

3. **Use Memoization**: Store the results of each subproblem in a data structure (like a dictionary or an array) when first computed. Check this structure before performing any computation to see if the result is already available.

4. **Recursive Calls**: Make recursive calls only if the results are not already in the memoization table.

5. **Return the Solution**: The recursive function should return the solution to the initial problem once all necessary subproblems have been solved.

### Example: Fibonacci Sequence

Let's examine how the top-down approach with memoization can be applied to the Fibonacci sequence:

The Fibonacci sequence is defined as follows:

- \( F(0) = 0 \)
- \( F(1) = 1 \)
- \( F(n) = F(n - 1) + F(n - 2) \) for \( n \geq 2 \)

**Top-Down Implementation**:

1. **Define the Recursive Function**: Create a function `fib(n)` that calculates \( F(n) \).

2. **Identify Base Cases**:
   ```python
   def fib(n):
       if n == 0:
           return 0
       if n == 1:
           return 1
   ```

3. **Use Memoization**: Use a dictionary or list to store Fibonacci numbers as they are computed.
   ```python
   memo = {}
   ```

4. **Recursive Calls with Memoization**:
   ```python
   def fib(n):
       if n in memo:
           return memo[n]
       if n == 0:
           return 0
       if n == 1:
           return 1
       memo[n] = fib(n - 1) + fib(n - 2)
       return memo[n]
   ```

5. **Return the Solution**: Call `fib(n)` to get \( F(n) \).

This approach ensures that each Fibonacci number is calculated only once, with results stored in `memo` to be reused whenever needed. By avoiding repeated calculations, the top-down approach with memoization optimizes the time complexity to \( O(n) \), similar to the bottom-up approach, while allowing for a more straightforward recursive implementation.

## Introduction to State Definition, State Transition, and Base Cases

In dynamic programming, solving a problem efficiently involves breaking it down into subproblems that are easier to handle. Key to this approach are three foundational concepts: state definition, state transition, and base cases. Understanding these concepts allows developers to model problems in a way that enables dynamic programming solutions.

### State Definition

State definition involves identifying the parameters that uniquely define each subproblem. A "state" typically represents a snapshot of the problem at a certain stage in the process of finding a solution. By defining a state, you determine what information is necessary to describe a subproblem and compute its solution.

- **Components of a State**: States are often defined by one or more variables that capture the essential aspects of the problem. For example, in the Fibonacci sequence, the state can be defined as the index \( n \), representing the Fibonacci number \( F(n) \) you want to compute.

- **Granularity**: The state should be defined at the right level of granularity. It should be detailed enough to capture the essential distinctions between subproblems but not so detailed that it becomes inefficient or redundant.

### State Transition

State transition describes how to move from one state to another and how the solution of one state can be derived from the solutions of other states. It involves formulating a recursive relationship that can be used to compute the solution of the problem.

- **Recurrence Relation**: The transition between states is often expressed as a recurrence relation. This relation is a mathematical formula that expresses the solution of a state in terms of the solutions of one or more smaller states. For example, in the Fibonacci sequence, the transition is given by:
  \[
  F(n) = F(n - 1) + F(n - 2)
  \]

- **Dependencies**: Understanding how states depend on each other is crucial for ensuring that subproblems are solved in the correct order, whether using a top-down or bottom-up approach.

### Base Cases

Base cases are the simplest instances of the problem that can be solved directly without further decomposition. They serve as the stopping condition for recursion (in a top-down approach) or as the starting point for iteration (in a bottom-up approach).

- **Initial Conditions**: Base cases provide the initial values needed to compute other states through state transitions. For example, in the Fibonacci sequence, the base cases are:
  \[
  F(0) = 0, \quad F(1) = 1
  \]

- **Termination**: Properly defining base cases ensures that recursion terminates, or in the context of a bottom-up approach, it ensures that the iterative process has a valid starting point.

### Example: Fibonacci Sequence

To see these concepts in action, let's revisit the Fibonacci sequence problem:

- **State Definition**: The state is defined by a single integer \( n \), representing the Fibonacci number \( F(n) \).

- **State Transition**: The transition is expressed by the recurrence relation:
  \[
  F(n) = F(n - 1) + F(n - 2)
  \]
  This means the solution for \( F(n) \) depends on the solutions of \( F(n-1) \) and \( F(n-2) \).

- **Base Cases**: The base cases are:
  \[
  F(0) = 0, \quad F(1) = 1
  \]
  These provide the initial values needed to compute higher Fibonacci numbers.

By clearly defining states, understanding how to transition between them, and setting up base cases, you can effectively apply dynamic programming techniques to solve complex problems efficiently. This structured approach allows dynamic programming to break down and systematically solve even the most challenging computational problems.

## Example Problem Walkthrough: Fibonacci Sequence

In this section, we'll walk through solving the Fibonacci sequence problem using both the top-down approach with memoization and the bottom-up approach. The Fibonacci sequence is a classic problem that provides a great introduction to dynamic programming techniques. 

### Problem Definition

The Fibonacci sequence is defined as follows:
- \( F(0) = 0 \)
- \( F(1) = 1 \)
- \( F(n) = F(n-1) + F(n-2) \) for \( n \geq 2 \)

Our goal is to compute \( F(n) \) for a given \( n \).

### Top-Down Approach with Memoization

The top-down approach involves solving the problem recursively and storing the results of subproblems to avoid redundant calculations. This is where memoization comes into play.

#### Steps:

1. **State Definition**: Define \( F(n) \) as the nth Fibonacci number.

2. **State Transition**: Use the recurrence relation:
   \[
   F(n) = F(n-1) + F(n-2)
   \]

3. **Base Cases**: 
   - \( F(0) = 0 \)
   - \( F(1) = 1 \)

4. **Memoization**: Create an array (or hash map) to store results of previously computed Fibonacci numbers.

#### Implementation:

Here's a Python implementation of the top-down approach with memoization:

```python
def fibonacci_top_down(n, memo={}):
    if n in memo:
        return memo[n]
    if n == 0:
        return 0
    if n == 1:
        return 1
    memo[n] = fibonacci_top_down(n-1, memo) + fibonacci_top_down(n-2, memo)
    return memo[n]

# Example usage
n = 10
print(f"Fibonacci({n}) = {fibonacci_top_down(n)}")
```

### Bottom-Up Approach

The bottom-up approach involves iteratively solving smaller subproblems and building up to the solution of the original problem. This approach usually saves space and avoids the overhead of recursive calls.

#### Steps:

1. **State Definition**: Define \( F(n) \) as the nth Fibonacci number.

2. **State Transition**: Use the same recurrence relation:
   \[
   F(n) = F(n-1) + F(n-2)
   \]

3. **Base Cases**: 
   - Initialize \( F(0) = 0 \)
   - Initialize \( F(1) = 1 \)

4. **Iterative Computation**: Start from the base cases and iteratively compute up to \( F(n) \).

#### Implementation:

Here's a Python implementation of the bottom-up approach:

```python
def fibonacci_bottom_up(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    
    fib = [0] * (n + 1)
    fib[0] = 0
    fib[1] = 1
    
    for i in range(2, n + 1):
        fib[i] = fib[i - 1] + fib[i - 2]
    
    return fib[n]

# Example usage
n = 10
print(f"Fibonacci({n}) = {fibonacci_bottom_up(n)}")
```

### Comparison

- **Top-Down Approach**: This is intuitive and easy to implement with recursion. The use of memoization helps optimize the recursive calls, making it efficient in terms of time complexity (\(O(n)\)) but can be costly in terms of space due to the recursion stack.
- **Bottom-Up Approach**: This approach is often more space-efficient because it only requires storing a few previous results at a time. It also avoids the overhead of recursive calls by using iteration.

By understanding these two approaches to solving the Fibonacci sequence, you gain insight into how dynamic programming can efficiently solve problems that involve overlapping subproblems and optimal substructure.
