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

Number of Nodes in a Complete Binary Tree (not Perfect Binary Tree - 2^n -1)

```
/*
 * check the left height (left->left->left...) and right height (right->right->right...)
 * if equal - we have a perfect binary tree - if not - check children - re-use lh and rh
 */
func helper (root *TreeNode, lh int, rh int) int {
	if root == nil { return 0 }
	if lh == -1 {
		lh = leftHt(root)  // this walks all Left till nil
	}
	if rh == -1 {
		rh = rightHt(root)  // this walks all Right till nil
	}
	if (lh == rh) {
		return (1 << lh)-1
	}
	return helper(root.Left, lh-1, -1) + helper(root.Right, -1, rh-1) + 1
}
```

Construct Tree from Pre and Post Order (is NOT always unique tree)
```

func constructFromPrePost(pre []int, post []int) *TreeNode {
    if len(pre) == 0 { return nil }
    if len(pre) == 1 { return &TreeNode{Val: pre[0]}}
    this := &TreeNode{Val:pre[0]}
    lpre, rpre := []int {}, []int {}
    lpos, rpos := []int {}, []int {}
    isleft := true
    for i:=1;i<len(pre);i++ {
        r, o := pre[i], post[i-1]
        if isleft {
            lpre = append(lpre, r)
            lpos = append(lpos, o)
        } else {
            rpre = append(rpre, r)
            rpos = append(rpos, o)
        }
        if o == pre[1] {
            isleft = false
        }
    }
    this.Left  = constructFromPrePost(lpre, lpos)
    this.Right = constructFromPrePost(rpre, rpos)
    return this
}
```

Least Common Ancestor of two nodes (used to find distance between two nodes)
```
func LCA (root, n1, n2):
  if root == nil {return nil}
  if root.val == n1 || root.val == n2 {
     return root
  }
  l := LCA(root.Left, n1, n2)
  r := LCA(root.Right, n1, n2)
  if l != nil && r != nil {
     return root // this node is the LCA
  }
  if l != nil { return l }
  return r
```

Binary Indexed Tree
```
type NumArray struct { tree []int }
func Constructor(nums []int) NumArray {
    t := NumArray{make([]int, len(nums)+1)}
    t.tree[0] = 0
    copy(t.tree[1:], nums)
    for i:=1;i<len(t.tree);i++ {
        p:=i+(i & -i )
        if p < len(t.tree) { t.tree[p] += t.tree[i] }

func (this *NumArray) prefixSum (i int) int {
   ni, sum := i+1, 0
   for ni > 0 {
       sum += this.tree[ni]
       ni -= (ni & -ni)
   }
   return sum

func (this *NumArray) Update(i int, val int)  {
    ov := this.prefixSum(i) - this.prefixSum(i-1)
    ni, delta := i+1, val - ov
    for ni < len(this.tree) {
        this.tree[ni] += delta
        ni += (ni & -ni)

func (this *NumArray) SumRange(i int, j int) int {
    return this.prefixSum(j) - this.prefixSum(i-1)
}

```


