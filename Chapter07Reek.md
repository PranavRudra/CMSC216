# Chapter 7: Functions

## typedef

```C
    typedef type-name new-name;     // can use new-name to refer to type-name
    
    typedef float Rate;
    Rate x, y;
```

## Function arguments

- **all** function arguments in C are pass-by-value
- must receive pointer to change a variable's value
- must receive double pointer to change pointer's value