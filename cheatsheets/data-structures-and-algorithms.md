# Data Structures and Algorithms Cheatsheet

Core complexity tables, data structures, sorting algorithms and coding patterns, with signals for when to use each one.

> Related: [cs-fundamentals.md](cs-fundamentals.md) · [programming-languages.md](programming-languages.md) · [Coding topic](../topics/02-coding/README.md) · [CS fundamentals topic](../topics/13-cs-fundamentals/README.md)

## Complexity basics

| Term | What it means |
|---|---|
| **Big-O notation** | Upper bound on how cost grows with input size. Used to compare algorithms independently of hardware; usually quoted for the worst case. |
| **Big-Omega notation** (Ω) | Lower bound on growth. Used to state limits, for example comparison sorts need Ω(n log n) comparisons. |
| **Big-Theta notation** (Θ) | Tight bound, where upper and lower bounds match. Describes growth rate exactly, ignoring constants. |
| **Amortised cost** | Average cost per operation across a sequence, even if some are expensive. Dynamic array appends are amortised O(1). |
| **Space complexity** | Extra memory used as input grows. Recursion depth counts, because the call stack consumes memory. |
| **Stable sort** | Equal items keep their original relative order. Matters when sorting by several keys in succession. |
| **In-place algorithm** | Uses only a small amount of extra memory. Important for large inputs or where memory is tight. |

```text
Growth: O(1) < O(log n) < O(n) < O(n log n) < O(n^2) < O(2^n) < O(n!)
Rough limits for about 10^8 simple operations per second:
n <= 10^8 O(n) · n <= 10^6 O(n log n) · n <= 10^4 O(n^2) · n <= 25 O(2^n) · n <= 10 O(n!)
```

## Data structures

| Term | What it means |
|---|---|
| **Array** | Contiguous elements accessed by index. Offers O(1) access and excellent cache locality, but middle inserts are slow. |
| **Dynamic array** | Array that grows by reallocating, as in Python `list` or Java `ArrayList`. Default sequence type, with amortised O(1) append. |
| **Linked list** | Nodes connected by pointers. O(1) insert and delete at a known node, but slow traversal and poor cache locality. |
| **Stack** | Last in, first out structure. Suits nested structures, undo, and converting recursion into iteration. |
| **Queue** | First in, first out structure. Suits BFS, scheduling and buffering. |
| **Deque** (Double-Ended Queue) | Adds and removes at both ends in O(1). Powers sliding window maximum and many BFS variants. |
| **Hash table** | Maps keys to buckets through a hash. Average O(1) lookup, but no ordering and worst-case O(n) with collisions. |
| **Binary tree** | Each node has at most two children. The basis for search trees, heaps and many recursion problems. |
| **BST** (Binary Search Tree) | Left subtree holds smaller keys, right subtree larger. Keeps order, but degrades to O(n) if unbalanced. |
| **Balanced BST** (Binary Search Tree) | Self-rebalancing tree such as AVL (Adelson-Velsky and Landis) or red-black. Guarantees O(log n) operations and ordered iteration. |
| **Heap** | Tree where each parent outranks its children, usually stored in an array. Gives O(1) peek and O(log n) insert and removal. |
| **Priority queue** | Abstract structure that always returns the highest-priority item. Normally implemented with a heap. |
| **Trie** | Tree keyed by characters, sharing prefixes. Fast prefix search and autocomplete, in time proportional to key length. |
| **Graph** | Nodes connected by edges, directed or undirected. Models networks, dependencies and routes. |
| **DAG** (Directed Acyclic Graph) | Directed graph with no cycles. Models dependencies, and always has a topological order. |
| **Adjacency list** | Stores each node's neighbours. Uses O(V + E) space, making it the default for sparse graphs. |
| **Adjacency matrix** | V × V grid of edges. O(1) edge checks but O(V²) space, so suited to dense graphs. |
| **Union-find** (Disjoint Set Union) | Tracks connected groups with near-constant merge and find. Used for connectivity, and for cycle detection with Kruskal's algorithm. |
| **Skip list** | Layered linked lists with random express lanes. Ordered operations in expected O(log n); simpler to implement than balanced trees. |

| Structure | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Dynamic array | O(1) | O(n) | O(1) amortised at end, O(n) elsewhere | O(1) at end, O(n) elsewhere | O(n) |
| Linked list | O(n) | O(n) | O(1) at known position | O(1) at known node (doubly linked) | O(n) |
| Stack or queue | Top or front only, O(1) | O(n) | O(1) | O(1) | O(n) |
| Hash table | Not applicable | O(1) average, O(n) worst | O(1) average, O(n) worst | O(1) average, O(n) worst | O(n) |
| Unbalanced BST | O(n) worst | O(n) worst | O(n) worst | O(n) worst | O(n) |
| Balanced BST | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Binary heap | O(1) peek | O(n) | O(log n) | O(log n) top | O(n) |
| Trie (key length L) | O(L) | O(L) | O(L) | O(L) | O(total characters) |
| Skip list | O(log n) expected | O(log n) expected | O(log n) expected | O(log n) expected | O(n) expected |
| Union-find | Not applicable | Near O(1) amortised | Near O(1) amortised merge | Not supported | O(n) |

