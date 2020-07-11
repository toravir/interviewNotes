

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
}
```

4. 

