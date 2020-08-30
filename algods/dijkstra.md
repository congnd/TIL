## Dijkstra's algorithm

### The Problem 

Find the shortest path from a source vertex to all vertices in the given graph.

### The solution

___Step1___: Create a list called `visitedList` which keeps track of visited vertices.
This list initially is empty.

___Step2___: Assign a distance value to all vertices in the input graph. 
Initialize all distance values as INFINITE. Assign distance value as 0 
for the source vertex so that it is picked first.

___Step3___: While `visitedList` doesn't include all vertices
- Pick a vertice `u` which is not in the `visitedList` and has minimum distance value.
- Include `u` to the `visitedList`
- Update distance value of all adjacent vertices of `u`. To update the distance values, 
iterate through all adjacent vertices. For every adjacent vertex `v`, if sum of distance value of `u`
(from source) ad weight of edge `u-v`, is less than the distance value of `v`, 
then update the distance value of v.

```
A---6---B---5
|     / |    \
1   2   2     C  
| /     |    /
D---1---E---5       


visitedList = []

| Vertex | Shorted Distance from A | Previous vertex |
    A                  0                    
    B                 INF
    C                 INF
    D                 INF
    E                 INF
    
- Pick A, and append to the `visitedList`. Find all adjacents of A and 
update the shortest distance from A to every adjacent.

visitedList = [A]

| Vertex | Shorted Distance from A | Previous vertex |
    A                  0                    
    B                  6                    A
    C                 INF
    D                  1                    A
    E                 INF

- Pick the vertex which currently has minimum distance from A. And now, it's D. 
Do the same as the previous step, now we update the adjacent vertices B and E.
A is also the adjacent of D, but the distance from A to A via D is not the minimum.

visitedList = [A, D]

| Vertex | Shorted Distance from A | Previous vertex |
    A                  0                    
    B                  3                    D
    C                 INF
    D                  1                    A
    E                  2                    D
    
- Now, pick E (E is not in the `visitedList` currently). 
Do the same as the previous step, now we update only C. D and B are also the adjacents of E 
but the distance from A to B and A to D via E is not the minimums.

visitedList = [A, D, E]

| Vertex | Shorted Distance from A | Previous vertex |
    A                  0                    
    B                  3                    D
    C                  7                    E
    D                  1                    A
    E                  2                    D

- Next, pick B. But this time we do not update any distance since 
the distance from A to all the adjacents of B via B is not the minimums.

visitedList = [A, D, E, B]

| Vertex | Shorted Distance from A | Previous vertex |
    A                  0                    
    B                  3                    D
    C                  7                    E
    D                  1                    A
    E                  2                    D
    
- Now the last one is C. We do not update any vertex now. 

visitedList = [A, D, E, B C]

And we are done because the `visitedList` now contains all vertices.

Now we know the shotest path from A to all vertices. And we also know the immeditate vertices for those paths.
- Shortest distance from A to B is 3 which is the path via D (A-D-B)
- Shortest distance from A to C is 7 which is the path via E - which is the path via D (recusively query in the table) (A-D-E-C)
- Shortest distance from A to D is 1 which is the direct path (A-D)
- Shortest distance from A to E is 2 which is the path via D (A-D-E)
```

### Code

```cpp

```