Building a heap from an existing array takes O(n), not O(n log n).

## Sorting

| Term | What it means |
|---|---|
| **Quicksort** | Partitions around a pivot, then recurses. Very fast in practice in place, but O(n²) with consistently poor pivots. |
| **Merge sort** | Splits, sorts each half, then merges. Stable and guaranteed O(n log n), but needs extra memory; suits linked lists and external sorting. |
| **Heapsort** | Builds a heap, then repeatedly extracts the top. In place with guaranteed O(n log n), but slower in practice from poor locality. |
| **Insertion sort** | Inserts each item into a growing sorted section. Efficient on small or nearly sorted inputs, so hybrid sorts use it. |
| **Counting sort** | Tallies values across a small known range. Runs in O(n + k), beating comparison sorts when k is small. |
| **Radix sort** | Sorts digit by digit using a stable sort. Efficient for fixed-length integers or strings. |
| **Timsort** | Merge and insertion hybrid exploiting existing runs. Stable, and used by Python and for Java object sorting. |
| **Quickselect** | Partitions like quicksort but recurses into one side. Finds the k-th item in O(n) on average. |

| Algorithm | Best | Average | Worst | Extra space | Stable |
|---|---|---|---|---|---|
| Bubble sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Insertion sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Merge sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quicksort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heapsort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Timsort | O(n) | O(n log n) | O(n log n) | O(n) | Yes |
| Counting sort | O(n + k) | O(n + k) | O(n + k) | O(n + k) | Yes |
| Radix sort | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | Yes |

## Array and string patterns

| Term | What it means |
|---|---|
| **Two pointers** | Two indexes moving through data, often inward or in step. Turns many O(n²) searches over sorted data into O(n). |
| **Fast and slow pointers** | One pointer moves twice as fast as the other. Finds list cycles and midpoints with O(1) extra memory. |
| **Sliding window** | Keeps a moving contiguous range, adding and removing at its edges. Solves subarray and substring problems in O(n). |
| **Prefix sum** | Stores running totals, so any range sum takes O(1). With a hash map, counts subarrays hitting a target. |
| **Difference array** | Records range updates at their edges, then sums once. Applies many range updates in O(n + q). |
| **Binary search** | Halves a sorted search space each step, for O(log n). Works on any monotonic true-or-false condition. |
| **Binary search on the answer** | Searches possible answers using a feasibility check. Suits "minimise the maximum" problems with monotonic feasibility. |
| **Monotonic stack** | Stack kept in sorted order by popping items that break it. Finds next greater or smaller elements in O(n). |
| **Monotonic deque** | Deque keeping window candidates in order. Finds sliding window maximum or minimum in O(n). |
| **Top K with a heap** | Keeps a heap of size k while scanning. Finds the largest k items in O(n log k). |
| **Merge intervals** | Sorts by start, then combines overlaps in one pass. Solves calendar, range and scheduling problems. |
| **Line sweep** | Processes sorted start and end events while tracking state. Finds maximum overlap, such as rooms needed. |
| **Greedy** | Makes the best local choice and never revisits it. Fast, but only correct when a proof or exchange argument holds. |
| **Divide and conquer** | Splits a problem, solves the parts, then combines them. Underpins merge sort, quicksort and binary search. |
| **Backtracking** | Builds candidates step by step, undoing choices that fail. Explores permutations, subsets and constraint problems, with pruning to reduce cost. |
| **Bit manipulation** | Uses masks and bitwise operators. Enables compact sets and tricks such as XOR to find a single unpaired number. |

```python
# Variable sliding window
left = 0
for right, x in enumerate(items):
    add(x)
    while invalid():
        remove(items[left])
        left += 1
    best = max(best, right - left + 1)
```

```python
# Binary search: first index where a[i] >= target
lo, hi = 0, len(a)
while lo < hi:
    mid = (lo + hi) // 2
    if a[mid] < target:
        lo = mid + 1
    else:
        hi = mid
```

## Trees and graphs

