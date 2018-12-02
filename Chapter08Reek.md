# Chapter 8: Arrays

## Initialization

```C
    int a[4] = {2, 4, 6, 8};        // VALID since number of elements in initializer <= 4
    int b[5] = {1, 3};              // VALID, remaining 3 elements will be initialized to 0
    int c[] = {5, 7, 9};            // VALID, size of the array is now 3
    int d[10] = {0};                // VALID, remaining 9 elements will be initialized to 0
    int e[100];                     // VALID, size of the array is 100 (and has garbage values if e automatic)
    
    int f[];                        // INVALID, must include size OR {} when initializing array
    int g[4] = {};                  // INVALID, {} cannot be empty (if used)
    int h[3] = {10, 20, 30, 40};    // INVALID, number of elements in {} must be <= size
    int i[a[0]] = {0, 1};           // INVALID, size can only be literal or #define'd preprocessor directive
    int x = 2, y = 3;               // INVALID, {} values can ONLY be literals or #define'd preprocessor directives
    int j[2] = { 2 * x, 3 * y};
    int k[4]; k = {1, 2, 3, 4};     // INVALID, {} can ONLY be used when initializing the array (not after)
```

## Array Arguments

- array name is decayed to a pointer to the first element if passed into a function
- size is ignored if present in function header (i.e. 5 ignored in `func(int a[5]);`)
- name of the array is treated as a **constant pointer** to the first element in array

```C
    // NOTE: *(arr + k) = arr[k] AND &arr[k] = arr + k
    // NOTE: subscripts can be used with a pointer as well
    
    int i, arr[5] = { 10, 20, 30, 40, 50 };
    int *p;
    
    p = &arr[2];
    *p = 60;                    // array is now { 10, 20, 60, 40, 50 }
    
    p = arr;
    *p = 70;                    // array is now { 70, 20, 60, 40, 50 }
    
    arr[4] = *(arr + 3);        // array is now { 70, 20, 60, 40, 40 }
    
    *(arr + 3) = 80;            // array is now { 70, 20, 60, 80, 40 }
    
    arr[1] = &arr[4] - arr;     // array is now { 70, 4, 60, 80, 40 }
    
    p = arr + 2;
    p[2] = 100;                 // array is now { 70, 4, 60, 80, 100 }
    p[-1] = 10;                 // array is now { 70, 10, 60, 80, 100 }
    
    arr++;                      // ILLEGAL since arr is treated as a constant pointer
    *(arr + 5);                 // SEGFAULT likely since arr + 5 points to region outside of array
    *(arr - 1);                 // SEGFAULT likely since arr - 1 points to region outside of array
```
