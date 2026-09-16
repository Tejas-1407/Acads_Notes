# Why Kosaraju Works — My Mental Model

My mental model:

- First, run **DFS on the original graph** and record **post times**.
- The SCC that finishes last gets the **maximum post order**.
- Build the **transpose graph** by reversing **every directed edge**.
- Now start DFS from the **maximum post** vertex.
- Because all the edges are reversed, the outgoing bridges between SCCs become incoming bridges.
- Therefore, DFS cannot leak into another SCC. It gets trapped inside its own strongly connected component.
- That DFS finishes and gives **one complete SCC**.
- Then start another DFS from the next highest unvisited post-order vertex.
- Again, it stays inside exactly one SCC.

**Conclusion:** Each DFS tree in the transpose graph corresponds to **one separate SCC**.

**One important nuance:** It's not just that the edges are reversed—it is the combination of **(1) decreasing post order** and **(2) the transpose graph** that guarantees each DFS stays inside one SCC.