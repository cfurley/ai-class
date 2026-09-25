# Assignment 1 — Written Tasks (A* Search)

Graph: Figure 3, heuristics from Figure 4.

Edge weights: Home–C 4, Home–E 3, Home–F 2, C–E 3, C–Store A 2, C–D 3, E–F 6,
E–Store A 5, Store A–D 4, Store A–G 10, D–K 2, K–J 2, K–G 12, G–J 4, G–Store B 3,
F–G 7, F–Store B 5, Store B–J 4.

A* expands the frontier node with the lowest f(n) = g(n) + h(n), breaking ties
alphabetically. The search stops when the goal is *expanded*, not merely added
to the frontier.

## Task 4: A* from Store A to Store B

Uses the **h(n): Store B** column.

**Step 1 — Expand Store A** (g=0, f=0+11=11). Add its neighbors:

| Node | g | h | f |
|---|---|---|---|
| C | 2 | 8 | **10** |
| D | 4 | 7 | 11 |
| G | 10 | 3 | 13 |
| E | 5 | 10 | 15 |

**Step 2 — Expand C** (f=10, lowest). New reachable node: Home (g=6, h=7, f=13).
Going C→D would cost g=5, f=12, worse than D's existing f=11, so D keeps its old
value. Frontier: **D(11)**, G(13), Home(13), E(15).

**Step 3 — Expand D** (f=11). Adds K: g=4+2=6, h=6, f=12.
Frontier: **K(12)**, G(13), Home(13), E(15).

**Step 4 — Expand K** (f=12). Adds J: g=6+2=8, h=4, f=12. (K→G gives g=18, f=21 —
worse than G's 13.) Frontier: **J(12)**, G(13), Home(13), E(15).

**Step 5 — Expand J** (f=12). Adds Store B: g=8+4=12, h=0, f=12. (J→G gives f=15 —
worse.) Frontier: **Store B(12)**, G(13), Home(13), E(15).

**Step 6 — Expand Store B** (f=12) → goal reached.

**Search sequence:** Store A, C, D, K, J, Store B

**Optimal path:** Store A → D → K → J → Store B, **cost = 4+2+2+4 = 12**

(Sanity check: the direct route Store A → G → Store B costs 10+3 = 13, so 12 is optimal.)

### Search tree

```mermaid
flowchart TD
    SA["1. Store A<br/>g=0, h=11, f=11"]
    C["2. C<br/>g=2, h=8, f=10"]
    D["3. D<br/>g=4, h=7, f=11"]
    G["G<br/>g=10, h=3, f=13<br/>(never expanded)"]
    E["E<br/>g=5, h=10, f=15<br/>(never expanded)"]
    HOME["Home<br/>g=6, h=7, f=13<br/>(never expanded)"]
    K["4. K<br/>g=6, h=6, f=12"]
    J["5. J<br/>g=8, h=4, f=12"]
    SB["6. Store B (GOAL)<br/>g=12, h=0, f=12"]

    SA -->|2| C
    SA ==>|4| D
    SA -->|10| G
    SA -->|5| E
    C -->|4| HOME
    D ==>|2| K
    K ==>|2| J
    J ==>|4| SB

    classDef expanded fill:#c8e6c9,stroke:#2e7d32,color:#1b3a1e
    classDef goal fill:#ffe082,stroke:#e65100,color:#4a2800,stroke-width:3px
    classDef frontier fill:#eeeeee,stroke:#9e9e9e,color:#555,stroke-dasharray: 5 5
    class SA,C,D,K,J expanded
    class SB goal
    class G,E,HOME frontier
```

## Task 5: A* from J to Store A

Uses the **h(n): Store A** column.

**Step 1 — Expand J** (g=0, f=0+7=7). Add its neighbors:

| Node | g | h | f |
|---|---|---|---|
| K | 2 | 6 | **8** |
| G | 4 | 9 | 13 |
| Store B | 4 | 11 | 15 |

**Step 2 — Expand K** (f=8). Adds D: g=2+2=4, h=4, f=8. (K→G gives g=14 — worse
than G's g=4.) Frontier: **D(8)**, G(13), Store B(15).

**Step 3 — Expand D** (f=8). Adds Store A: g=4+4=8, h=0, f=8, and C: g=7, h=2, f=9.
Frontier: **Store A(8)**, C(9), G(13), Store B(15).

**Step 4 — Expand Store A** (f=8) → goal reached.

**Search sequence:** J, K, D, Store A

**Optimal path:** J → K → D → Store A, **cost = 2+2+4 = 8**

(Sanity check: J → G → Store A costs 4+10 = 14, so 8 is optimal.)

### Search tree

```mermaid
flowchart TD
    J["1. J<br/>g=0, h=7, f=7"]
    K["2. K<br/>g=2, h=6, f=8"]
    G["G<br/>g=4, h=9, f=13<br/>(never expanded)"]
    SB["Store B<br/>g=4, h=11, f=15<br/>(never expanded)"]
    D["3. D<br/>g=4, h=4, f=8"]
    C["C<br/>g=7, h=2, f=9<br/>(never expanded)"]
    SA["4. Store A (GOAL)<br/>g=8, h=0, f=8"]

    J ==>|2| K
    J -->|4| G
    J -->|4| SB
    K ==>|2| D
    D -->|3| C
    D ==>|4| SA

    classDef expanded fill:#c8e6c9,stroke:#2e7d32,color:#1b3a1e
    classDef goal fill:#ffe082,stroke:#e65100,color:#4a2800,stroke-width:3px
    classDef frontier fill:#eeeeee,stroke:#9e9e9e,color:#555,stroke-dasharray: 5 5
    class J,K,D expanded
    class SA goal
    class G,SB frontier
```
