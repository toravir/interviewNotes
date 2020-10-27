
Parent / Child Relation
```
  l,r := 2p+1, 2p+2
  p = (c-1)/2
```

Insert to heap
```
/*
  1. Insert at the end of the array (bottom right of tree)
  2. propagate upwards till max/min heap is satisfied
*/
   h.arr = append(h.arr, n)
   c := len(h.arr)-1
   for c > 0 {
       p:=parent(c)
       if h.arr[p] > h.arr[c] { break }
       h.arr[p], h.arr[c] = h.arr[c], h.arr[p]
       c = p
   }
```

Pop from Heap
```
/*
   Save the first element in the array - highest/smallest
   extract the last element from the array and put it on the first position
   walk down till heap property is satisfied
     -> always swap with the max or min of children..
*/

    r := h.arr[0]
    if len(h.arr) == 1 {
        h.arr = []int{}
        return r
    }
    l := h.arr[len(h.arr)-1]
    h.arr = h.arr[:len(h.arr)-1]
    p, h.arr[0] = 0, l
    for {
       c1, c2 := children(p)
       lgst := p
       if c1 < len(h.arr) && h.arr[lgst] < h.arr[c1] {
           lgst = c1
       }
       if c2 < len(h.arr) && h.arr[lgst] < h.arr[c2] {
           lgst = c2
       }
       if lgst != p {
          h.arr[lgst], h.arr[p] = h.arr[p], h.arr[lgst]
          p = lgst
       } else {
          break
       }
    }
```