| Term | What it means |
|---|---|
| **BFS** (Breadth-First Search) | Explores level by level using a queue. Finds shortest paths in unweighted graphs in O(V + E). |
| **DFS** (Depth-First Search) | Follows one path as deep as possible, using recursion or a stack. Suits connectivity, cycles and exhaustive tree logic. |
| **Multi-source BFS** (Breadth-First Search) | Starts BFS from several nodes at once. Finds the distance to the nearest source, as in rotting-oranges problems. |
| **Pre-order traversal** | Visits node, then left, then right. Useful for copying or serialising a tree. |
| **In-order traversal** | Visits left, then node, then right. Returns a BST's keys in sorted order. |
| **Post-order traversal** | Visits left, then right, then node. Suits computing a result from children, such as height. |
| **Level-order traversal** | Visits a tree level by level through BFS. Suits views by depth and finding minimum depth. |
| **Topological sort** | Orders a DAG so each dependency comes first, using Kahn's algorithm or DFS. Suits build order and course schedules. |
| **Dijkstra's algorithm** | Finds shortest paths from one source using a priority queue. Requires non-negative edge weights. |
| **Bellman-Ford algorithm** | Relaxes all edges repeatedly. Slower than Dijkstra but handles negative weights and detects negative cycles. |
| **Floyd-Warshall algorithm** | Dynamic programming over intermediate nodes. Finds all-pairs shortest paths, practical for small graphs. |
| **MST** (Minimum Spanning Tree) | Lowest-weight edges connecting all nodes without cycles. Solved by Kruskal's or Prim's algorithm. |

| Algorithm | Solves | Time | Note |
|---|---|---|---|
| BFS | Unweighted shortest path | O(V + E) | Queue |
| DFS | Traversal, cycles, components | O(V + E) | Recursion depth can overflow |
| Topological sort | Dependency order | O(V + E) | DAG only |
| Dijkstra | Weighted shortest path | O((V + E) log V) | No negative edges; binary heap |
| Bellman-Ford | Shortest path with negative edges | O(VE) | Detects negative cycles |
| Floyd-Warshall | All-pairs shortest path | O(V³) | Small graphs |
| Kruskal | Minimum spanning tree | O(E log E) | Sort edges, union-find |
| Prim | Minimum spanning tree | O(E log V) | Priority queue |

```python
# BFS
queue, seen = deque([start]), {start}
while queue:
    node = queue.popleft()
    for next_node in graph[node]:
        if next_node not in seen:
            seen.add(next_node)
            queue.append(next_node)
```

## Dynamic programming

| Term | What it means |
|---|---|
| **DP** (Dynamic Programming) | Solves problems by storing answers to overlapping subproblems. Turns exponential recursion into polynomial time. |
| **Optimal substructure** | The best overall answer is built from the best answers to subproblems. Required for DP to apply. |
| **Overlapping subproblems** | The same subproblems recur many times. This repetition is what makes caching their results worthwhile. |
| **Memoisation** | Top-down recursion with cached results. Quick to write from a recurrence, and computes only states it needs. |
| **Tabulation** | Bottom-up table filled in dependency order. Avoids recursion limits and often allows reducing memory use. |
| **DP state** (Dynamic Programming) | The minimum information that identifies a subproblem. Choosing it well is the hardest part of DP. |
| **0/1 knapsack** | Chooses items, each used at most once, to maximise value within capacity. O(nW) time, and the template for subset selection. |
| **LIS** (Longest Increasing Subsequence) | Longest strictly rising subsequence. O(n²) with simple DP or O(n log n) with binary search. |
| **LCS** (Longest Common Subsequence) | Longest sequence appearing in order in both strings. O(nm), and the base for diff tools. |
| **Edit distance** | Minimum inserts, deletes and substitutions turning one string into another. O(nm), used for spell checking and fuzzy matching. |
| **Kadane's algorithm** | Tracks the best subarray ending at each position. Finds maximum subarray sum in O(n). |
| **Bitmask DP** (Dynamic Programming) | Uses bits in an integer to represent chosen items. Handles subset states when n is roughly 20 or fewer. |

## When to use each

| Term | What it means |
|---|---|
| **Sorted input** | Reach for binary search or two pointers before anything else. Often removes a whole factor of n. |
| **Contiguous subarray or substring** | Use a sliding window for monotonic validity. Use prefix sums with a hash map when values may be negative. |
| **Duplicates or fast lookups** | Use a hash set or hash map. Trades O(n) space for O(1) average lookups. |
| **Next greater or smaller element** | Use a monotonic stack. Each item is pushed and popped at most once. |
| **Top K or streaming median** | Use one or two heaps. Avoids fully sorting the data. |
| **Shortest path, unweighted** | Use Breadth-First Search (BFS). Its first arrival at a node is always the shortest path. |
| **Shortest path, weighted** | Use Dijkstra for non-negative weights; otherwise use Bellman-Ford. |
| **Dependencies or ordering** | Use topological sort. A cycle means no valid order exists. |
| **Connected groups** | Use union-find for changing connectivity, or DFS or BFS for a static graph. |
| **All combinations or arrangements** | Use backtracking with pruning. Exponential, so expect small input limits. |
| **Count ways or best value over choices** | Use DP when states repeat. Consider greedy only if it can be proven correct. |
| **Prefix matching** | Use a trie. Suits autocomplete and word search across many words. |
