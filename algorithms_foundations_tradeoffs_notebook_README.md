# Guide: Algorithms Foundations and Trade-Offs

This guide explains how to reason through `algorithms_foundations_tradeoffs_notebook.ipynb`.

## Main Goal

The point is not memorizing code. The point is knowing:
- what algorithm class fits the problem
- what runtime it usually has
- what memory it usually costs
- what assumptions it requires
- where it breaks down

## Traversals

### Array traversal

Question:
- scan an array for a target

Resolution:
1. Check each element once.
2. Return index on match.
3. Return failure marker if absent.

Key lesson:
- linear scan is simple and often unavoidable on unsorted data

### Tree traversal

Question:
- implement preorder, inorder, and level-order traversal

Resolution:
1. Preorder: root, left, right
2. Inorder: left, root, right
3. Level order: queue by breadth

Key lesson:
- same structure, different traversal order, different use cases

## BFS vs DFS

### BFS

Resolution pattern:
1. Initialize queue with start node.
2. Track visited nodes.
3. Pop from queue, push neighbors.

Use BFS when:
- shortest path in unweighted graph matters
- level-by-level exploration matters

Trade-off:
- can use a lot of memory on wide graphs

### DFS

Resolution pattern:
1. Initialize stack or recursion with start node.
2. Track visited nodes.
3. Explore one branch deeply before siblings.

Use DFS when:
- recursive structure is natural
- backtracking or component discovery matters

Trade-off:
- no shortest path guarantee
- recursion depth can be risky

## Divide and Conquer

### Binary search

Resolution:
1. Check midpoint.
2. Throw away half the search space.
3. Repeat until found or exhausted.

Key assumption:
- sorted input

Key lesson:
- if input is unsorted, binary search logic is invalid

### Merge sort

Resolution:
1. Split array into halves.
2. Recursively sort halves.
3. Merge sorted halves.

Key lesson:
- predictable `O(n log n)` runtime
- extra memory is the cost

## Prompt-Based Reasoning

### Prompt A: shortest hops in social graph

Best fit:
- BFS

Why:
- unweighted shortest path problem

### Prompt B: search sorted millions-row array

Best fit:
- binary search

Why:
- sorted data makes divide-and-conquer valid

### Prompt C: process reachable file dependencies

Best fit:
- BFS or DFS with visited set

Why:
- dependency graph traversal

What to mention:
- recursion risk
- cycle handling

## What Strong Answers Include

- correct algorithm class
- correct runtime order, not exact constant factors
- correct memory discussion
- a clear note on assumptions
- a clear note on trade-offs
