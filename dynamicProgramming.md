
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
            if counta == len(costs)/2 {
                return 0
            }
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
        if a < b {
            return a
        }
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
                if dp[i] < dp[j]+1 {
                    dp[i] = dp[j]+1
                }
            }
        }
        if max < dp[i] {
            max = dp[i]
        }
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
    if dp[K+1][dst] == max {
        return -1
    }
    return dp[K+1][dst]
}
```

