# Lesson 3: Classic Problems in Dynamic Programming

## Dynamic Programming: Knapsack Problem

Dynamic programming is a powerful technique used to solve complex problems by breaking them down into simpler subproblems. One of the classic problems that can be efficiently solved using dynamic programming is the Knapsack Problem. In this section, we'll explore the 0/1 Knapsack Problem, which is a fundamental problem in computer science and operations research.

### Problem Description

The 0/1 Knapsack Problem is defined as follows:

- **Input**: You are given a set of items, each with a weight and a value. You are also given a maximum weight capacity for a knapsack.
- **Objective**: Determine the maximum value that can be obtained by selecting a subset of the items such that the total weight does not exceed the knapsack's capacity. Each item can either be included in the knapsack or not (hence the name 0/1).

### Example

Suppose you have the following items:

| Item | Weight | Value |
|------|--------|-------|
| 1    | 2      | 3     |
| 2    | 3      | 4     |
| 3    | 4      | 5     |
| 4    | 5      | 6     |

And the maximum weight capacity of the knapsack is 5. The goal is to find the maximum value possible under this weight constraint.

### Dynamic Programming Approach

The dynamic programming solution involves creating a table to store the results of subproblems. Let's define `dp[i][w]` as the maximum value that can be obtained with the first `i` items and a maximum weight capacity `w`.

#### Steps to Solve the Problem

1. **Initialize the Table**: Create a table `dp` with dimensions `(n+1) x (W+1)`, where `n` is the number of items and `W` is the maximum weight capacity of the knapsack. Initialize `dp[0][w] = 0` for all `w` because with 0 items, the maximum value is 0.

2. **Fill the Table**: Use a nested loop to fill the table:
   - For each item `i` from 1 to `n`:
     - For each weight capacity `w` from 0 to `W`:
       - If the weight of the current item is greater than `w`, then `dp[i][w] = dp[i-1][w]` (the item cannot be included).
       - Otherwise, `dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i-1]] + value[i-1])`. This decision represents whether to include the current item or not.

3. **Decision-Making Process**: The decision at each step is whether to include the current item in the knapsack. This decision is based on maximizing the total value:
   - **Include the item**: Add the item's value to the result of the subproblem that excludes the item's weight.
   - **Exclude the item**: Simply take the result of the subproblem without the current item.

#### Example Table Filling

Using the example items and a capacity of 5, the table `dp` would be filled as follows:

| i\w | 0 | 1 | 2 | 3 | 4 | 5 |
|-----|---|---|---|---|---|---|
| 0   | 0 | 0 | 0 | 0 | 0 | 0 |
| 1   | 0 | 0 | 3 | 3 | 3 | 3 |
| 2   | 0 | 0 | 3 | 4 | 4 | 7 |
| 3   | 0 | 0 | 3 | 4 | 5 | 7 |
| 4   | 0 | 0 | 3 | 4 | 5 | 7 |

The maximum value that can be obtained with a capacity of 5 is found at `dp[4][5]`, which is 7.

### Summary of Key Concepts

- **Subproblem Identification**: Break the problem into smaller subproblems, each representing a decision about whether to include an item.
- **Optimal Substructure**: Use the optimal solutions of subproblems to construct the solution to the original problem.
- **Table Filling**: Systematically fill in the table based on decisions made at each step, ensuring that previous computations are reused.

By understanding and applying these concepts, you can efficiently solve the Knapsack Problem using dynamic programming. This method not only provides a solution but also helps in understanding the decision-making process that leads to an optimal result.

## Coin Change Problem

### Introduction

The Coin Change Problem is a classic dynamic programming problem that involves finding the minimum number of coins needed to make a certain amount of money, given a set of coin denominations. This problem is a practical illustration of how dynamic programming can be used to break down complex problems into simpler subproblems, making it a valuable tool for developers.

### Problem Statement

Given a set of coin denominations `coins = [c1, c2, ..., cn]` and a total amount `amount`, the goal is to determine the minimum number of coins required to make up that amount. If it's not possible to make the amount with the given coins, return -1.

#### Example

For `coins = [1, 2, 5]` and `amount = 11`, the minimum number of coins is 3 (11 = 5 + 5 + 1).

### Dynamic Programming Approach

#### Step-by-Step Explanation

1. **Define the State:**
   - Let `dp[i]` represent the minimum number of coins required to make the amount `i`.

2. **Base Case:**
   - `dp[0] = 0`: No coins are needed to make the amount 0.

