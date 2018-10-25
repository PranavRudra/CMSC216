# Chapter 9: Strings, Characters, and Bytes

## String representation

- strings are stored as either:
  - null-terminated character arrays
  - read-only literals (i.e. "xyz")

## String length

```C
    size_t strlen(char const *string);
    
    // returns number of characters in string EXCLUDING the terminating NULL
    
    // size_t is an unsigned integer so it should be cast to an int when using strlen's return value
    // e.g. strlen(x) >= strlen(y) always works as espected
    // e.g. strlen(x) - strlen(y) >= 0 ALWAYS returns true however (since unsigned ints cannot be negative)
    // e.g. strlen(x) - 10 >= 0 also has undefined behavior (you shouldn't mix signed and unsigned integers)
```
