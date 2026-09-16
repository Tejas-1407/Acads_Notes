# Lecture 2 — DFS Summary Notes

## 1. Depth First Search (DFS)

**Idea:** Explore as deep as possible, then backtrack.

### DFSAll (covers disconnected graphs)

```text
DFSAll(G):
    mark all vertices unvisited

    for each vertex v:
        if v is unvisited:
            DFS(v)
```

### DFS

```text
DFS(v):
    mark v visited

    clock = clock + 1
    pre[v] = clock

    for each neighbour w of v:
        if w is unvisited:
            parent[w] = v
            DFS(w)

    clock = clock + 1
    post[v] = clock
```

---

## 2. Pre & Post Times

- **Pre(v):** Time when vertex is first discovered.
- **Post(v):** Time when DFS completely finishes that vertex.
- Every vertex has **unique** pre and post times.

Example:

| Vertex | Pre | Post |
|---|---:|---:|
| A | 1 | 10 |
| B | 2 | 7 |
| C | 3 | 6 |
| D | 4 | 5 |

---

## 3. Two Important Lemmas

### Lemma 1 — Active Intervals

For any vertices `u` and `v`, their active intervals are **either nested or disjoint**.

Nested:

```text
u: |----------------------|
v:      |----------|
```

Disjoint:

```text
u: |------|

v:             |------|
```

**There is no partial overlap.**

### Lemma 2 — Active Path

At any moment during DFS, all active vertices form **one directed path** (the recursion stack).

```text
Root
 ↓
 u
 ↓
 w
 ↓
 v (current)
```

---

## 4. Edge Classification

For a directed edge `u → v`:

### Tree Edge

- `v` was unvisited.
- DFS(u) directly calls DFS(v).

### Forward Edge

- `v` is a descendant of `u`.
- Not the parent-child edge.

### Cross Edge

- Connects two different DFS subtrees.
- Intervals are disjoint.

### Back Edge

- Goes from a vertex to its ancestor.
- **Back Edge ⇔ Cycle**

This is the most important theorem.

---

## 5. DAG (Directed Acyclic Graph)

**Definition:** A directed graph with **no cycles**.

### DFS Test

Run DFS.

- Back edge found → **Not DAG**
- No back edge → **DAG**

---

## 6. Topological Sort

A topological ordering is an ordering of vertices such that:

> For every edge `u → v`, `u` appears before `v`.

Example valid orders:

- A B C D
- A C B D

Invalid:

- A B D C ❌ (violates `C → D`)

### Algorithm

1. Run DFS.
2. Compute post numbers.
3. Sort vertices in **decreasing post order**.

Time complexity:

**O(V + E)**

---

## 7. Strongly Connected Components (SCC)

An SCC is a maximal set of vertices where every vertex can reach every other vertex.

Example:

```text
SCC₁ = {A, B, C}
SCC₂ = {D, E}
```

Bridge:

```text
C → D
```

This bridge **does not** merge the SCCs.

---

## 8. Transpose Graph (Gᵀ)

Transpose = Reverse **every** directed edge.

Example:

```text
Original:      A → B → C

Transpose:     A ← B ← C
```

Vertices remain the same.

Only edge directions change.

---

## 9. Kosaraju's Algorithm

1. Run DFS on original graph.
2. Record post numbers.
3. Build transpose graph Gᵀ.
4. Run DFS on Gᵀ in decreasing post order.
5. Each DFS tree is one SCC.

### Why it works

- Component graph is a DAG.
- Source SCCs get the largest post numbers.
- After transpose, DFS stays inside one SCC.

Time complexity:

**O(V + E)**

---

# Key Takeaways

- DFS explores deep, then backtracks.
- Pre/Post timestamps describe DFS structure.
- Active intervals are nested or disjoint.
- Back edge means a cycle.
- DAG has no back edges.
- Topological sort = decreasing post order.
- SCCs are found using **Kosaraju (2 DFS + Transpose)**.