3. **State Transition:**
   - For each amount `i` from 1 to `amount`, compute `dp[i]` using the formula:
     \[
     dp[i] = \min(dp[i - c] + 1) \quad \text{for each coin } c \text{ in } coins \text{ where } i - c \geq 0
     \]
   - This formula implies that for each coin, if subtracting the coin's value from `i` gives a non-negative result, we check the solution for `i - c` and add 1 (for using one more coin). We take the minimum of these possible values.

4. **Decision-Making Process:**
   - At each step, decide whether to use a particular coin by checking if it leads to a solution with fewer coins than previously found solutions.

5. **Result:**
   - The final result, `dp[amount]`, will give the minimum number of coins needed for the `amount`. If `dp[amount]` is still infinity (or a large number you've initialized with), it means it's impossible to form the amount with the given coins.

#### Implementation

Here's a Python implementation to illustrate the above dynamic programming approach:

```python
def coin_change(coins, amount):
    # Initialize dp array with infinity for all values except dp[0]
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    # Fill the dp table
    for i in range(1, amount + 1):
        for coin in coins:
            if i - coin >= 0:  # If the coin can be used
                dp[i] = min(dp[i], dp[i - coin] + 1)

    # Return the result
    return dp[amount] if dp[amount] != float('inf') else -1

# Example usage:
coins = [1, 2, 5]
amount = 11
print(coin_change(coins, amount))  # Output: 3
```

#### Table Filling

The table `dp` is filled iteratively for each amount from 1 to `amount`. This process ensures that each subproblem is solved before it is used in solving larger problems, which is the essence of dynamic programming. By filling the table iteratively, we build up the solution from the simplest case (`amount = 0`) to the desired target (`amount`).

#### Decision Making

At each step, the decision to include a coin is made by checking if using that coin results in a fewer number of total coins than the current known minimum for that amount. This decision-making process involves comparing the current value in `dp[i]` with `dp[i - coin] + 1` for each coin, ensuring that we always choose the minimum.

## Longest Common Subsequence

### Introduction

The Longest Common Subsequence (LCS) problem is a classic problem in computer science and bioinformatics. Given two sequences, the goal is to find the longest subsequence present in both of them. A subsequence is a sequence derived from another sequence by deleting some elements without changing the order of the remaining elements.

### Problem Definition

Given two sequences, `X` and `Y`, the task is to find the length of the longest subsequence that appears in both sequences.

For example, consider the sequences:
- `X = "ABCBDAB"`
- `Y = "BDCAB"`

The LCS is `"BCAB"`, with a length of `4`.

### Dynamic Programming Approach

Dynamic programming is an optimization technique that solves problems by breaking them down into simpler subproblems. For the LCS problem, we'll use a table to store solutions to subproblems and build up to the solution of the entire problem.

#### Step-by-Step Explanation

1. **Define the Subproblems:**

   The subproblem for the LCS involves finding the LCS of prefixes of `X` and `Y`. Let's define `dp[i][j]` as the length of LCS of the first `i` characters of `X` and the first `j` characters of `Y`.

2. **Formulate the Recurrence Relation:**

   - If `X[i-1] == Y[j-1]`, then the character is part of the LCS. Thus, `dp[i][j] = dp[i-1][j-1] + 1`.
   - If `X[i-1] != Y[j-1]`, the LCS could be obtained by either:
     - Ignoring the current character of `X`: `dp[i][j] = dp[i-1][j]`
     - Ignoring the current character of `Y`: `dp[i][j] = dp[i][j-1]`
   - Therefore, the recurrence relation is:
     \[
     dp[i][j] = 
     \begin{cases} 
     dp[i-1][j-1] + 1 & \text{if } X[i-1] == Y[j-1] \\
     \max(dp[i-1][j], dp[i][j-1]) & \text{if } X[i-1] \neq Y[j-1]
     \end{cases}
     \]

3. **Initialize the Base Cases:**

   - If either string is empty, the LCS is `0`. Thus, `dp[i][0] = 0` for all `i` and `dp[0][j] = 0` for all `j`.

4. **Fill the Table:**

   - Construct a `dp` table where `dp[i][j]` represents the length of the LCS of the first `i` elements of `X` and `j` elements of `Y`.
   - Iteratively calculate the values of the `dp` table using the recurrence relation.

5. **Extract the Result:**

   - Once the table is filled, the length of the LCS of the entire sequences `X` and `Y` will be found in `dp[m][n]`, where `m` is the length of `X` and `n` is the length of `Y`.

#### Example

Let's fill a table for `X = "ABCBDAB"` and `Y = "BDCAB"`:

|    | 0 | B | D | C | A | B |
|----|---|---|---|---|---|---|
| 0  | 0 | 0 | 0 | 0 | 0 | 0 |
| A  | 0 | 0 | 0 | 0 | 1 | 1 |
| B  | 0 | 1 | 1 | 1 | 1 | 2 |
| C  | 0 | 1 | 1 | 2 | 2 | 2 |
| B  | 0 | 1 | 1 | 2 | 2 | 3 |
| D  | 0 | 1 | 2 | 2 | 2 | 3 |
| A  | 0 | 1 | 2 | 2 | 3 | 3 |
| B  | 0 | 1 | 2 | 2 | 3 | 4 |

The length of the LCS is `4`, which is stored in `dp[7][5]`.

### Decision-Making Process

While filling the table, the decision at each cell `dp[i][j]` depends on whether the characters `X[i-1]` and `Y[j-1]` are the same. If they are, the value is incremented from `dp[i-1][j-1]`. If not, the maximum of the two possibilities (ignoring one character from either string) is taken. This decision-making process is fundamental to understanding how dynamic programming optimizes the solution by reusing previously computed results.

## Explanation of Table Filling and Decision-Making Process in Dynamic Programming

Dynamic programming (DP) is a powerful technique used to solve problems by breaking them down into simpler subproblems. The key idea is to solve each subproblem just once and store its solution, usually in a table, thus avoiding the need to recompute it every time it is needed. This part of the lesson will focus on explaining how to fill tables and make decisions when implementing dynamic programming solutions.

### Key Concepts of Table Filling

1. **Subproblem Definition**: 
   - Identify the smaller problems that can be solved independently. These subproblems should be defined in such a way that they can be reused to construct the solution to the original problem.
   - For example, in the Knapsack Problem, a subproblem can be defined as the maximum value obtainable with a given weight limit and a subset of items.

2. **Table Structure**:
   - Choose a data structure (usually a 1D or 2D array) to store the solution to each subproblem.
   - The size of this table is typically determined by the constraints of the problem, such as the number of items and the total capacity in the Knapsack Problem.

3. **Base Cases**:
   - Initialize the table with base cases that are trivial to solve.
   - For instance, in the Coin Change Problem, if the amount is 0, the number of coins required is 0.

4. **Recursive Relation**:
   - Develop a recursive formula that relates the solution of a subproblem to the solutions of its smaller subproblems.
   - For example, in the Longest Common Subsequence (LCS) problem, the length of the LCS of two strings can be derived from the LCS of their prefixes.

### Steps in Table Filling

1. **Initialize the Table**:
   - Start by filling in the base cases. These are the simplest form of the problem with known solutions.
   - For example, if you are solving the Knapsack Problem, initialize the table with zeros for all cases where the weight capacity is zero.

2. **Iterate Over Subproblems**:
   - Use nested loops to fill out the table, iterating through all possible states of the problem defined by your table dimensions.
   - For the Coin Change Problem, iterate over each coin and each sub-amount to fill out the table.

3. **Apply the Recursive Formula**:
   - For each state, use the recursive relationship to fill in the table.
   - In the LCS problem, if characters match, update the table using the value of the previous diagonal cell plus one; otherwise, take the maximum of the adjacent cells.

4. **Decision-Making Process**:
   - At each step, make decisions based on the recursive relation. This often involves choosing between two or more options.
   - For example, in the Knapsack Problem, decide whether to include an item based on whether it increases the total value without exceeding the capacity.

5. **Construct the Solution**:
   - Once the table is filled, use it to construct the solution to the original problem.
   - For instance, in the LCS problem, backtrack through the table to find the actual longest common subsequence.

### Example Walkthrough: Coin Change Problem

Let's work through the Coin Change Problem to solidify these concepts:

- **Problem Statement**: Given a set of coin denominations and a total amount, find the minimum number of coins needed to make the amount.
  
- **Subproblem Definition**: Let `dp[i]` represent the minimum number of coins needed to make the amount `i`.

- **Base Case**: `dp[0] = 0` because no coins are needed to make the amount 0.

- **Recursive Formula**: 
  - For each coin `c` and amount `i`, if `i >= c`, then `dp[i] = min(dp[i], dp[i - c] + 1)`.

- **Table Initialization**:
  - Initialize `dp` with a large number (infinity) except `dp[0]`.

- **Iterative Process**:
  - For each coin, iterate through each amount from the coin value to the total amount, updating `dp[i]` using the recursive formula.

This process efficiently constructs the solution by considering each subproblem and reusing previously computed results, embodying the essence of dynamic programming.
