# Chapter 3: Data

## Data Types

- *scalar types*: integer types, floating point types, pointer types
- *aggregate types*: arrays, structures, unions

## Integer Types

- size of `short` <= size of `int` <= size of `long` according to C standard
- integer types can be *signed* or *unsigned* (convert w/ Two's Complement)
- `char`: always 1 byte
- `short`: >= 2 bytes
- `int`: >= 2 bytes
- `long`: >= 4 bytes
 
## Floating-point Types

- size of `float` <= size of `double` <= size of `long` according to C standard
- `float`: >= 4 bytes
- `double`: >= 8 bytes
- `long double`: >= 10 bytes

## Enumerated Types

```C
    enum Suit = { SPADES, HEARTS, DIAMONDS, CLUBS };
    
    Suit suit1 = SPADES, suit2 = CLUBS;
    
    // default values of the constants in enums are 0, 1, 2 but can be set differently
    // e.g. enum Suit = { SPADES, HEARTS, DIAMONDS = 42, CLUBS }; makes SPADES = 0, 
    // HEARTS = 1, DIAMONDS = 42, and CLUBS = 43
```

## Scope

- region of a program where an *identifier* (name of a variable) may be used
- *file scope*: variable declared outside all blocks is accessible anywhere in that source file
- *block scope*: variable declared within a *block* (region within braces) is only accessible in that block
  - identifier inside nested block with same name as identifier in outer block shadows the outer block's identifier

```C
    int a = 1;
    
    int main(void) {
        int a = 2;
        printf("a is %d\n", a);         // prints 2 since a in main() shadows the file-scoped a
        {
            int a = 3;
            printf("a is %d\n", a);     // prints 3 since a shadows outer a and file-scoped a
        }
        printf("a is %d\n", a);         // prints 2 since a in main() shadows the file-scoped a         
    }
```

## Linkage 

### Introduction 

- determines how multiple occurrences of an identifier are treated across one or multiple source files
- *no linkage*: multiple declarations of identifier treated as separate and distinct entities
- *internal linkage*: all declarations of identifier within a source file refer to same entity
  - but multiple declarations of the identifier across different files refer to disparate entities
- *external linkage*: all references to the identifier refer to the same entity

### Defaults

- file-scoped variables have external linkage
- block-scoped variables have no linkage

### Modifying Defaults

- using keyword `static` on **file-scoped variables** gives them internal linkage
- using keyword `extern` on **block-scoped variables** gives them external linkage
  - gives the function access to a file-scoped variable in a different file

```C
    static int i;       // declaring file-scoped i as static gives it internal linkage
    
    int funct() {
        int j;
        extern int k;   // gives k external linkage, giving function access to a k in another source file
        extern int i;   // gives i external linkage, giving function access to an i in another source file
                        // DOES NOT change the internal linkage of the file-scoped i (the two i's are separate)
                        // HOWEVER, this i shadows the outer one in this function (so all references to i refer to 
                        // the one defined here with external linkage)
    }
```

## Storage Class

### Introduction

- refers to the type of memory in which the variable's value is stored
- determines when the variable is destroyed and how long it stays in memory

### Defaults

- file-scoped variables have *static* storage by default (i.e. they are stored in static memory)
- block-scoped variables have *automatic* storage by default (i.e. stored in stack or heap memory)
  - variables are created just before program execution enters block
  - variables are removed just as program execution leaves the block

### Modifying Defaults

- using `static` on block-scoped variables changes their storage class from automatic to static
  - variable then exists for the entire duration of the program
  - as a result these variables are only initialized once
- file-scoped variables' storage class CANNOT be changed 

```C
    static void f(void) {
        static int i = 1;
        int j = 1;
        
        i = i + 1;
        j = j + 1;
                                            // if f called 3 times:
        printf("i: %d, j: %d\n", i, j);     // i: 2, j: 2
                                            // i: 3, j: 2
                                            // i: 4, J: 2
    }
```