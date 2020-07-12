

1. sorting two item array
```
  //0s and 1s
	l,r:=0,len(arr)-1
  for l < r {
      if arr[l] == 1 {
         arr.swap(l, r)
         r--
      } else {
         l++
      }
  }
```


2. Sorting three item array - 3 pointers...
```
//0s, 1s and 2s
l, r:=0, len(arr)-1
for i:=0;i<len(arr); {
    if arr[i] == 2 {
       arr.swap(i, r)
       r--
    } else if arr[i] == 0 {
       arr.swap(i, l)
       i++
       l++
    } else {
       i++
    }
}
```

3. Binary Search / Insert
```
    l := 0
    r := len(nums)-1
    for l < r {
       m := l + (r-l)/2
       if search > nums[m] {
          l = m+1
       } else if search < nums[m] {
          r = m-1
       } else {
           return m
       }
    }
    if nums[l] >= search {
       return l
    }
    return l+1
```

4. Find the Duplicate Number (all but one number is repeated)

```
/*
 * Slow/Fast pointer - use the index as next pointer
 *
 * once slow & fast meet, to find the start of the loop - move slow to start of the loop
 * and move one at a time until slow & fast meet again (which is at the start of the loop)
 */
    s,f := 0,0
    for {
        s, f = nums[s], nums[nums[f]]
        if (s == f) {
            break
        }
    }
    s = 0  // slow goes back to start of the list
    for {
        s,f = nums[s], nums[f]
        if (s == f) {
            return s
        }
    }
```

5. Prison Cell - each iteration - each cell's value is 1 if left cell == right cell (both occupied or vacant)
Width is 8 cells - leftmost and rightmost become zero after the first iteration and stay that way.
```
/*
 * Key is the pattern repeats - for 8 cells, it repeats after 14 (15th is same as 1st)
 * use Algebra to find when it repeats
 * Iteration #n:  |  c1  |   c2  |   c3  |   c4  |   c5  |   c6  |   c7  | c8  |
 * Iteration #n+1:|   0  |1^c1^c3|1^c2^c4|1^c3^c5|1^c4^c6|1^c5^c7|1^c8^c8|  0  |
 *           ...
 *           ...
 *                |  c1  |   c2  |   c3  |   c4  |   c5  |   c6  |   c7  | c8  |
 */
```

6. 3 Sum - given an array of numbers and target num find all combinations of 3 numbers that add upto target num
```
/*
 * Sort the input array
 * for i = 0 ... arr[n-3] {
 *    // check and ignore dup values
 *    set left is i+1, right as n-1
 *    while l < r {
 *        if num[i]+num[l]+num[r] == target // found a solution
 *            > target // move right pointer
 *            < target // move left pointer
 *    }
 * }
 */
    ret := make([][]int, 0)
    sort.Ints(nums)
    for i:=0;i<len(nums)-2;i++ {
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }
        l, r :=i+1, len(nums)-1
        for l<r {
            sum := nums[i]+nums[l]+nums[r]
            if sum == 0 {
                ret = append(ret, []int{nums[i],nums[l],nums[r]})
                for l<r && nums[l] == nums[l+1] { // to avoid dups
                    l++
                }
                for l<r && nums[r] == nums[r-1] { // to avoid dups
                    r--
                }
                l++
                r--
            }
            if sum > 0 {
                r--
            } else if sum < 0 {
                l++
            }
        }
    }
    return ret 
```


