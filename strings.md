
KMP Algorithm
```
func prepro(p string) []int {
   pfx := make([]int, len(p))
   for i,j:=1,0; i <len(p); {
      if p[i] == p[j] {
         pfx[i] = j+1
         i,j = i+1, j+1
      } else {
         if j != 0 {
            j = pfx[j-1]
         } else {
           pfx[i], i = 0, i+1
         }
      }
   }
   return pfx
}

func strstr(s string, p string) bool {
   pfx := prepro(p)
   j:=0
   for i:=0;i<len(s)&&j<len(p); {
      if s[i] == p[j] {
         i++
         j++
      } else {
         if j != 0 {
            j = pfx[j-1]
         } else {
            i++
         }
      }
   }
   return j == len(p)
}
```


