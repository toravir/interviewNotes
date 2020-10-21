  
1. All possible combinations of coins to a target value
```
/*
 * Reasoning: TBD
 *
 *
 */
    dp := make([]int, amount+1)
    dp[0]=1
    for _, coin := range coins {
        for i:=coin; i<amount+1; i++ {
            dp[i] += dp[i-coin]
        }
    }
    return dp[amount]
```


2. Two City Scheduling - minimize total cost of flying each candidate to one of two cities.
Equal numbers to each city.
```
/*
 * Reasoning: Send each candidate to one or other - pick whichever is min
 * Capture the total number to each city by keeping track of how many to one city (beyond which costs will be MAX)
 *
 * use a cache to reduce repeated calls
 */
    var dpf func(int, int) int
    cache := make(map[int]int,0)

    dpf = func (i, counta int) int {
        if i>=len(costs)/2 && counta >len(costs)/2 {
            return 9999999999
        }
        if i == len(costs) {
            if counta == len(costs)/2 { return 0 }
            return 9999999999
        }
        a,b:=0,0
        if v, ok := cache[(i+1)*1000+counta+1]; ok {
            a = costs[i][0]+v
        } else {
            a = dpf(i+1, counta+1)
            cache[(i+1)*1000+counta+1] = a
            a+=costs[i][0]
        }

        if v, ok := cache[(i+1)*1000+counta]; ok {
            b = costs[i][1]+v
        } else {
            b = dpf(i+1, counta)
            cache[(i+1)*1000+counta] = b
            b+=costs[i][1]
        }
        if a < b { return a }
        return b
    }
    return dpf(0,0)
```

3. Perfect Squares - given a number n, find the least number of squares add up to n
```
/*
 * for each number i,
 *     iterate j: 1... Sqrt(i)
 *        set dp[i] to the lowest dp[i-(j*j)]+1
 */
    dp := make([]int, n+1)
    for i:=0;i<=n;i++ {
        dp[i] = math.MaxInt32
    }
    dp[0], dp[1]=0, 1
    for i:=2;i<=n;i++ {
        for j:=1; j*j <= i; j++ {
            if dp[i] > (dp[i-(j*j)]+1) {
                dp[i] = dp[i-(j*j)]+1
            }
        }
    }
    return dp[n]
```

4. Longest Increasing Subsequence (or Largest Divisible subset)
```
/*
 * 1. Sort optional
 * 2. DP array to keep the largest divisible subset size so far (initial value of 1)
 * 3. for x = 1..i-1 { if arr[i]%arr[x] == 0 then dp[i] = max(dp[i], dp[x]+1) }
 * 4. //We know the largest set size in DP
 * 5. Walk back dp array and see when size bumped up and record those numbers
 * 6. additional check for divisibility (coz if there are more than one same size set - need to pick the right seq)
 */
 func largestDivisibleSubset(nums []int) []int {
    sort.Ints(nums)
    dp := make([]int, len(nums))
    for i:=0;i<len(dp);i++ {
        dp[i]=1
    }
    max := 1
    for i:=0;i<len(nums);i++ {
        for j:=0;j<i;j++ {
            if nums[i]%nums[j] == 0 {
                if dp[i] < dp[j]+1 { dp[i] = dp[j]+1 }
            }
        }
        if max < dp[i] { max = dp[i] }
    }
    prev := 0
    result := []int{}
    for i:=len(dp)-1; i >= 0; i-- {
        if dp[i]==max && (len(result) == 0 || nums[prev]%nums[i] == 0) {
            prev = i
            max--
            result = append(result, nums[i])
        }
    }
    return result
} 
```

5. Cheapest Flight with K stops
```
/*
 * dp array of hops vs dst
 *
 * dp[hop][dst] = min (dp[hop][dst], dp[hop-1][src]+flight_cost_from_src_to_dst)
 *
 */
func findCheapestPrice(n int, flights [][]int, src int, dst int, K int) int {
    max := 9999999999
    dp := make([][]int, K+2)
    for i:=0; i< len(dp); i++ {
        dp[i] = make([]int, n)
        for j:=0;j<n;j++ {
            dp[i][j] = max
        }
    }
    dp[0][src] = 0
    for i:=1;i<len(dp);i++ {
        dp[i][src] = 0  // cost is zero for any hops from src to src
        for _, flt := range flights {
            s,d,c := flt[0], flt[1], flt[2]
            dp[i][d] = min(dp[i][d], dp[i-1][s]+c)
        }
    }
    if dp[K+1][dst] == max { return -1 }
    return dp[K+1][dst]
}
```

