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

Difficulty follows each platform's own rating (LeetCode: Easy/Medium/Hard, HackerRank: Easy/Medium/Hard).

### Two Pointers
| Problem | Platform | Difficulty |
|---|---|---|
| Two Sum II | LeetCode | Medium |
| 3Sum | LeetCode | Medium |
| 4Sum | LeetCode | Medium |
| Container With Most Water | LeetCode | Medium |
| Trapping Rain Water | LeetCode | Hard |
| Sort Colors | LeetCode | Medium |
| Remove Duplicates from Sorted Array | LeetCode | Easy |
| Valid Palindrome | LeetCode | Easy |
| Sherlock and Array | HackerRank | Easy |
| Pairs | HackerRank | Medium |
| Sock Merchant | HackerRank | Easy |
| ANAGRAM | HackerRank | Medium |

### Sliding Window
| Problem | Platform | Difficulty |
|---|---|---|
| Longest Substring Without Repeating Characters | LeetCode | Medium |
| Minimum Window Substring | LeetCode | Hard |
| Longest Repeating Character Replacement | LeetCode | Medium |
| Permutation in String | LeetCode | Medium |
| Sliding Window Maximum | LeetCode | Hard |
| Fruit Into Baskets | LeetCode | Medium |
| Substring Diff | HackerRank | Medium |
| Max Subarray Sum | HackerRank | Medium |
| Highest Value Palindrome | HackerRank | Medium |
| Maximum Subarray Sum (Circular) | HackerRank | Hard |

### Fast & Slow Pointers
| Problem | Platform | Difficulty |
|---|---|---|
| Linked List Cycle | LeetCode | Easy |
| Linked List Cycle II | LeetCode | Medium |
| Middle of the Linked List | LeetCode | Easy |
| Happy Number | LeetCode | Easy |
| Find the Duplicate Number | LeetCode | Medium |
| Palindrome Linked List | LeetCode | Easy |
| Cycle Detection | HackerRank | Easy |
| Print in Reverse | HackerRank | Easy |

### Binary Search
| Problem | Platform | Difficulty |
|---|---|---|
| Search in Rotated Sorted Array | LeetCode | Medium |
| Koko Eating Bananas | LeetCode | Medium |
| Find Minimum in Rotated Sorted Array | LeetCode | Medium |
| Median of Two Sorted Arrays | LeetCode | Hard |
| Search a 2D Matrix | LeetCode | Medium |
| Split Array Largest Sum | LeetCode | Hard |
| Capacity To Ship Packages Within D Days | LeetCode | Medium |
| Ice Cream Parlor | HackerRank | Medium |
| Missing Numbers | HackerRank | Easy |
| Climbing the Leaderboard | HackerRank | Medium |

### Prefix Sum
| Problem | Platform | Difficulty |
|---|---|---|
| Subarray Sum Equals K | LeetCode | Medium |
| Product of Array Except Self | LeetCode | Medium |
| Range Sum Query - Immutable | LeetCode | Easy |
| Continuous Subarray Sum | LeetCode | Medium |
| Maximum Size Subarray Sum Equals k | LeetCode | Medium |
| Sales by Match | HackerRank | Easy |
| Circular Array Rotation | HackerRank | Easy |
| Crush | HackerRank | Medium |

### Hashing / Hash Map
| Problem | Platform | Difficulty |
|---|---|---|
| Two Sum | LeetCode | Easy |
| Group Anagrams | LeetCode | Medium |
| Top K Frequent Elements | LeetCode | Medium |
| Longest Consecutive Sequence | LeetCode | Medium |
| Valid Anagram | LeetCode | Easy |
| Contains Duplicate II | LeetCode | Easy |
| Two Strings | HackerRank | Easy |
| Frequency Queries | HackerRank | Medium |
| Sherlock and Anagrams | HackerRank | Medium |
| Ransom Note | HackerRank | Easy |

### Stack / Monotonic Stack
| Problem | Platform | Difficulty |
|---|---|---|
| Valid Parentheses | LeetCode | Easy |
| Daily Temperatures | LeetCode | Medium |
| Largest Rectangle in Histogram | LeetCode | Hard |
| Min Stack | LeetCode | Medium |
| Next Greater Element I | LeetCode | Easy |
| Next Greater Element II | LeetCode | Medium |
| Evaluate Reverse Polish Notation | LeetCode | Medium |
| Basic Calculator | LeetCode | Hard |
| Balanced Brackets | HackerRank | Medium |
| Equal Stacks | HackerRank | Easy |
| Maximum Element | HackerRank | Medium |
| Game of Two Stacks | HackerRank | Medium |

