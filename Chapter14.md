# Chapter 14: The Preprocessor

## Functions of the preprocessor

- remove comments from the source file
- paste contents of included header files
- define and substitute macros
- use conditional compilation to determine if certain code should be compiled

## #define 

- general form is `#define name stuff` (stuff will be substituted for name)
- general form for macros is `#define name(parameter-list) stuff
- macros can contain defined symbols but CANNOT be recursive

```C
    // examples of #define's usage
    
    #define reg     register
    
    // multiline definitions are also allowed
    
    #define DEBUG_PRINT     printf("File %s line %d:" \
                            " x=%d, y=%d, z=%d", \
                            __FILE__, __LINE__, \
                            x, y, z)
    
    // adjacent string literals are concatenated into one string
    // no semicolon is included since user will often write one in the source file
```

```C
    // incorrect macro
    
    #define SQUARE(x)       x * x
    
    // this works fine with code like SQUARE(5). however, substituting in something like SQUARE(3 + 1) will 
    // output 10 instead of the expected 16 since the preprocessed code looks like 3 + 1 * 3 + 1 which 
    // resolves to 3 + 3 + 1 = 10
    
    // slightly improved macro
    
    #define SQUARE(x)       (x) * (x)
    
    // this works fine with code like SQUARE(3 + 1). however, substituting in something like 10 * SQUARE(3 + 1) 
    // will output 34 instead of the expected 160 since the preprocessed code looks like 10 * 3 + 1 * 3 + 1 which 
    // resolves to 30 + 3 + 1 = 34
    
    // correct macro
    
    #define SQUARE(x)       ((x) * (x))
    
    // this works for any x substituted in. macros evaluating numeric expressions should always be written this way
```

```C
    // injecting macro arguments into string literals (method 1)
    
    #define PRINT(FORMAT,VALUE)         printf("The value is " FORMAT "\n", VALUE)
    
    // as previously stated, adjacent strings are concatenated so given source code like PRINT("%d", x + 3), 
    // the preprocessed code would look like printf("The value is %d", x + 3) and the string 
    // "The value is [insert value of x + 3]" would be printed to stdout.
    
    // injecting macro arguments into string literals (method 2)
    
    #define PRINT(FORMAT,VALUE)         printf("The value of " #VALUE " is " #FORMAT "\n", VALUE)
    
    // the construct #VALUE is converted to "[insert string to be substituted for VALUE]". consequently, given
    // source code like PRINT("%d", x + 3), the preprocessed code would look like 
    // printf("The value of x + 3 is %d\n", x + 3) and the string "The value of x + 3 is [insert value of x + 3]"
    // would be printed to stdout
```

```C
    // watch out for side effects when defining macros
    
    #define MAX(a,b)        ((a) > (b) : (a) ? (b))
    ...
    x = 5;
    y = 8;
    z = MAX(x++, y++);
    printf("x = %d, y = %d, z = %d", x, y, z);
    
    // this code would print out x = 6, y = 10, z = 9 instead of the expected x = 6, y = 9, z = 8 since the macro
    // causes repeated side effects. preprocessed code would look like z = ((x++) > (y++) : (x++) ? (y++)). after
    // the 5 > 8 check, x is incremented to 6 and y is incremented to 9. however, since x was not greater in 
    // the greater than check, z = y++ is executed, assigning 9 to z and then incrementing y again. thus, the side 
    // effect occurs twice instead of once
```

## Macros vs. functions

- advantages:
  - macros are typeless (i.e. `#define MAX(a,b)   ((a) > (b) : (a) ? (b))` works for any data type comparable by `>`)
  - macros are faster since text substitution doesn't have the overhead of function stack frames
- disadvantages:
  - macros increase code size since they are substituted in every location where the defined symbol is found
  - errors caused by functions are MUCH easier to debug than those caused by macros
  - intended operator precedence is easier to code with functions than with macros

## Conditional compilation

```C
    // without else condition
    
    #if constant-expression
        statements
    #endif
    
    // with else condition(s)
    
    #if constant-expression
        statements
    #elif constant-expression
        more code
    #else 
        other code
    #endif
    
    // sample usage
    
    #if DEBUG
        printf("x = %d, y = %d\n", x, y);
    #endif
    
    // printf will only be compiled if the DEBUG symbol has been given a nonzero value
```

```C
    // defined construct
    
    #if defined(symbol)     // defined(symbol) resolves to true if symbol has been defined via a #define
                            // OR if symbol has been defined via the command line using the -Dname=stuff flag
```

```C
    // suppose we have a source file myCode.c and myCode.c includes two header files a.h and b.h
    // each of these header files includes the header file x.h. as a result myCode.c includes x.h twice
    // causing a compilation error (the same header file cannot be included multiple times)
    
    // solution (within x.h)
    
    #if !defined(COMPILED)
        #define COMPILED    1
        /*
            all the code x.h should have
        */
    #endif
    
    // now, COMPILED will be defined after x.h is included in the first header file. so when the second header
    // file includes x.h again, COMPILED will already be defined and the contents of x.h will not be pasted into 
    // that header file
```
