## Floyd-Warshall Algorithm

Find shortest distances between every pair of vertices in a given weighted directed graph.
The graph is presented as Adjancency Matrix, the matrix denote the weight of the edges.

Input
```
2 // Number of tests
2 // Number of vertices
0 25 // The matrix
INF 0 // The matrix
3 // Number of vertices
0 1 43 // The matrix
1 0 6 // The matrix
INF INF 0 // The matrix
```

Output
```
0 25
INF 0 
0 1 7
1 0 6
INF INF 0 
```

### The Solution

Let us assume a graph with V nodes, and `dist[a][b]` initially represent 
the cost of the direct path from a to b.

We are going to consider all vertices as intermediate vertices. 
Starting from no intermediate vertex, then only 1 intermediate vertex 
then 2 intermediate vertices,... then V intermediate vertices.

***Step0***: No intermediate vertex: then the matrix will not be change (exactly the same with the original one)

***Step1***: 1 intermediate vertex, let say `k0`: update the matrix calculated from the step above with this rule: 
If `k0` is in the shortest path from vertex `i` to vertex `j` then the shortest distance from `i` to `j` must be equal
the sum of the shortest distance from `i` to `k` and the distance path from `k` to `j`. 
The shortest distance from `i` to `k` and `k` to `j` are already calculated from the previous step (don't forget that
we are considering `k0` as the only one intermediate vertex). After this step, the `dist` matrix now will tell us 
the shortest distance from any pair of vertices include (or not) the `k0` as an only intermediate vertex.

***Step2***: 2 intermediate vertices, let say this time the new one is k1 (k0 is from Step 1): 
Do exaclty the same as step 1 using the matrix computed from Step above. After this step, 
the `dist` matrix now will tell us the shortest distance from any pair of vertices 
include (or not) the `k1` as a new intermediate vertex (the old one is `k0`).

Repeat this until we finish considering all vertices (`k0`, `k1`....`kV`) as immediate vertices. 
The final `dist` matrix will tell us the shortest distance from any pair of vertices 
which includes all vertices as immediate vertices.

```cpp
using namespace std;

int inf = 10000000;

int main() {
  int numberOfTests;
  cin >> numberOfTests;

  for(int t=0; t<numberOfTests; t++) {
    int numberOfVertices;
    cin >> numberOfVertices;

    int dist[numberOfVertices][numberOfVertices];

    for(int i=0; i<numberOfVertices; i++) {
      for(int j=0; j<numberOfVertices; j++) {
        cin >> dist[i][j];
      }
    }
    
    for(int k=0; k<numberOfVertices; k++) {
      for(int i=0; i<numberOfVertices; i++) {
        for(int j=0; j<numberOfVertices; j++) {
          dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
        }
      }
    }
    
    for(int i=0; i<numberOfVertices; i++) {
      for(int j=0; j<numberOfVertices; j++) {
        if (dist[i][j] == inf) {
          cout << "INF" << " ";
        } else {
          cout << dist[i][j] << " ";
        }
      }
      cout << endl;
    }
  }
}
```