### Linked List In-Place Reversal
| Problem | Platform | Difficulty |
|---|---|---|
| Reverse Linked List | LeetCode | Easy |
| Reverse Linked List II | LeetCode | Medium |
| Reverse Nodes in k-Group | LeetCode | Hard |
| Merge Two Sorted Lists | LeetCode | Easy |
| Add Two Numbers | LeetCode | Medium |
| Copy List with Random Pointer | LeetCode | Medium |
| LRU Cache | LeetCode | Medium |
| Reverse a Linked List | HackerRank | Easy |
| Insert a Node at a Specific Position | HackerRank | Easy |
| Merge Two Sorted Linked Lists | HackerRank | Easy |
| Delete Duplicate-Value Nodes from a Sorted Linked List | HackerRank | Easy |

### Trees: DFS / BFS
| Problem | Platform | Difficulty |
|---|---|---|
| Binary Tree Level Order Traversal | LeetCode | Medium |
| Diameter of Binary Tree | LeetCode | Easy |
| Maximum Depth of Binary Tree | LeetCode | Easy |
| Path Sum II | LeetCode | Medium |
| Binary Tree Right Side View | LeetCode | Medium |
| Lowest Common Ancestor of a Binary Tree | LeetCode | Medium |
| Serialize and Deserialize Binary Tree | LeetCode | Hard |
| Tree: Height of a Binary Tree | HackerRank | Easy |
| Tree: Level Order Traversal | HackerRank | Easy |
| Tree: Top View | HackerRank | Medium |
| Balanced Forest | HackerRank | Hard |

### Binary Search Tree
| Problem | Platform | Difficulty |
|---|---|---|
| Validate Binary Search Tree | LeetCode | Medium |
| Kth Smallest Element in a BST | LeetCode | Medium |
| Insert into a Binary Search Tree | LeetCode | Medium |
| Delete Node in a BST | LeetCode | Medium |
| Convert Sorted Array to Binary Search Tree | LeetCode | Easy |
| Recover Binary Search Tree | LeetCode | Medium |
| Binary Search Tree: Insertion | HackerRank | Easy |
| Tree: Huffman Decoding | HackerRank | Medium |
| Self Balancing Tree | HackerRank | Hard |

### Graphs: BFS / DFS
| Problem | Platform | Difficulty |
|---|---|---|
| Number of Islands | LeetCode | Medium |
| Course Schedule | LeetCode | Medium |
| Word Ladder | LeetCode | Hard |
| Clone Graph | LeetCode | Medium |
| Rotting Oranges | LeetCode | Medium |
| Pacific Atlantic Water Flow | LeetCode | Medium |
| Number of Connected Components in an Undirected Graph | LeetCode | Medium |
| BFS: Shortest Reach in a Graph | HackerRank | Medium |
| Roads and Libraries | HackerRank | Medium |
| Dijkstra: Shortest Reach 2 | HackerRank | Medium |
| Even Tree | HackerRank | Medium |

### Topological Sort
| Problem | Platform | Difficulty |
|---|---|---|
| Course Schedule II | LeetCode | Medium |
| Alien Dictionary | LeetCode | Hard |
| Sequence Reconstruction | LeetCode | Medium |
| Minimum Height Trees | LeetCode | Medium |
| Parallel Courses | LeetCode | Medium |
| Topological Sort (Practice) | HackerRank | Medium |
| Job Sequencing Problem | HackerRank | Medium |

### Union-Find (Disjoint Set)
| Problem | Platform | Difficulty |
|---|---|---|
| Number of Provinces | LeetCode | Medium |
| Redundant Connection | LeetCode | Medium |
| Accounts Merge | LeetCode | Medium |
| Number of Islands II | LeetCode | Hard |
| Graph Valid Tree | LeetCode | Medium |
| Satisfiability of Equality Equations | LeetCode | Medium |
| Merging Communities | HackerRank | Medium |
| Journey to the Moon | HackerRank | Medium |
| Components in a Graph | HackerRank | Easy |

