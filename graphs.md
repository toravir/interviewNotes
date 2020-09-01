

V - number of Vertices
E - number of Edges
Y - number of cycles

Number of different spanning Trees = (eC(v-1) - Y) ---> 
     number of edges is e and you are choosing v-1 - #cycles has to be removed
    

Minimum Spanning Tree
```
Prims Algo:
1. Start with lowest cost edge
2. Add the next lowest cost edge that is also connected to existing MST
3. Do #2 until all Nodes are added to MST


Kruskals Algo:
1. Start with the lowest cost edge
2. Get the next lowest cost Edge that does NOT introduce a cycle to existing MST
   (Even if it is disconnected from existing MST)
3. Do #2 until all nodes are added to MST
#Use MinHeap to reduce complexity to O(nloge)   
```

Longest Path - multiply weights by -1 and find shortest path 
               and restore the weights and result
Shortest Path
```
//One starting node, find shortest paths to all nodes (can stop in the middle)
Djikstra's Algo - CANNOT HANDLE -ve edges (unreliable) - Greedy approach

1. init - dist[src] = 0
2. create a vertex priority Q
3. for each vertex v in Graph:
     if v != src
         dist[v] = infinity
         pred[v] = undefined //predecessor is undef
     Q.add(v, dist[v])
4. while Q is not empty:
      u = Q.extract_min()
      for each neighbor v of u: // v is still in Q
         newd = dist[u]+weight(u,v)
         if dist[v] > newd:
            dist[v] = newd
            pred[v] = u
            Q.decrease_priority(v, newd) // See optimization in #6
            
5. //Dist has distances and //pred can be used to walk back
6. optimization - add a dupl entry with v and newd into PQ since
   - decrease priority is not easy - sometimes O(N) vs O(lg N) for insert

Bellman Ford - handles -ve edges - used dynamic Programming approach
O(n^3)

1. for each vertext v in vertices:
      dist[v] = infinity
      pred[v] = undefined
2. for i := 1 ... len(vertices)-1:
     for each edge(u,v):   // we run this loop V-1 times (V = num of Vertices)
        if dist[v] > dist[u]+weight(u,v):
           dist[v] = dist[u]+weight(u,v)
           pred[v] = u
3. for each edge (u,v):
      if dist[v] > dist[u] + weight(u,v)
         ERROR ("Graph has -ve weight cycle - so it will keep spinning to minimize")
```

1. Topological Sort - ordering to satisfy dependencies

```
1. DFS Approach

result = []
while exists nde without a perm mark do:
   select an unmarked node n
   visit(n)
return result

def visit(n) :
   if n has perm mark:
      return
   if n has temp mark:
      loop detected
   mark n with temp mark
   for each child node m of n do:
       visit(m)
   remove temp mark from n
   set perm mark on n
   add n to head of result

```
