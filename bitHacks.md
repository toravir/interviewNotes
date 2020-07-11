
1. Clear the rightmost (set) bit - can be used to see if a number is power of 2
```
x = x & (x-1)
```

2. counting bigs in parallel

```
unsigned int v; // count bits set in this (32-bit value)
unsigned int c; // store the total here
static const int B[] = {0x55555555, 0x33333333, 0x0F0F0F0F, 0x00FF00FF, 0x0000FFFF};

c = v - ((v >> 1) & B[0]);  // count 2 bits at a time
c = ((c >> 2) & B[1]) + (c & B[1]); // count 4 bits at a time - two sums
c = ((c >> 4) + c) & B[2];  // count 4 bits at a time
c = ((c >> 8) + c) & B[3]; // count 8 bits
c = ((c >> 16) + c) & B[4]; // count 16 bits
```

3. Swapping values with ```+ and - or ^```
```
#define SWAP(a,b) ( \
                    ((&a) == (&b)) || \
                    ( ((a) -= (b)), ((b)+=(a)), ((a) = (b) - (a)) )
                    
#define SWAP(a,b) ( ((a)^=(b)), ((b)^=(a)), ((a)^=(b)) )

```

4. Reverse N-bits
```
unsigned int v; // 32-bit word to reverse bit order

// swap odd and even bits
v = ((v >> 1) & 0x55555555) | ((v & 0x55555555) << 1);
// swap consecutive pairs
v = ((v >> 2) & 0x33333333) | ((v & 0x33333333) << 2);
// swap nibbles ... 
v = ((v >> 4) & 0x0F0F0F0F) | ((v & 0x0F0F0F0F) << 4);
// swap bytes
v = ((v >> 8) & 0x00FF00FF) | ((v & 0x00FF00FF) << 8);
// swap 2-byte long pairs
v = ( v >> 16             ) | ( v               << 16);
```

5. Count the consecutive zero bits (trailer)
```
unsigned int v;      // 32-bit word input to count zero bits on right
unsigned int c = 32; // c will be the number of zero bits on the right
v &= -signed(v);
if (v) c--;
if (v & 0x0000FFFF) c -= 16;
if (v & 0x00FF00FF) c -= 8;
if (v & 0x0F0F0F0F) c -= 4;
if (v & 0x33333333) c -= 2;
if (v & 0x55555555) c -= 1;
```


