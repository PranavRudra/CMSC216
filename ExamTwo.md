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
                    
    int (*f)[]();   // () has greater precedence than [] so we must deal with the leftmost () first
                    // dereferencing f gives us something that when subscripted and then invoked returns an integer
                    // therefore, f is a pointer to an array of functions that each return an integer
```
