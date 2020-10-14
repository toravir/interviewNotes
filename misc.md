
1. Modulo

```
(a+b) mod c = ((a mod c) + (b mod c)) mod c
(a-b) mod c = ((a mod c) - (b mod c)) mod c
(a*b) mod c = ((a mod c) * (b mod c)) mod c

a^2 mod c = ((a mod c) * (a mod c)) mod c
a^4 mod c = ((a^2 mod c) * (a^2 mod c)) mod c  <<<-- reuse prev value here
..
..

a^n mod c -> split n to binary and use above logic to get individual
submodulo and multiply them together.

Modular Inverse:

a * x = 1 mod c --> x is modulo c inverse of a

modulo inverse exists ONLY IF a and c are co-prime (no common prime factors)
```

2. GCD

```
Euclidean Algorithm (sub-problem):
   1. GCD(A,0) = A
   2. A = B*Q + R, GCD (A,B) = GCD(B, R),   Q is integer, R is (0..B-1)

```

3. XOR

```
1. SWAP Two numbers using XOR
a = a ^ b     ;;   b = a ^ b     ;;    a = a ^ b
Proof: a = (a ^ b),   b = (a ^ b) ^ b = a ;;    a = (a ^ b) ^ a == b


2. All numbers occurs 3 times and one number occurs once
ones = twos = 0

foreach n {
    twos = twos | (ones & n)   --> find the common bits from ones and n - add them into twos
    ones = ones ^ n            --> This is normal tracking of odd occurance numbers
    threes: = ~(ones & twos)   --> find bits common to ones and twos - these occur three times - we don't care
    ones &= threes             --> remove three bits from ones
    twos &= threes             --> remove three bits from twos
}
return ones

```

Permutations and Combinations
```
nPr = (n!)/(n-r)!
nCr = (n!)/((n-r)!r!)

Pascal's Triangle - each row k is:
kC0, kC1, kC2, ... kCk

```

Calculate nCr for Large Values
```
func nCr (n, r int) int {
    num, den := 1, 1
    if n - r < r { r = n - r }
    if r == 0 { return 1 }
    for r > 0 {
        num, den = num*n, den*r
        g := gcd(num, den)
        num, den = num/g, den/g
        n, r = n-1, r-1
    }
    return num/den
}
```

Transform the array - so A[i] => A[A[i]], given 0 <= A[i] <= N-1

```
/*
Store two numbers in one place a & b can be stored if
a + b*n is stored. A[i]%n = a, A[i]/n = b, since 0 <= a,b <= N-1
*/
    int i = 0;
    for (i=0; i<n1;i++) {
        A[i] += (A[A[i]]%n1)*n1;  // need a %n1 - because A[A[i]] could already be encoded
    }
    for (i=0;i<n1;i++) {
        A[i] = A[i]/n1;
    }
```

Sieve of Prime Numbers (until N)
```
#1: Eratosthenes
/*
Start with an array A[0..N]- mark it all true
for i=2 .. N:
  if A[i] == True:
     Add i to list of primes
     mark all multiples of i starting from i*i till N as false
*/

#2: Sieve of Sundaram
/*
Create an array of A[0..(n-1)/2+1] - fill it will true
for i:=1;i<=(n-1)/2;i++ {
   for j:=1 ; (i+j+2ij) < (n-1)/2 ; j++ {
      A[i+j+2*i*j] = false
   }
}
primesList = {2}
for i:=1;i<=(n-1)/2;i++ {
   if A[i] == true {
      primesList += {2*i+1}
   }
}
*/
    t := (n-1)/2 + 1
    cand := make([]bool, t)
    for i:=0;i<t;i++ { cand[i] = true }
    for i:=1;i<t;i++ {
        for j:=i; (i+j+2*i*j) <t;j++ {
            cand[i+j+2*i*j] = false
        }
    }
    ans := []int{2}
    for i:=1;i<t;i++ {
        if cand[i] { ans = append(ans, 2*i + 1) }
    }
```


To get fraction - A/B -
```
/*
    1. whole number = A/B (integer division)
    2. To get the fractional part - do this - start with
       remainder = A % B
       for remainder > 0 {
           remainder *= 10
           nxtdigit = remainder/B
           remainder %= B
       }
    3. If the same remainder is seen - then the digits repeat - so keep a 
       map of what remainder was at each position in the fractional part
*/

func fractionToDecimal(A int , B int )  (string) {
     if A == 0 { return "0" }
     ans:=""
     if (A<0) != (B<0) {
         ans+="-"
     }
     A, B = abs(A), abs(B)
     div, rem :=A/B, A%B
     ans+=strconv.Itoa(div)
     dec, tbl, dotted := "", map[int]int{}, false
     v, i := -1, 0
     var ok bool
     for rem > 0 {
         if v, ok = tbl[rem]; ok { break }
         tbl[rem]=i
         rem *= 10
         if !dotted { 
             dec += "."
             dotted = true
         }
         dec+=string([]byte{'0'+byte(rem/B)})
         i++
         rem %= B
     }
     if rem == 0 { return ans+dec }
     return ans + dec[:v+1] + "(" + dec[v+1:] + ")"
}

```

