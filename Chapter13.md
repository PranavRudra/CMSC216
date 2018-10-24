# Chapter 13: Advanced Pointer Concepts

## Declaration by inference

```C
    int f;          // an integer
    int f[];        // subscripting f gives us an integer; therefore f is an array of integers
    int *f;         // dereferencing f gives us an integer; therefore, f is a pointer to an integer
    int f();        // invoking f gives us an integer; therefore, f is a function returning an integer
    int *f, g;      // f is a pointer to an integer but g is just an integer (g is not dereferenced to give an int)
    
    int *f();       // operator precedence makes us deal w/ () first
                    // invoking f gives us something that, when dereferenced, gives an integer
                    // therefore, f is a function that returns a pointer to an integer
                    
    int (*f)();     // () have left-to-right associativity so we deal with the leftmost () first
                    // dereferencing f gives us something that when invoked, returns an integer
                    // therefore, f is a pointer to a function that returns an integer
                    
    int *(*f)();    // () have left-to-right associativity so we deal with the leftmost () first
                    // then we will deal with the rightmost () and then the leftmost indirection
                    // dereferencing f gives us something that when invoked returns a pointer to an integer
                    // therefore f is a pointer to a function that returns a pointer to an integer
                    
    int *f[];       // [] has greater precedence than * so we have to deal with the subscripting first
                    // subscripting f gives us something that when dereferenced gives us an integer
                    // therefore, f is an array of pointer to integers
                    
    int f()[];      // () has greater precedence than [] so we have to deal with it first
                    // invoking f gives us something that when subscripted returns an integer
                    // therefore, f must be a function which returns an array of integers (ILLEGAL)
    
    int f[]();      // () has greater precedence than [] but we need to subscript first before invoking
                    // subscripting f gives us something that when invoked returns an integer
                    // therefore, f is an array of functions that return integers (ILLEGAL)
                    
    int (*f[])();   // () has greater precedence than [] so we must deal with the leftmost () first
                    // in the leftmost (), [] has greater precedence than * so we must deal with it first
                    // subscripting f gives us something that when dereferenced and then invoked returns an integer
                    // therefore, f is an array of pointers to functions that each return an integer
                    
    int *(*f[])();  // () has greater precedence than [] so we must deal with the leftmost () first
                    // in the leftmost (), [] has greater precedence than * so we must deal with it first
                    // subscripting f gives us something that when dereferenced and then invoked returns an int *
                    // therefore, f is an array of pointers to functions that each return a pointer to an integer
```

## Pointers to functions

```C
    // Sample declaration
    
    int f(int);                         // sample function prototype
    int (*pf)(int) = &f;                // however, int (*pf)(int) = f; is also valid
                                        // the compiler converts function names to function pointers
```

```C
    // Sample invocation (all three are valid)
    
    f(25);
    (*pf)(25);
    pf(25);
    
    // dereferencing the pointer doesn't matter since compiler converts function name back to a function pointer 
    // before applying the function invocation (i.e. ()) operator anyways. thus, either invocation is valid 
```

## Uses of function pointers

```C
    // Callback functions 
    
    Node* search(Node *node, void const *value, int (*compare)(void const *, void const *)) {
        while (node != NULL) {
            if (compare(&node -> value, value) == 0)
                break;
            node = node -> link;
        }
        return node;
    }
    
    // passing in the compare function pointer enables this search() function to be typeless
    // value and parameters to (*compare) are declared void * since type of data to be compared isn't known
    
    // one possible compare function
    
    int compare_ints(void const *a, void const *b) {
        if (*(int *)a == *(int *)b)
            return 0;
        return 1;
    }
    
    // a and b are casted to int * before being dereferenced since they are void * variables
```

```C
    // Jump tables
    
    double add(double, double);
    double sub(double, double);
    double mul(double, double);
    double div(double, double);
    
    double (*jump_table[])() = { add, sub, mul, div };
    
    // jump_table is an array of pointers to functions that return doubles
    // now, to invoke the add function we can simply write jump_table[0](5, 5);
```

## Command line arguments

```C
    
```
