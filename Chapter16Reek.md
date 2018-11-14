# Chapter 16: Standard Library

## Integer Functions

### Arithmetic (stdlib.h)
```C
    int abs(int value);             // returns absolute value of value
    
    div_int div(int numerator, int denominator);
    
    // returns a div_int struct with two fields: quot and rem
    // quot represents quotient of the division
    // rem represents remainder of the division
```

### Random Nubers (stdlib.h)
```C
    int rand (void);                // returns a pseudorandom integer
    void srand(unsigned int seed);  // seeds random number generator with seed
```

### String conversion (stdlib.h)
```C    
    int atoi(char const *string);   // converts string into base-10 integer
    
    // skips any leading whitespace characters and ignores any illegal trailing characters
    // i.e. atoi("12C123"); returns 12 while atoi("a123t567"); returns 0 
    // returns 0 in the case where no integer can be extracted from the string
    
    long int strtol(char const *string, char **unused, int base);

    // converts string into specified base integer
    
    // skips any leading whitespace characters and ignores any illegal trailing characters
    // saves a pointer to first illegal character in variable pointed to by unused
    // i.e. strtol("   590bear", next, 12); stores pointer to 'e' in variable that next points to
```