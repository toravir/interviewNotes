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
