Quicksort
```
/*
   1. partition the array to fix location of pivot
      take a pivot elem (usually the last elem)
      two pointers (i, j) - both start at index 0
      if arr[j] < pivot { swap (arr[i], arr[j]}; i++ }
 
      After the end, move pivot with i.
   2. recurse left half and right half
*/

func partition (arr []int) int {
    i, piv := 0, arr[len(arr)-1]
    for j:=0;j<len(arr)-1;j++ {
        if arr[j] <= piv {
            arr[i], arr[j], i = arr[j], arr[i], i+1
    } }
    arr[i], arr[len(arr)-1] = arr[len(arr)-1], arr[i]
    return i
}
func quicksort (arr []int) {
    if len(arr) < 2 { return }
    p := partition(arr)
    quicksort(arr[:p])
    quicksort(arr[p+1:])
}

```
