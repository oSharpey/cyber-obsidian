# Greedy programming
**Overview:**  
The greedy paradigm involves making the best local (or immediate) choice at each step with the hope that these locally optimal solutions will lead to a globally optimal solution. Greedy algorithms are often simpler and run faster than other approaches, but they do not always guarantee an optimal solution.
Greedy algorithms are algorithms that make the best possible choice at every local decision point.
Greedy algorithms are not used for efficiency, because typically, they’re not looking at every possible outcome, just the best outcome at each stage

Two properties must be true for a problem for a greedy algorithm to work:
- **Greedy Choice Property:** Means that the problem is so that the solution (the global optimum) can be reached by making greedy choices in each step (locally optimal choices).
- **Optimal Substructure:** Means that the optimal solution to a problem, is a collection of optimal solutions to sub-problems. So solving smaller parts of the problem locally (by making greedy choices) contributes to the overall solution.

**When It’s Useful:**
- **Optimal Solutions:**  
    Problems that exhibit the _greedy choice property_ and _optimal substructure._ Examples include activity selection, Huffman coding, and certain shortest-path problems in graphs (like using a greedy strategy in a weighted acyclic graph).
- **“Good-Enough” Solutions:**  
	Good for problems with a locally optimal choice that leads to a global optimum or a near-optimal solution; simple and fast but may fail in problems without the greedy property. - Prims
    The [0/1 Knapsack Problem](https://www.w3schools.com/dsa/dsa_ref_knapsack.php) cannot be solved by a greedy algorithm because it does not fulfill the greedy choice property, and the optimal substructure property, as mentioned earlier.


# Recursion (with divide and conquer)
Recursion is a method of solving a problem where the solution depends on solutions to smaller instances of the same problem. Divide and conquer is a specific form of recursion where a problem is divided into two or more subproblems, each solved independently, and then combined to form a solution to the original problem.
**When It’s Useful:**
- **Problems with a Natural Recursive Structure:**  
    Many problems, such as tree traversals, the Fibonacci sequence, and quicksort/mergesort, are naturally defined recursively.
- **Divide and Conquer Examples:**  
    Algorithms like merge sort (dividing the list into halves, sorting each half, and merging) and quicksort (dividing based on a pivot) use recursion to efficiently solve the problem.
# Dynamic Programming
Dynamic Programming is a method for designing algorithms.
An algorithm designed with Dynamic Programming divides the problem into subproblems, finds solutions to the subproblems, and puts them together to form a complete solution to the problem we want to solve.

To design an algorithm for a problem using Dynamic Programming, the problem we want to solve must have these two properties:
- **Overlapping Subproblems:** Means that the problem can be broken down into smaller subproblems, where the solutions to the subproblems are overlapping. Having subproblems that are overlapping means that the solution to one subproblem is part of the solution to another subproblem.
- **Optimal Substructure:** Means that the complete solution to a problem can be constructed from the solutions of its smaller subproblems. So not only must the problem have overlapping subproblems, the substructure must also be optimal so that there is a way to piece the solutions to the subproblems together to form the complete solution.


# Memoization



Memoization is a technique where results are stored to avoid doing the same computations many times.

When Memoization is used to improve recursive algorithms, it is called a "top-down" approach because of how it starts with the main problem and breaks it down into smaller subproblems.
Memoization is used in dynamic programming

**When It’s Useful:**
- **Overlapping Subproblems:** Whenever recursive calls compute the same values repeatedly, memoization ensures that once a subproblem is solved, the result is saved and simply returned on subsequent calls.
- **Improving Recursive Algorithms:** Algorithms that would otherwise be exponential in time—like naive Fibonacci or recursive solutions to combinatorial problems—can often be reduced to polynomial or even linear time with memoization.

## Using Memoization To Find The nth Fibonacci Number

The nth Fibonacci number can be found using recursion. Read more about how that is done on [this page](https://www.w3schools.com/dsa/dsa_algo_simple.php#nthfibo).
The problem with this implementation is that the number of computations and recursive calls "explodes" when trying to find a higher Fibonacci number, because the same computations are done over and over again.

But using memoization can help finding the n
th Fibonacci number using recursion much more effectively.
We use memoization by creating an array `memo` to hold the Fibonacci numbers, so that Fibonacci number `n` can be found as element `memo[n]`. And we only compute the Fibonacci number if it does not already exist in the `memo` array.

```python
def F(n):
    if memo[n] != None: # Already computed
        return memo[n]
    else: # Computation needed
        print('Computing F('+str(n)+')')
        if n <= 1:
            memo[n] = n
        else:
            memo[n] = F(n - 1) + F(n - 2)
        return memo[n] 

memo = [None]*7
print('F(6) = ',F(6))
print('memo = ',memo)
```