Stock Buy/Hold/Sell/Cooldown:
```
/*
 * you can buy or sell or hold stock. when you sell - u have to wait a day before buying again
 * 
 * Logic:
   bought = MAX (hold already bought, new buy)
   sold   = MAX (sold, wait)
   wait   = selling what was bought (in prev iteration)
 return MAX (sold, wait)
 */
   sold, bought, wait := 0, -prices[0], -9999999
   for i:=1;i<len(prices);i++ {
     pbought := bought
     bought = MAX(bought, sold - prices[i])
     sold = MAX(sold, wait)
     wait = pbought+prices[i]
   }
   return MAX(sold, wait)
```
Longest Common Subsequence (also for longest palindrome subseqence)

```
/*
 * dp - 2d array of size m+1 x n+1 (first row and first col is all zeros)
 *
 * if (s1[i] == s2[j])
 *     dp[i][j] = dp[i-1][j-1]+1 
 * else
 *     dp[i][j] = MAX(dp[i-1][j], dp[i][j-1])
 *
 * LCS of string and reverse of string is the longest palindrome subsequence
 */

    m:=len(text1)
    n:=len(text2)
    dp:=make([][]int, n+1)
    for i:=0;i<len(dp);i++ {
       dp[i]=make([]int, m+1)
    }
    for i:=0;i<n;i++ {
       for j:=0; j<m;j++ {
           if text2[i] == text1[j] {
              dp[i+1][j+1] = dp[i][j]+1
           } else {
              dp[i+1][j+1] = MAX(dp[i][j+1], dp[i+1][j])
           }
       }
    }
    //fmt.Println(dp)
    return dp[n][m]
```

Longest Palindrome SUBSTRING

```
/* Explore substrings of len 1..n
 *  For each length check various starting points 0...n
 *   given start and end indices - 
 *      if s[start] == s[end] && s[start+1:end-] is a palindrome
 *          expand
 *      else
 *          mark it as non-palindrome
 * remember the largest palindrome and start and end indices
 */
    maxl := 0
    maxst, maxen := 0, 0
    for ln:=1; ln<=n; ln++ {
      for st:=0; st < n; st++ {
         en:=st+ln-1
         if en > n-1 { break }
         if en == st {
            dp[st][en] = 1
         } else {
            // If st & end match and pre. substring is also palindrome - expand...
            if s[st] == s[en] && dp[st+1][en-1] >= 0 {
               dp[st][en] = dp[st+1][en-1]+2
               if maxl < dp[st][en] {
                  maxl, maxst, maxen = dp[st][en], st, en
               }
            } else { dp[st][en] = -1 }
         }
      }
    }
```

Stock/Buy Sell - K transactions

```
/*
1. if k > n/2 (we can buy and sell whenever) - so max prof is sum of all increase in prices
2. Setup DP - two arrays : bought[] and sold[] of size k
// For each price - price[i] go through the entire bought/sold arrays
for each price[i]
   bought[0] = max(bought[0], -price[i])  // day 0 we are down on money
   sold[0] = max(sold[0], bought[0]+price[i])
   for j=1..k
     bought[j] = max(bought[j], sold[j-1]-price[i])
     sold[j]   = max(sold[j], bought[j]+price[i])

sold[k-1] will have the final answer
*/
    if k > len(prices)/2 {
        prof:=0
        for i:=0;i<len(prices)-1;i++ {
            if prices[i]<prices[i+1] {
                prof+=prices[i+1]-prices[i]
            }
        }
        return prof
    }
  buy := make([]int, k)
  sell := make([]int, k)    
  for i,_ := range buy {
       buy[i] = MIN_INTEGER
  }
  for i:=0;i<len(prices);i++ {
    buy[0]  = maxOf(buy[0], -prices[i])
    sell[0] = maxOf(sell[0], buy[0]+prices[i])
    for j:=0; j < k; j++ {
        buy[j]  = maxOf(buy[j], sell[j-1]-prices[i])
        sell[j] = maxOf(sell[j], buy[j]+prices[i])
      }
  }
  return sell[k-1]
```

