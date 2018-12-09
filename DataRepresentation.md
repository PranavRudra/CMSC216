# Data Representation

## Real Numbers

- `(-1)^s * m * r^e`
- r is the radix or base
- s is the sign, 0 for positive and 1 for negative
- m is the mantissa, which represents a number between 0 and 1
- e is the exponent (bias of 127 for floats and 1024 for doubles)
- 32 bit floating point number has 1 sign bit, 8 exponents bits, 23 mantissa bits

```C
    // e.g. how would 243.59375 be stored in binary?
    
    // sign bit should be 0 since number is positive
    // 243 in base-10 is 1111 0011 in binary
    // .59375 in base-10 is .10011 in binary
    // now, mantissa is 1110 0111 0011 in binary (rightmost 1 excluded)
    // adding enough zeroes to the right to give us 23-bit mantissa then gives us 1110 0111 0011 0000 0000 000 
    // exponent is 7 (number of spaces we moved decimal to the right) + 127 (bias) = 134 in base-10 or 1000 0110
    
    // thus we have 0 1000 0110 1110 0111 0011 0000 0000 000 (sign, exponent, mantissa order)
```