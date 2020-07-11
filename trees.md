1. Max width of a binary tree (width is distance between leftmost to right most node at each level - incl missing nodes)

```
/*
 * do breadth-first traversal - each node give it a number, 
 * if root is n, set left child to 2n-1 and right child to 2n
 * compute the diff between rightmost and leftmost and keep track of
 * largest difference
 */
    type tnWithLoc struct {
        tn *TreeNode
        loc int
    }
    if root == nil {
        return 0
    }
    curQ := make([]tnWithLoc, 0)
    nxtQ := []tnWithLoc{}
    curQ = append(curQ, tnWithLoc{root,1})
    res := 1
    for {
        nxtQ = make([]tnWithLoc, 0)
        li, ri :=-1, 0
        for _,v := range(curQ) {
            if v.tn.Left != nil {
                if li == -1 {
                    li = 2*v.loc-1
                }
                nxtQ = append(nxtQ, tnWithLoc{v.tn.Left, 2*v.loc-1})
                ri = 2*v.loc-1
            }
            if v.tn.Right != nil {
                if li == -1 {
                    li = 2*v.loc
                }
                nxtQ = append(nxtQ, tnWithLoc{v.tn.Right, 2*v.loc})
                ri = 2*v.loc
            }
        }
        if li == -1 {
            break
        }
        if res < (ri - li +1) {
            res = ri-li+1
        }
        curQ = nxtQ
    }
    return res
```