Longest Palindromic Substring
```
/*
Core: substring A[x...y] is palindrome if A[x]==A[y] and A[x+1...y-1] is a palindrome
Base: dp[x][x] is true (palindrome)... + dp[x][x+1] is true if A[x]==A[x+1]

1.fill the diagonal [i][i] and [i][i+1]
2. for varying lengths ranging from 3..n (aka indices 2..n-1)
     for each starting position 0..n-j
        dp[i][i+j] = A[i]==A[i+j] && dp[i+1][i+j-1]
*/
     n, ba, dp := len(A), []byte(A), make([][]bool, n)
     for i:=0;i<n;i++ {
        dp[i]=make([]bool, n)
     }
     for i:=0;i<n;i++ {
         dp[i][i]=true
     }
     ans:=string(A[0:1])
     for i:=0;i<n-1;i++ {
        if A[i]==A[i+1] {
            dp[i][i+1]=true
            ans=string(A[i:i+2])
        }
     }
     for j:=2;j<n;j++ {
        for i:=0;i<n-j;i++ {
            dp[i][i+j] = dp[i+1][i+j-1] && A[i] == A[i+j]
            if dp[i][i+j] && j > len(ans)-1 {
               ans=string(ba[i:i+j+1])
            }
        }
     }
```

Regular expression matching - '* - matches with any characters'
```
   i - index of str, j - index of pat
   if str[i-1] == pat[j-1] || pat[j-1] == '?'
       dp[i][j] = dp[i-1][j-1]
   if pat[j-1] == '*'
       dp[i][j] = dp[i-1][j] || dp[i][j-1]   // consume ith char or not
```

Minimum edit distance between two strings - opers:
1. modify one char,
2. add one char
3. del one char

```
   for i:=0;i<len(A)+1;i++
      for j:=0;j<len(B)+1;j++
         if i == 0 || j == 0 { 
            dp[i][j] = i + j
            continue
         }
         if A[i-1] == B[j-1] { 
            dp[i][j] = dp[i-1][j-1]
         } else {
            dp[i][j] = 1 + min (dp[i-1][j-1], dp[i-1][j], dp[i][j-1])
         }
```

Number of subarrays that sub to k
items can be +ve or -ve
```
/*
  init map[int]int
  iterate the array from l to r
     accumulate the sum in a var say csum
     if csum == k { count ++ }
     if map[k-csum] is found { count += value in map[k-csum] }
     map[csum] += 1
  return count
*/
    for i:=0;i<len(nums);i++ {
        sum+=nums[i]
        if sum == k { count ++ }
        if v, ok := tbl[sum-k]; ok { count+=v }
        if v, ok := tbl[sum]; ok { tbl[sum] = v+1 } else { tbl[sum]=1 }
    }
```

Number of Submatrices that add up to k

```
/* extension of the sub-array sum to k
   precompute cumu - sum of all columns - ie  precomp[i][j] = SUM(A[0..i][j])
   for each sr and end row - compute the values along the column - and see how many subarrays result in sum k   
*/
    csum := make([][]int, nr+1)
    for i:=0;i<nr+1;i++ {
       csum[i] = make([]int, nc)
    }
    for i:=1;i<nr+1;i++ {
       for j:=0;j<nc;j++ {
          csum[i][j] = A[i-1][j]+csum[i-1][j]
       }
    }
    ans := 0
    for sr:=1;sr<=nr;sr++ {
       for er:=sr; er<nr+1; er++ {
           tbl, cs :=map[int]int{}, 0
           for j:=0;j<nc;j++ {
               cs += csum[er][j]-csum[sr-1][j]
               if cs == k { ans ++ }
               if v, ok := tbl[cs-k]; ok { ans += v }
               if v, ok := tbl[cs]; ok { tbl[cs]=v+1 } else {tbl[cs]=1}
           }
       }
    }
}
```

Ways to decode a string - no wild cards
```
   /*
      if A[0] == '0' { return 0 } // cannot start with '0'
      dp[0], dp[1] = 1, 1
      if A[i] > '0' { dp[i] += dp[i-1] }
      if A[i-1]=='1' || (A[i-1]=='2' && A[i] <= '7') { dp[i] += dp[i-2] }
   */
   

```
