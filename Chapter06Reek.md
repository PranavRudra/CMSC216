# Chapter 6: Pointers

## Introduction

- *pointer* is a variable which holds the address of another variable
- & - used to get the address of any variable (only rvalue)
- \* - *dereferences* the pointer or accesses value in memory location it points to (lvalue or rvalue)

```C
    int *p, q;  // p is a pointer to an integer and q is an integer
```

## const

```C
    const int v = 1;    // indicates that the value of v cannot be changed
    
    int *const p = &a;  // indicates that p is a *constant pointer*
    *p = 5;             // works fine
    p = &y;             // CANNOT reassign p since it's a constant pointer

    const int *q;      // indicates that q is a pointer to an integer constant
    q = &k;             // works fine
    *q = 10;            // CANNOT change value of variable pointer to be q
    
    const int *const m; // indicates a m is a constant pointer to an integer constant
    m = &h;             // CANNOT change variable pointed to by m
    *m = 10;            // CANNOT change value of variable pointed to by m
```

## Pointer Arithmetic

- pointers of same type can be compared with >=, <=, <, >, !=, ==
- comparing pointers is only sensible when they point to same array
- comparing pointers compares the memory addresses they contain

```C
    int arr[] = {0, 1, 2};
    int *p = &arr[0];
    
    // assume each of the following statements is independent
    
    *p--;   // decrements p to point to 4 bytes before start of array and then dereferences (likely segfault)
    *p++;   // increments p to point to index 1 and then dereferences p, giving 1
    (*p)++; // dereferences p, giving 0, and then increments, making array {1, 1, 2}
    (*p)--; // dereferences p, giving 0, and then decrements, making array {-1, 1, 2}
```

## Void Pointers

- void pointers can point to variables of any type
- void pointers can be assigned to pointers of any type (WITHOUT casting)
- void pointers cannot be dereferenced unless they are assigned to a non-void pointer type

```C
    int a = 2, *q = &a, *r;
   
   void *p;
    
   p = q;                           // LEGAL since void pointers can point to variables of any type
   r = p;                           // LEGAL since void pointers can be assigned to non-void pointers w/o casting
   
   print("%d\n", *p);               // ILLEGAL since void pointers can't be dereferenced w/o assignment to non-void type
   printf("%d\n", *r);              // LEGAL since r now points to the variable a as well
   printf("%d\n", (int) *p);        // ILLEGAL since void pointers can't be dereferenced w/o assignment to non-void type
   printf("%d\n", *((int *) p));    // LEGAL since p is being dereferenced ONLY after being treated as an int * variable
```

## Mistakes

```C
    // returning pointer to block-scoped local variable is NOT a good idea since stack frame containing it
    // may be popped (and associated memory locations overwritten) by the time the returned pointer is
    // dereferenced
    
    int * return_a_pointer(void) {
        int x = 42;
        return &x;
    }
```

```C
    // make sure casting of pointers is used appropriately
    
    int x = 10, *p = &x;
    double *q;
    q = (double *) p;       // now q points to the integer variable x
    *q = 6.666;             // will create problems since x is an int
```
