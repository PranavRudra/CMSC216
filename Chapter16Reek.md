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

## Floating-Point Functions

### General (math.h)
```C
    // common mistake regarding mathematical functions
    // assume that math.h was not included in the source
    
    double x;
    x = sqrt(5.5);
    
    // compiler assumes that sqrt() returns an int (since it hasn't seen prototype for sqrt())
    // thus, sqrt() will return an int that is typecasted into a double variable which is incorrect
    
    // domain error: error that occurs if function argument is NOT within a certain range
    // i.e. sqrt(-5.0); causes domain error since square roots of negative numbers are undefined
    
    // range error: error that occurs if function result is too large or too small to be stored in double
    // i.e. exp(DBL_MAX) will cause range error since the output is too large to be stored in a double
```

## Execution Environment

### Terminating Execution (stdlib.h)
```C
    void abort(void);                   // terminates program abnormally by sending the SIGABRT signal
    void atexit(void (func)(void));     // registers an exit function (function cannot call exit())
    
    // exit functions are invoked before a program terminates when `exit()` is called
    
    exit(int status);                   // terminates the program with the given status code

    // registered exit functions are called in reverse order
    // all buffers are flushed
    // all open files are closed
    // removes all temporary files
    // exit status is returned to host environment
```

### Assertions (assert.h)
```C
    void assert(int expression);
    
    // if expression evaluates to false program terminates and message printed to stdout
    // using #define NDEBUG in source or -D NDEBUG on command line tells preprocessor to 
    // remove all assert statements (should only use when your code is fully tested)
```

### Environment (stdlib.h)
```C
    char *getenv(char const *name);
    
    // environment is implementation-defined key-value list of pairs maintained by OS
    // getenv() searches for the key name and returns pointer to value if name exists
    // if name doesn't exist as a key in the environment, getenv() returns NULL pointer
```

### System Commands (stdlib.h)
```C
    void system(char const *command);           // executes command by system's command processor
```

## Searching and Sorting
```C
    void qsort(void *base, size_t n_elements, size_t el_size, int (*compare)(void const*, void const*));
    
    // base: array of elements to be sorted
    // n_elements: number of elements in base
    // el_size: size of each element in base
    // compare: pointer to comparator function
    
    // example
    
    typedef struct {
        char key[10];
        int data;
    } Record;
    
    int r_compare(void const *a, void const *b) {
        return strcmp((Record *) a -> key, (Record *) b -> key);
    }
    
    Record array[50];
    
    qsort(array, 50, sizeof(Record), r_compare);    // sorts array by key field in Record
```
 