### Heap / Priority Queue
| Problem | Platform | Difficulty |
|---|---|---|
| Kth Largest Element in an Array | LeetCode | Medium |
| Merge K Sorted Lists | LeetCode | Hard |
| Task Scheduler | LeetCode | Medium |
| Find Median from Data Stream | LeetCode | Hard |
| Top K Frequent Elements | LeetCode | Medium |
| K Closest Points to Origin | LeetCode | Medium |
| Find the Running Median | HackerRank | Hard |
| Jesse and Cookies | HackerRank | Medium |
| QHEAP1 | HackerRank | Easy |
| Almost Sorted | HackerRank | Medium |

### Backtracking
| Problem | Platform | Difficulty |
|---|---|---|
| Subsets | LeetCode | Medium |
| Permutations | LeetCode | Medium |
| N-Queens | LeetCode | Hard |
| Word Search | LeetCode | Medium |
| Combination Sum | LeetCode | Medium |
| Palindrome Partitioning | LeetCode | Medium |
| Sudoku Solver | LeetCode | Hard |
| Generate Parentheses | LeetCode | Medium |
| Crossword Puzzle | HackerRank | Hard |
| Recursive Digit Sum | HackerRank | Easy |
| The Power Sum | HackerRank | Medium |
| Sansa and XOR | HackerRank | Easy |

### Dynamic Programming
| Problem | Platform | Difficulty |
|---|---|---|
| Coin Change | LeetCode | Medium |
| Longest Increasing Subsequence | LeetCode | Medium |
| Edit Distance | LeetCode | Hard |
| Climbing Stairs | LeetCode | Easy |
| House Robber | LeetCode | Medium |
| Word Break | LeetCode | Medium |
| Unique Paths | LeetCode | Medium |
| Longest Palindromic Substring | LeetCode | Medium |
| Partition Equal Subset Sum | LeetCode | Medium |
| The Coin Change Problem | HackerRank | Medium |
| Sam's Puzzle (Max Array Sum family) | HackerRank | Medium |
| Max Array Sum | HackerRank | Medium |
| Candies | HackerRank | Medium |
| Fibonacci Modified | HackerRank | Medium |

### Greedy
| Problem | Platform | Difficulty |
|---|---|---|
| Jump Game | LeetCode | Medium |
| Jump Game II | LeetCode | Medium |
| Gas Station | LeetCode | Medium |
| Non-overlapping Intervals | LeetCode | Medium |
| Minimum Number of Arrows to Burst Balloons | LeetCode | Medium |
| Best Time to Buy and Sell Stock II | LeetCode | Medium |
| Greedy Florist | HackerRank | Medium |
| Luck Balance | HackerRank | Easy |
| Minimum Absolute Difference in an Array | HackerRank | Easy |
| Priyanka and Toys | HackerRank | Easy |

### Intervals
| Problem | Platform | Difficulty |
|---|---|---|
| Merge Intervals | LeetCode | Medium |
| Insert Interval | LeetCode | Medium |
| Meeting Rooms II | LeetCode | Medium |
| Non-overlapping Intervals | LeetCode | Medium |
| My Calendar I | LeetCode | Medium |
| Crush (interval update variant) | HackerRank | Medium |

### Trie
| Problem | Platform | Difficulty |
|---|---|---|
| Implement Trie (Prefix Tree) | LeetCode | Medium |
| Word Search II | LeetCode | Hard |
| Design Add and Search Words Data Structure | LeetCode | Medium |
| Replace Words | LeetCode | Medium |
| Longest Word in Dictionary | LeetCode | Medium |
| Contacts | HackerRank | Medium |
| No Prefix Set | HackerRank | Medium |

### Bit Manipulation
| Problem | Platform | Difficulty |
|---|---|---|
| Single Number | LeetCode | Easy |
| Counting Bits | LeetCode | Easy |
| Sum of Two Integers | LeetCode | Medium |
| Number of 1 Bits | LeetCode | Easy |
| Power of Two | LeetCode | Easy |
| Missing Number | LeetCode | Easy |
| Reverse Bits | LeetCode | Easy |
| Lonely Integer | HackerRank | Easy |
| Flipping Bits | HackerRank | Easy |
| Maximum XOR of Two Numbers in an Array | HackerRank | Medium |
| AND Product | HackerRank | Medium |

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
