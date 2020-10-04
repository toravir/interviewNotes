

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

4. Find the Duplicate Number (array of n+1 elements - each value 1..n - only number is repeated)

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

7. Finding minimum in a rotated sorted array (with dup)
```
/*
 * mid may be equal to right - in that case do (right--)
 */
 l, r := 0, len(nums)-1
 for l < r {
   m := l + (r-l)/2
   if nums[m] == nums[r] {
      r--
   } else if nums[m] > nums[r] {
      l=m+1
   } else {
      r=m     // NOT m-1
   }
} 
```

Largest rectangle in a histogram
```
/*
 * append a zero beginning and end of the arry
 * push first elem's index (0) into the stack
 * maxArea = 0
 *
 * while elements in heights:
 *    if element > topOfStack {
 *       push(element); go to next element
 *    } else {
 *       p = pop()
 *       while p != peek() { pop() }  // to handle multiple same height buildings
 *       thisArea = p * (current index - peek() -1)
 *       maxArea = MAX(thisArea, maxArea)
 *    }
 *    return maxArea
 */
     heights = append([]int{0}, heights...)
     heights = append(heights, 0)
     maxarea, i := 0, 1
     st.push(0)   // Store indices into the stack
     for i<len(heights) {
        tos := st.peek()
        if st.isEmpty() || heights[i] >= heights[tos] {
           st.push(i)
           i++
           continue
        }
        ev:=st.pop()
        for ev == st.peek() && !st.isEmpty() {
            st.pop()
        }
        thisarea := heights[ev]*(i - st.peek()-1)
        if thisarea > maxarea {
           maxarea = thisarea
        }
     }
     return maxarea
```

Manacher Algorithm (Longest Palindrome Substring)

```

```

K Nearest Elements from a sorted array

```
/*
 * do binary search - keep track of delta with middle - remember the smallest delta & index
 * move left and right to pickup k elements
 */
    l, r :=0, len(arr)-1
    closest, val := INF, -1 
    for l<=r {
       m:=l+(r-l)/2
       if abs(arr[m]-x) < closest {
          closest, val = abs(arr[m]-x), m
       }
       if arr[m] > x {
          r = m-1
       } else {
          l = m+1
       }
    }
    l, r = val, val+1
    for i:=0;i<k;i++ {
       ld, rd := INF, INF
       if l >= 0 {
          ld = abs(arr[l]-x)
       }
       if r <= len(arr)-1 {
          rd = abs(arr[r]-x)
       }
       if ld <= rd {
          l--
       } else {
          r++
       }
    }
    l++
    return arr[l:r]
```

H-Index
```
/*
 1. Find freq of papers with 0..n (citations greater than n is accounted under n)
 2. Walk from the back - summing papers & reducing i - the moment sum is greater than i, return i
 */

func hIndex(a []int) int {
    n:=len(a)
    freq := make([]int, n+1)
    for i:=0;i<n;i++ {
       freq[min(a[i], n)] += 1
    }
    tot:=0
    for i:=n;i>=0;i-- {
       tot += freq[i]
       if tot >= i {
	  return i
       }
    }
    return 0
}
```
Rotate array Inplace
```
array of len (n)
num of rotations (r)
r = r % n   // since after n rotations array is restored

Brute force: move one elem at a time O(n*r)
Efficient: O(n) - find GCD(n, r)
for i:=0;i<gcd(n,r);i++
   save and a[i], 
       a[i] = a[(i+r)%n] .... until loop is complete then next i...

   n := int32(len(a))
   r = r % n
   iti := GCD(n, r)
   for i:=int32(0); i<iti; i++ {
       t, dst:=a[i], i
       for {
           a[dst] = a[(dst+d)%n]
           dst = (dst+d)%n
           if (dst+d)%n == i { // end of loop
               a[dst]=t
               break
           }}}
   return a

```

Search in a Rotated Array
```
/*
1. do binary search
2. if left half is sorted
   and if target is in left - go left
   else go right
3. right must be sorted
   if target is on the right, go right
   else go left

    for l<=r {
        m:=l+(r-l)/2
        if nums[m] == target { return m }
        if nums[l] <= nums[m] {
            if nums[l] <= target && target <= nums[m] {
                r = m-1
            } else {
                l = m+1
            }
        } else {
            if nums[m] <= target && target <= nums[r] {
                l = m+1
            } else {
                r = m-1
            }
	}}
*/
```

Find a number that appears more than n/3 times

```
/*
Have two candidates - (val, count)

foreach element:
   if value matches a candidate - incr counter
   else if count is zero pickup value into the candidate
   else decr count for both candidates

We have two candidates that can exceed n/3

Walk the list and find both candidate counts
verify if the count is indeed > n/3.

*/
    c1, v1 := 0, 0
    c2, v2 := 0, 0
    for _, v := range nums {
        if v == v1 {
            c1++
        } else if v == v2 {
            c2++
        } else if c1 == 0 {
            v1, c1 = v, 1
        } else if c2 == 0 {
            v2, c2 = v, 1
        } else {
            c1, c2 = c1-1, c2-1
        }
    }
    c1, c2 = 0, 0 
    for _, v := range nums {
        if v == v1 {
            c1++
        } else if v == v2 {
            c2++
        }
    }
    ans := []int{}
    if c1 > len(nums)/3 { ans = append(ans, v1) }
    if c2 > len(nums)/3 { ans = append(ans, v2) }
    return ans
```

MAX Abs Difference

```
/*
Given an Array find max value of f(i,j) = |A[i]-A[j]| + |i-j|

keep track of min and max as you go - adjust for diff in indices
if cmax+(i-imax) < A[i] { cmax, imax = A[i], i }
if cmin-(i-imin) > A[i] { cmin, imin = A[i], i }
*/
    fn := func (i, j int) int { return abs(A[i]-A[j])+abs(i-j) }
    for i:=1;i<len(A);i++ {
        if cmin-(i-imin) > A[i] {
            imin, cmin = i, A[i]
        } else if cmax+(i-imax) < A[i] {
            imax, cmax = i, A[i]
        }
        tv := max(fn(i, imin), fn(i, imax))
        if fv < tv { fv = tv }
    }

return fv
```


