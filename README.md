# LeetCode / HackerRank DSA Cheat Sheet

A practical, pattern-first reference for solving Data Structures & Algorithms problems on
LeetCode and HackerRank. Every pattern includes: **when to use it**, a **Python template**,
**complexity**, and a short list of **classic problems** to drill.

> Tip: most interview/contest problems are a recognizable pattern in disguise. Learn to
> spot the signal (see "Recognize it by" in each section) instead of memorizing solutions.

## Table of Contents

- [Big-O Quick Reference](#big-o-quick-reference)
- [Data Structure Complexity Cheat Sheet](#data-structure-complexity-cheat-sheet)
- [Patterns](#patterns)
  1. [Two Pointers](#1-two-pointers)
  2. [Sliding Window](#2-sliding-window)
  3. [Fast & Slow Pointers](#3-fast--slow-pointers)
  4. [Binary Search](#4-binary-search)
  5. [Prefix Sum](#5-prefix-sum)
  6. [Hashing / Hash Map](#6-hashing--hash-map)
  7. [Stack / Monotonic Stack](#7-stack--monotonic-stack)
  8. [Linked List In-Place Reversal](#8-linked-list-in-place-reversal)
  9. [Trees: DFS / BFS](#9-trees-dfs--bfs)
  10. [Binary Search Tree](#10-binary-search-tree)
  11. [Graphs: BFS / DFS](#11-graphs-bfs--dfs)
  12. [Topological Sort](#12-topological-sort)
  13. [Union-Find (Disjoint Set)](#13-union-find-disjoint-set)
  14. [Heap / Priority Queue](#14-heap--priority-queue)
  15. [Backtracking](#15-backtracking)
  16. [Dynamic Programming](#16-dynamic-programming)
  17. [Greedy](#17-greedy)
  18. [Intervals](#18-intervals)
  19. [Trie](#19-trie)
  20. [Bit Manipulation](#20-bit-manipulation)
- [Curated Problem List by Pattern](#curated-problem-list-by-pattern)
- [How to Practice](#how-to-practice)

---

## Big-O Quick Reference

| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Hash map lookup, array index |
| O(log n) | Logarithmic | Binary search, balanced BST ops |
| O(n) | Linear | Single pass scan |
| O(n log n) | Linearithmic | Sorting, merge sort, heap sort |
| O(n²) | Quadratic | Nested loops, brute-force pairs |
| O(2ⁿ) | Exponential | Subsets, brute-force backtracking |
| O(n!) | Factorial | Permutations |

**Rule of thumb for input size → allowed complexity (typical judge limits ~1-2s):**

| n | Acceptable complexity |
|---|---|
| ≤ 10 | O(n!), O(2ⁿ · n) |
| ≤ 20-25 | O(2ⁿ) |
| ≤ 500 | O(n³) |
| ≤ 5,000 | O(n²) |
| ≤ 10⁶ | O(n log n) |
| ≤ 10⁸ | O(n) |

---

## Data Structure Complexity Cheat Sheet

| Structure | Access | Search | Insert | Delete | Notes |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | Insert/delete at end amortized O(1) |
| Dynamic Array (list) | O(1) | O(n) | O(1)* | O(n) | *amortized, end only |
| Linked List | O(n) | O(n) | O(1) | O(1) | O(1) insert/delete given a node ref |
| Hash Map / Set | - | O(1)* | O(1)* | O(1)* | *avg case; O(n) worst case |
| Binary Search Tree (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | e.g. `TreeMap`, `sortedcontainers` |
| Heap (binary) | O(1) top | O(n) | O(log n) | O(log n) | peek min/max O(1) |
| Trie | - | O(L) | O(L) | O(L) | L = key length |
| Union-Find | - | O(α(n)) | O(α(n)) | - | with path compression + union by rank |

---

## Patterns

### 1. Two Pointers
**Recognize it by:** sorted array, pair/triplet sum, palindrome check, in-place partition.

```python
def two_sum_sorted(nums: list[int], target: int) -> list[int]:
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        s = nums[lo] + nums[hi]
        if s == target:
            return [lo, hi]
        elif s < target:
            lo += 1
        else:
            hi -= 1
    return [-1, -1]
```
**Complexity:** O(n) time, O(1) space.
**Classics:** Two Sum II, 3Sum, Container With Most Water, Valid Palindrome, Trapping Rain Water.

### 2. Sliding Window
**Recognize it by:** contiguous subarray/substring, "longest/shortest/max/min" + constraint.

```python
def longest_substring_no_repeat(s: str) -> int:
    seen = {}
    left = best = 0
    for right, ch in enumerate(s):
        if ch in seen and seen[ch] >= left:
            left = seen[ch] + 1
        seen[ch] = right
        best = max(best, right - left + 1)
    return best
```
**Complexity:** O(n) time, O(k) space (k = window alphabet/size).
**Classics:** Longest Substring Without Repeating Characters, Minimum Window Substring,
Longest Repeating Character Replacement, Sliding Window Maximum, Max Sum Subarray of Size K.

### 3. Fast & Slow Pointers
**Recognize it by:** cycle detection, middle of linked list, happy number.

```python
def has_cycle(head) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow is fast:
            return True
    return False
```
**Complexity:** O(n) time, O(1) space.
**Classics:** Linked List Cycle I/II, Middle of the Linked List, Happy Number, Find the Duplicate Number.

### 4. Binary Search
**Recognize it by:** sorted array, "find minimum X such that condition holds" (search on answer).

```python
def binary_search(nums: list[int], target: int) -> int:
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1

# Search on answer template (find smallest x satisfying predicate(x))
def search_on_answer(lo: int, hi: int, predicate) -> int:
    while lo < hi:
        mid = (lo + hi) // 2
        if predicate(mid):
            hi = mid
        else:
            lo = mid + 1
    return lo
```
**Complexity:** O(log n) time, O(1) space.
**Classics:** Binary Search, Search in Rotated Sorted Array, Find First/Last Position,
Koko Eating Bananas, Split Array Largest Sum, Median of Two Sorted Arrays.

### 5. Prefix Sum
**Recognize it by:** range sum queries, subarray sum equals K.

```python
def subarray_sum_equals_k(nums: list[int], k: int) -> int:
    count, running = 0, 0
    seen = {0: 1}
    for x in nums:
        running += x
        count += seen.get(running - k, 0)
        seen[running] = seen.get(running, 0) + 1
    return count
```
**Complexity:** O(n) time, O(n) space.
**Classics:** Subarray Sum Equals K, Range Sum Query - Immutable, Product of Array Except Self,
Continuous Subarray Sum.

### 6. Hashing / Hash Map
**Recognize it by:** need O(1) lookup, counting frequencies, detecting duplicates/anagrams.

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    index = {}
    for i, x in enumerate(nums):
        if target - x in index:
            return [index[target - x], i]
        index[x] = i
    return []
```
**Complexity:** O(n) time, O(n) space.
**Classics:** Two Sum, Group Anagrams, Valid Anagram, Longest Consecutive Sequence, Top K Frequent Elements.

### 7. Stack / Monotonic Stack
**Recognize it by:** matching brackets, "next greater/smaller element", histogram/area problems.

```python
def next_greater_element(nums: list[int]) -> list[int]:
    result = [-1] * len(nums)
    stack = []  # indices, values decreasing
    for i, x in enumerate(nums):
        while stack and nums[stack[-1]] < x:
            result[stack.pop()] = x
        stack.append(i)
    return result
```
**Complexity:** O(n) time (each element pushed/popped once), O(n) space.
**Classics:** Valid Parentheses, Daily Temperatures, Largest Rectangle in Histogram,
Next Greater Element I/II, Min Stack.

### 8. Linked List In-Place Reversal
**Recognize it by:** reverse a list/sublist without extra space.

```python
def reverse_list(head):
    prev = None
    while head:
        nxt = head.next
        head.next = prev
        prev = head
        head = nxt
    return prev
```
**Complexity:** O(n) time, O(1) space.
**Classics:** Reverse Linked List, Reverse Linked List II, Reverse Nodes in k-Group,
Swap Nodes in Pairs, Palindrome Linked List.

### 9. Trees: DFS / BFS
**Recognize it by:** tree traversal, path sum, level-order output, tree depth/diameter.

```python
def inorder(root) -> list[int]:
    result, stack = [], []
    node = root
    while node or stack:
        while node:
            stack.append(node)
            node = node.left
        node = stack.pop()
        result.append(node.val)
        node = node.right
    return result

from collections import deque

def level_order(root) -> list[list[int]]:
    if not root:
        return []
    result, queue = [], deque([root])
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        result.append(level)
    return result
```
**Complexity:** O(n) time, O(h) DFS space (h = height) / O(n) BFS space.
**Classics:** Binary Tree Level Order Traversal, Maximum Depth of Binary Tree, Path Sum,
Diameter of Binary Tree, Lowest Common Ancestor, Serialize/Deserialize Binary Tree.

### 10. Binary Search Tree
**Recognize it by:** "BST" explicitly mentioned, validate/insert/delete keeping sorted order.

```python
def is_valid_bst(root, low=float('-inf'), high=float('inf')) -> bool:
    if not root:
        return True
    if not (low < root.val < high):
        return False
    return (is_valid_bst(root.left, low, root.val) and
            is_valid_bst(root.right, root.val, high))
```
**Complexity:** O(n) time, O(h) space.
**Classics:** Validate BST, Kth Smallest Element in a BST, Insert into a BST,
Convert Sorted Array to BST, Lowest Common Ancestor of a BST.

### 11. Graphs: BFS / DFS
**Recognize it by:** grid traversal, connected components, shortest path in unweighted graph.

```python
from collections import deque

def bfs_shortest_path(graph: dict, start, target) -> int:
    visited = {start}
    queue = deque([(start, 0)])
    while queue:
        node, dist = queue.popleft()
        if node == target:
            return dist
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, dist + 1))
    return -1

def dfs(graph: dict, start, visited=None) -> set:
    visited = visited or set()
    visited.add(start)
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited
```
**Complexity:** O(V + E) time, O(V) space.
**Classics:** Number of Islands, Clone Graph, Course Schedule, Word Ladder, Rotting Oranges,
Pacific Atlantic Water Flow, Flood Fill.

### 12. Topological Sort
**Recognize it by:** task scheduling with dependencies, "prerequisite" wording, DAG ordering.

```python
from collections import deque

def topo_sort(num_nodes: int, edges: list[tuple[int, int]]) -> list[int]:
    graph = {i: [] for i in range(num_nodes)}
    indegree = [0] * num_nodes
    for u, v in edges:  # u -> v
        graph[u].append(v)
        indegree[v] += 1
    queue = deque([n for n in range(num_nodes) if indegree[n] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)
    return order if len(order) == num_nodes else []  # [] => cycle
```
**Complexity:** O(V + E) time, O(V) space.
**Classics:** Course Schedule I/II, Alien Dictionary, Sequence Reconstruction.

### 13. Union-Find (Disjoint Set)
**Recognize it by:** connectivity queries, grouping, detecting cycles in undirected graphs, "number of provinces".

```python
class DSU:
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x: int) -> int:
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, a: int, b: int) -> bool:
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.rank[ra] < self.rank[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        if self.rank[ra] == self.rank[rb]:
            self.rank[ra] += 1
        return True
```
**Complexity:** ~O(α(n)) per op (near constant), O(n) space.
**Classics:** Number of Provinces, Redundant Connection, Accounts Merge, Number of Islands II.

### 14. Heap / Priority Queue
**Recognize it by:** "top K", "kth largest/smallest", merge K sorted lists, scheduling.

```python
import heapq

def k_largest(nums: list[int], k: int) -> list[int]:
    return heapq.nlargest(k, nums)

def kth_largest_stream(k: int, nums: list[int]):
    heap = nums[:k]
    heapq.heapify(heap)
    for x in nums[k:]:
        if x > heap[0]:
            heapq.heapreplace(heap, x)
    return heap[0]
```
**Complexity:** O(n log k) time for top-K, O(log n) per push/pop.
**Classics:** Kth Largest Element, Top K Frequent Elements, Merge K Sorted Lists,
Find Median from Data Stream, Task Scheduler.

### 15. Backtracking
**Recognize it by:** "generate all", permutations/combinations/subsets, constraint satisfaction (N-Queens, Sudoku).

```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []

    def backtrack(start: int, path: list[int]):
        result.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()

    backtrack(0, [])
    return result
```
**Complexity:** typically O(2ⁿ) or O(n!) depending on the problem.
**Classics:** Subsets, Permutations, Combination Sum, N-Queens, Word Search, Sudoku Solver,
Palindrome Partitioning.

### 16. Dynamic Programming
**Recognize it by:** "min/max/count ways", overlapping subproblems, optimal substructure.

```python
# 1D DP - House Robber
def rob(nums: list[int]) -> int:
    prev, curr = 0, 0
    for x in nums:
        prev, curr = curr, max(curr, prev + x)
    return curr

# 2D DP - Longest Common Subsequence
def lcs(a: str, b: str) -> int:
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[m][n]

# 0/1 Knapsack
def knapsack(weights: list[int], values: list[int], capacity: int) -> int:
    dp = [0] * (capacity + 1)
    for w, v in zip(weights, values):
        for c in range(capacity, w - 1, -1):
            dp[c] = max(dp[c], dp[c - w] + v)
    return dp[capacity]
```
**Complexity:** varies; typically O(n), O(n·m), or O(n·capacity).
**Classics:** Climbing Stairs, House Robber, Coin Change, Longest Increasing Subsequence,
Edit Distance, Longest Common Subsequence, 0/1 Knapsack, Word Break, Unique Paths,
Partition Equal Subset Sum.

### 17. Greedy
**Recognize it by:** local optimal choice leads to global optimal; interval scheduling, gas station.

```python
def max_profit(prices: list[int]) -> int:
    profit = 0
    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:
            profit += prices[i] - prices[i - 1]
    return profit
```
**Complexity:** usually O(n log n) (if sorting needed) or O(n).
**Classics:** Best Time to Buy and Sell Stock II, Jump Game, Gas Station,
Non-overlapping Intervals, Minimum Number of Arrows to Burst Balloons.

### 18. Intervals
**Recognize it by:** list of [start, end] ranges, merge/insert/overlap questions.

```python
def merge_intervals(intervals: list[list[int]]) -> list[list[int]]:
    intervals.sort(key=lambda x: x[0])
    merged = [intervals[0]]
    for start, end in intervals[1:]:
        if start <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])
    return merged
```
**Complexity:** O(n log n) time (sort dominates), O(n) space.
**Classics:** Merge Intervals, Insert Interval, Non-overlapping Intervals,
Meeting Rooms I/II.

### 19. Trie
**Recognize it by:** prefix search, autocomplete, word dictionary.

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_word = True

    def search(self, word: str) -> bool:
        node = self._find(word)
        return node is not None and node.is_word

    def starts_with(self, prefix: str) -> bool:
        return self._find(prefix) is not None

    def _find(self, s: str):
        node = self.root
        for ch in s:
            if ch not in node.children:
                return None
            node = node.children[ch]
        return node
```
**Complexity:** O(L) per operation (L = word length), O(N·L) space.
**Classics:** Implement Trie, Word Search II, Design Add and Search Words, Replace Words.

### 20. Bit Manipulation
**Recognize it by:** single number among duplicates, subsets via bitmask, power-of-two checks.

```python
def single_number(nums: list[int]) -> int:
    result = 0
    for x in nums:
        result ^= x
    return result

def count_set_bits(n: int) -> int:
    count = 0
    while n:
        n &= n - 1  # clears lowest set bit
        count += 1
    return count

def is_power_of_two(n: int) -> bool:
    return n > 0 and (n & (n - 1)) == 0
```
**Complexity:** O(1) to O(log n) typically.
**Classics:** Single Number, Number of 1 Bits, Counting Bits, Power of Two,
Sum of Two Integers (no + operator), Subsets via bitmask.

---

## Curated Problem List by Pattern

| Pattern | LeetCode | HackerRank |
|---|---|---|
| Two Pointers | Two Sum II, 3Sum, 4Sum, Container With Most Water, Trapping Rain Water, Sort Colors, Remove Duplicates from Sorted Array, Valid Palindrome | Sherlock and Array, Pairs, Sock Merchant, ANAGRAM |
| Sliding Window | Longest Substring Without Repeating Characters, Minimum Window Substring, Longest Repeating Character Replacement, Permutation in String, Sliding Window Maximum, Fruit Into Baskets | Substring Diff, Max Subarray Sum, Highest Value Palindrome, Maximum Subarray Sum Circular |
| Fast & Slow Pointers | Linked List Cycle, Linked List Cycle II, Middle of the Linked List, Happy Number, Find the Duplicate Number, Palindrome Linked List | Cycle Detection, Print in Reverse |
| Binary Search | Search in Rotated Sorted Array, Koko Eating Bananas, Find Minimum in Rotated Sorted Array, Median of Two Sorted Arrays, Search a 2D Matrix, Split Array Largest Sum, Capacity To Ship Packages | Ice Cream Parlor, Missing Numbers, Pairs, Climbing the Leaderboard |
| Prefix Sum | Subarray Sum Equals K, Product of Array Except Self, Range Sum Query - Immutable, Continuous Subarray Sum, Maximum Size Subarray Sum Equals k | Cumulative Array Sum, Sales by Match, Circular Array Rotation |
| Hashing | Two Sum, Group Anagrams, Top K Frequent Elements, Longest Consecutive Sequence, Valid Anagram, Subarray Sum Equals K, Contains Duplicate II | Two Strings, Frequency Queries, Sherlock and Anagrams, Ransom Note |
| Stack / Monotonic Stack | Valid Parentheses, Daily Temperatures, Largest Rectangle in Histogram, Min Stack, Next Greater Element I/II, Evaluate Reverse Polish Notation, Basic Calculator | Balanced Brackets, Equal Stacks, Maximum Element, Game of Two Stacks |
| Linked List | Reverse Linked List, Reverse Linked List II, Reverse Nodes in k-Group, Merge Two Sorted Lists, Add Two Numbers, Copy List with Random Pointer, LRU Cache | Reverse a Linked List, Insert a Node, Merge Two Sorted Linked Lists, Delete Duplicate-Value Nodes |
| Trees DFS/BFS | Level Order Traversal, Diameter of Binary Tree, Maximum Depth of Binary Tree, Path Sum II, Binary Tree Right Side View, Lowest Common Ancestor, Serialize/Deserialize Binary Tree | Tree: Height, Level-Order Traversal, Tree: Top View, Balanced Forest |
| BST | Validate BST, Kth Smallest in BST, Insert into a BST, Delete Node in a BST, Convert Sorted Array to BST, Recover Binary Search Tree | Binary Search Tree Insertion, Tree: Huffman Decoding, Self Balancing Tree |
| Graphs | Number of Islands, Course Schedule, Word Ladder, Clone Graph, Rotting Oranges, Pacific Atlantic Water Flow, Number of Connected Components | BFS: Shortest Reach, Roads and Libraries, Dijkstra: Shortest Reach 2, Even Tree |
| Topological Sort | Course Schedule II, Alien Dictionary, Sequence Reconstruction, Minimum Height Trees, Parallel Courses | Topological Sort variants, Job Sequencing |
| Union-Find | Number of Provinces, Redundant Connection, Accounts Merge, Number of Islands II, Graph Valid Tree, Satisfiability of Equality Equations | Merging Communities, Journey to the Moon, Components in a graph |
| Heap | Kth Largest Element, Merge K Sorted Lists, Task Scheduler, Find Median from Data Stream, Top K Frequent Elements, K Closest Points to Origin | Find the Running Median, Jesse and Cookies, QHEAP1, Almost Sorted |
| Backtracking | Subsets, Permutations, N-Queens, Word Search, Combination Sum, Palindrome Partitioning, Sudoku Solver, Generate Parentheses | Crossword Puzzle, Recursive Digit Sum, The Power Sum, Sansa and XOR |
| Dynamic Programming | Coin Change, Longest Increasing Subsequence, Edit Distance, 0/1 Knapsack, Climbing Stairs, House Robber, Word Break, Unique Paths, Longest Palindromic Substring, Partition Equal Subset Sum | The Coin Change Problem, Sam's Puzzle, Max Array Sum, Candies, Fibonacci Modified |
| Greedy | Jump Game, Jump Game II, Gas Station, Non-overlapping Intervals, Minimum Number of Arrows to Burst Balloons, Best Time to Buy and Sell Stock II | Greedy Florist, Luck Balance, Minimum Absolute Difference in an Array, Priyanka and Toys |
| Intervals | Merge Intervals, Insert Interval, Meeting Rooms II, Non-overlapping Intervals, My Calendar I | Crush (interval update variant) |
| Trie | Implement Trie, Word Search II, Design Add and Search Words, Replace Words, Longest Word in Dictionary | Contacts, No Prefix Set |
| Bit Manipulation | Single Number, Counting Bits, Sum of Two Integers, Number of 1 Bits, Power of Two, Missing Number, Reverse Bits | Lonely Integer, Flipping Bits, Maximum XOR of Two Numbers in an Array, AND Product |

---

## How to Practice

1. **Diagnose the pattern first.** Read the constraints (input size, whether it's sorted,
   whether it mentions "contiguous", "prerequisite", "prefix", etc.) before coding.
2. **Write the brute force**, then optimize once correctness is confirmed.
3. **Time yourself** — 20-30 min per medium problem is a healthy target once comfortable.
4. **Redo missed problems** after a few days (spaced repetition beats grinding new ones).
5. Track weak patterns and drill those specifically instead of solving randomly.

---

*Contributions welcome — open a PR to add problems, fix templates, or add a new pattern.*
