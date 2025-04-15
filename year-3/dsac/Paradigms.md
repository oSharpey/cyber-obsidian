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
    The [0/1 Knapsack Problem](https://www.w3schools.com/dsa/dsa_ref_knapsack.php) cannot be solved by a greedy algorithm because it does not fulfill the greedy choice property, and the optimal substructure property, as mentioned earlier.


# Recursion (with divide and conquor)

# Dynamic Programming

# Amortisation