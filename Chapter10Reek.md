# Chapter 10: Structures and Unions

## Declarations

- general form is `struct tag { member-list } variable-list`
- initializations can happen w/ {} or . member access
  - {} used when initialization happens along with declaration (like arrays)
  - elements in {} assigned to structure members in order of inclusion

```C
    // declares and initializes a struct with the following three fields 
    struct {
        int a;
        char b;
        float c;
    } x;
    
    // could have initialized as follows
    struct {
        int a;
        int b;
        float c;
    } x = { 2, 2, 3.46 };
```

```C
    // creates a template for a struct with the following fields
    struct SIMPLE {
        int a;
        char b;
        float c;
    };
    
    // initializes a struct
    struct SIMPLE x;
```

```C
    // creates a template for a struct with the following fields
    typedef struct {
        int a;
        char b;
        float c;
    } Simple;
    
    // initializes a struct
    Simple x;
```

```C
    // ILLEGAL since this struct must have a struct b which itself must have a b etc....
    struct SELF_REF1 {
        int a;
        int b;
        struct SELF_REF1 *c;
    };
    
    // LEGAL since compiler knows the size of a pointer at compile time
    // therefore we don't have any endless recursion like we do above
    struct SELF_REF2 {
        int a;
        int b;
        struct SELF_REF2 *c;
    };
    
    // initializes a struct
    struct SELF_REF2 x;
```

```C
    // ILLEGAL since SELF_REF3 doesn't exist until end of typedef
    typedef struct {
        int a;
        int b;
        struct SELF_REF3 *c;
    } SELF_REF3;
    
    // LEGAL since SELF_REF3_TAG exists by the time the compiler reaches the pointer declaration
    typedef struct SELF_REF3_TAG {
        int a;
        int b;
        struct SELF_REF3_TAG *c;
    } SELF_REF3;
    
    // initializes a struct
    SELF_REF3 x;
```

```C
    // LEGAL since the type B exists (partially) by the time the compiler declares a pointer to B
    struct B;
    
    struct A {
        struct B *partner;
    }
    
    struct B {
        struct A *partner;
    }
```

## Storage Allocation

- size of a structure is greater than or equal to the size of its constituent fields
- compiler sometimes wastes space to comply with by machine alignment requirements

## Function Arguments

- functions like `int compute_cost(Transaction trans);` cause entire struct to be copied
- functions like `int compute_cost(Transaction *trans);` don't copy entire struct 
  - indirection is used to modify the structure whose address is passed in

## Unions

- aggregate type whose members refer to the **same location(s) in memory**
- used to store different things in one place at different times
- size of a union is greater than or equal to the size of the union's largest member

```C
    union {
        float f;
        int i;
    } fi;
    
    fi.f = 3.14159;
    printf("%d\n", fi.i);       // prints 3 since it interprets the SAME bits in fi.f as an integer
```

```C
    union {
        int i;
        float f;
        char c[4];
    } fi = { 5 };               // {} must have a value that is compatible with first union member
```
