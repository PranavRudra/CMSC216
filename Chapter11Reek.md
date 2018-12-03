# Chapter 11: Dynamic Memory Allocation

## Functions (stdlib.h)

```C
    // NOTE: behavior is undefined if 0 bytes are requested with any of the functions
    
    void *malloc(size_t size);
    
    // allocates `size` bytes of memory from heap and returns pointer to starting address
    // returns NULL in case of failure
    // does not initialize allocated memory at all

    void *calloc(size_t num, size_t size);
    
    // allocates `num * size` bytes of memory from heap and returns pointer to starting address
    // returns NULL in case of failure
    // initializes allocated memory to all 0 bits
    
    void *realloc(void *ptr, size_t new_size);
    
    // allocates `new_size` bytes in same location if possible and returns pointer to starting address
    // otherwise allocates `new_size` bytes in a different location, copies data, and frees old memory
    // returns NULL in case of failure
    
    void free(void *p);
    
    // frees memory being used by p
    // does nothing if p is NULL
```

## Mistakes

- never free a pointer that has already been freed (be careful with pointer aliasing for this reason)
- never call free() on memory that is not dynamically allocated (behavior is undefined)
- never try to free a part of allocated memory (i.e. behavior of `free(ptr + 3)` is undefined)
