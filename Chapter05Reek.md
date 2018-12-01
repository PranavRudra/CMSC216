# Chapter 5: Operators

## Unary Operators

### sizeof

- returns number of bytes of expression or type WITHOUT evaluating it
- returns size_t (which may not be an int) so casting is good practice

```C
    char c = 'a';
    int a = 3;
    printf("%d %d\n", sizeof(int), sizeof(a++));    // prints 4 4 on machine with 4-byte integers (a++ NOT executed)
    printf("%d\n", (int) sizeof(c));                // casting is best practice since siz_t may not be an int
```

## Equity, Relational, Logical Operators

- && and || short-circuit evaluation are used just like in Java

## Conditional (Ternary) Expression

- `expr1 ? expr2 : expr3` where `expr2` evaluated if `expr1` true and `expr3` evaluated otherwise
- both `expr2` and `expr3` will never be evaluated in the ternary expression
- i.e. `(2 < 3) ? ++x : --y;` will evaluate `++x` only

## Type Promotion

- arithmetic only performed with types of int or higher than int
- char and short must first be promoted to int before arithmetic operations performed
- in general, if disparate types present, "smaller" types are promoted to "largest" type before arithmetic performed

## Expression Evaluation

- if multiple operators applied, operator precedence and then associativity are used to determine evaluation order

## Comma Operator

- evaluates all operands in left-to-right order and then returns the result of the rightmost operation

```C
    printf("%d\n", (1, 2));         // prints 2 since rightmost result returned
    
    int b = 2;
    int c = -3;
    if (c += 3, a = b, --b) {
        printf("Will I print?");    // c = 0, a = 2, b = 1 here and since 1 returned, statement will print
    }
```

## Different Bases

- integer literals: starting with 0 --> octal, starting with 0x --> hexadecimal, otherwise --> decimal
- for `printf()` and `scanf()`:
  - %o: prints/reads unsigned integer type in octal
  - %u: prints/reads unsigned integer type in decimal
  - %x or %X: prints/reads unsigned integer type in hexadecimal

## Bitshift Operators

- `a << n`: shifts n bits of a to the left 
  - leftmost n bits of a are discarded
  - rightmost n bits of a are zeroes
  - value of a does NOT change
  - a << n = (2 ^ n) * a;
- `a >> n`: shifts n bits of a to the right
  - rightmost n bits of a are discarded
  - leftmost n bits of a are zeroes
  - value of a does NOT change
  - a >> n = a / (2 ^ n);

## Bitwise Operators

- bitwise operations do NOT change operands UNLESS there is an assignment operation involved
- bitwise AND (&) : returns 1 if both sides evaluate to 1 and 0 otherwise
- bitwise OR (|): returns 0 if both sides evaluate to 0 and 1 otherwise
- bitwise COMPLEMENT (~): flips the bit (0 --> 1 or 1 --> 0)
- bitwise XOR (^): returns 1 if both sides evaluate differently (i.e. one 0 and other 1) and 0 otherwise

## Mixted Type Assignments

- rvalue data is converted to lvalue type if the types are different (can lead to loss of information)

```C
    float f = 2.25;
    float g = 4.99;
    int i = f;          // assigns 2 to i
    int j = g;          // assigns 4 to g
    
    unsigned int x, y;
    unsigned char z;
    y = oxface;
    x = z = y;          // right-to-left associativity so x and z BOTH get a converted value of 0xface
```