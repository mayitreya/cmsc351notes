# Graphs

## Preliminary Definitions
- A *graph* is a collection of nodes/vertices that are connected together using edges.
- We say that two vertices are *adjacent* when there is a single edge connecting both of the vertices together.
- The *degree* of a single vertex is just the number of edges that it has (which connect to other vertices).
- A *loop* is a vertex that has an edge connecting to itself.
- *Multiple edges* refers to when two vertices are connected using two or more edges.
- A graph is *simple* when it has zero loops and zero multiple edges.
- A graph is *weighted* when each edge has a "cost" (a value) associated with it.
- A graph is *directed* when you can go from one vertex to another, but can't go the opposite direction using the same edge.

## More Definitions
- A *walk* is a sequence from one vertex to another, and you may repeat vertices and edges.
- A *trail* is just a walk without the ability to repeat edges (but you may repeat vertices).
- A *path* is just a walk without the ability to repeat edges OR vertices.
- A *cycle* is a path in which we must allow (insist) that the starting vertex is equal to the ending vertex. Order does NOT matter!
- A graph is *connected* if, for two vertices, there is a path between them.
- A *tree* is a simple connected graph with no cycles.

## Graph Storage
We have a few different options in order to store a graph.

### Adjacency Matrices
Suppose we had a simple, unweighted, undirected graph with $V$ vertices. The adjacency matrix for this graph is a  $V \times V$ matrix, $A$, in which:

$$A[i][j] = \left\{\begin{matrix}
\text{0 if i and j aren't connected by an edge}
\\
\text{1 if i and j are connected by an edge}
\end{matrix}\right.$$


A graph can be represented as an adjacency matrix as such:

$$
\begin{bmatrix}
0 &  1&  0&  1&  0\\
1 &  0&  1&  1&  0\\
0 &  1&  0&  1&  1\\
1 &  1&  1&  0&  0\\
0 &  0&  1&  0&  0\\
\end{bmatrix}
$$

Very difficult to insert images here so this is the best way I can show the graph.

```

  1
 /|\
0 | 2
 \|/ \
  3   4

```

This graph can be represented as an adjacency list as such:

$$[[1, 3], [0, 2, 3], [1, 3, 4], [0, 1, 2], [2]]$$

Where list at index $i$ represents the vertices that vertex $i$ connects to directly.

## Shortest Path Algorithm
Suppose we have a simple, undirected, unweighted, and connected graph. How can we find the shortest distance from starting vertex $s$ to ending vertex $t$?

- We start at $s$
- We visit all vertices that directly connect to $s$ (suppose they're $g$, $h$, and $k$)
- We visit all vertices that directly connect to $g$, $h$, and $k$
- Repeat
- It's important that we don't revisit vertices that we've already seen

The thing is, we don't necessarily need a $t$ (ending vertex) in order for this shortest path algorithm to work correctly. We can simply find the shortest path from $s$ to *any* vertex that we want.

## Pseudo-pseudocode
Suppose a graph has $V$ vertices...
- We initialize a $d$ list of length $V$, where $d[i]$ is the distance from the starting vertex $s$ to some vertex $i$
- All entries of $d$ are initially $\infty$, except $d[s]$ which we say is 0
- We initialize a $p$ list of length $V$, where $p[i]$ is the predecessor vertex used to get to vertex $i$. We use this $p$ list to figure out the shortest path at the end
- All entries of $p$ are initially $NULL$ or $N$
- Initialize an empty queue, $Q$ and enqueue the starting vertex $s$, such that $Q = \{s\}$

Then...
- `x = Q.dequeue` which returns the vertex at the front of the queue (the very first time this is run, it returns $s$, and now $Q = \{\}$)
- For each vertex $y$ adjacent to $x$ such that $d[y]$ == $\infty$:
  - Assign $d[y]$ = $d[x] + 1$
  - Assign $p[y]$ = $x$
  - `Q.enqueue(y)`
  - If $y$ == $t$ then exit program
- Repeat the above until $t$ is found *or* we have seen every vertex in the graph ($Q$ is empty)

## Time Complexity
This depends on *how* the graph is stored. Is it stored as an adjacency matrix? Then it's $\Theta(V^2)$. Is it stored as an adjacency list? Then it's $\Theta(V + E)$ where $E$ stands for edges.

## Breadth First Traversal
This is very similar to the shortest path algorithm. The basic idea is that we start at vertex $s$, and we visit the vertices that are closest to $s$ first, and then branch out from there. There are a few notable differences, however:
1. Remove the distance ($d$) list
2. Replace $d$ with a $VISITED$ list instead, indicating what vertices we have visited
3. Remove the $p$ (predecessor) list, and replace it with a $VORDER$ list, which indicates the order in which we have visited vertices

The time complexity is the same as the shortest path algorithm from above

## Depth First Traversal
The complete opposite of BFT, we visit the vertices furthest away from $s$, and backtrack to figure out all possible paths.

### Pseudocode
```pseudocode
VORDER  = []
VISITED = [FALSE, FALSE, FALSE, ..., FALSE] # Length V
STACK   = [s]

while STACK not empty:
  x = STACK.pop()

  if VISITED[x] == FALSE:
    VISITED[x] = TRUE
    VORDER.append( x )
  endif

  for all nodes y adjacent to x:
    if VISITED[y] = FALSE:
      STACK.push( y )
    endif
  endfor

endwhile
```
### Time Complexity
$\Theta(V^2 + E)$ when we look at the pseudocode.

## Dijkstra's Algorithm
Very very similar to the shortest path algorithm from above, except now the graphs can have *positive* weights associated with them (when transitioning from vertex to vertex). The basic idea is that given a graph, what is the *smallest cost* to get from vertex $s$ to vertex $t$? The important thing is that it may not actually be the *shortest* path (i.e., the shortest path could be from vertex $A$ to vertex $B$ directly, but the shortest *cost* could be $A \rightarrow C \rightarrow F \rightarrow B$).


### The Basic Idea
Given a graph G and a starting vertex $s_0$:
- We create a set called $S = \{\}$
- We create a $d$ (distance) list and assign $\infty$ as each distance except $s_0$, which gets a 0 distance (naturally)
- We create a $p$ (predecessor) list and assign $NULL$ or $N$ to each index

Then:
- Pick a vertex, $x$, of minimum distance from where we currently are, which is also *not* in $S$
- Put $x$ in $S$
- For each vertex $y$ that's adjacent to $x$:
  - If $d[x]$ + `weight_between(x, y)` < $d[y]$:
    - $d[y]$ = $d[x]$ + `weight_between(x, y)`
    - $p[y]$ = $x$
- Repeat until we've seen every vertex

### Time Complexity
$\Theta(V^2 + E)$, just like DFT, due to the pseudocode.