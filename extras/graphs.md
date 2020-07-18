1. Toposort - Kahn's Algorithm

```
2. Kahn's Algorithm

result = []
S = Set of all nodes with no incoming edge

while S is not empty do:
    remove a node n from S
    add n to tail of result
    foreach child m node of n do:
       remove edge e (m->n) from the graph
       if m has no other incoming edges then
          insert m to S
    if graph has edges then
       LOOP DETECTED
    else
       return result
```
