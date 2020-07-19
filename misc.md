
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

