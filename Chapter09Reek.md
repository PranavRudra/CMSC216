# Chapter 9: Strings, Characters, and Bytes

## String representation

- strings are stored as either:
  - null-terminated character arrays
  - read-only literals (i.e. char str[] = "xyz")
- all other forms of array declaration can be used as well (just make sure to include terminating NULL)

## String length

```C
    size_t strlen(char const *string);
    
    // returns number of characters in string EXCLUDING the terminating NULL
    
    // size_t is an unsigned integer so it should be cast to an int when using strlen's return value
    // e.g. strlen(x) >= strlen(y) always works as espected
    // e.g. strlen(x) - strlen(y) >= 0 ALWAYS returns true however (since unsigned ints cannot be negative)
    // e.g. strlen(x) - 10 >= 0 also has undefined behavior (you shouldn't mix signed and unsigned integers)
```

## Copying strings

```C
    char* strcpy(char* dst, char const *src);
    
    // Copies the contents of src into dst (overwriting previous contents) and then returns a pointer to dst 
    // e.g. if message contains "ORIGINAL MESSAGE", strcpy(message, "DIFFERENT") returns a pointer to message,
    // which has been modified to contain the string D I F F E R E N T \0 E S S A G E \0. everything beyond
    // the first '\0' is essentially ignored by other function calls
    
    // dst CANNOT contain a string literal since it is modified 
    // src CAN contain a string literal since it is NOT modified
    
    // WARNINGS:
    // dst MUST be large enough to accommodate src. if it is not, then dst will not be null-terminated
    // and therefore will no longer be a string. also, dst and src CANNOT overlap or else the behavior of the 
    // function is undefined.
```

```C
    char* strncpy(char* dst, char const *src, size_t len);
    
    // Copies the first len characters of src into dst (overwriting previous contents) and then returns a pointer 
    // to dst. if strlen(src) < len <= strlen(dst) then remaining characters in dst (up until len is reached) will 
    // hold '\0' characters.
    // e.g. if dst holds a b c d \0 and src holds the string a \0 and len is 3, then dst will be a \0 \0  d \0
    
    // dst CANNOT contain a string literal since it is modified 
    // src CAN contain a string literal since it is NOT modified
    
    // WARNINGS:
    // dst MUST be large enough to accommodate the first len characters of src. if it is not, then dst will not be 
    // null-terminated and therefore will no longer be a string. similarly, if strlen(src) >= len then dst will 
    // not be null-terminated. also, dst and the first len characters of src CANNOT overlap or else the 
    // behavior of the function is undefined
    
    // to ensure that strncpy works
    
    char buffer[BSIZE];
    
    ...
    strncpy(buffer, name, BSIZE);
    buffer[BSIZE - 1] = '\0';
```

## Concatenating strings

```C
    char* strcat(char* dst, char const *src);
    
    // Concatenates the contents of src to dst and then returns a pointer to dst 
    // e.g. if message contains M E S S A G E \0 \0 \0, strcat(message, "HI") returns a pointer to message,
    // which has been modified to contain the string M E S S A G E H I \0. everything beyond
    // the first '\0' is essentially ignored by other function calls
    
    // dst CANNOT contain a string literal since it is modified 
    // src CAN contain a string literal since it is NOT modified
    
    // WARNINGS:
    // dst MUST be large enough to accommodate itself AND src. if it is not, then dst will not be null-terminated
    // and concatenation will continue into memory that is out of bounds. also, dst and src CANNOT overlap or 
    // else the behavior of the function is undefined.
```

```C
    char* strncat(char* dst, char const *src, size_t len);
    
    // Concatenates the the first len characters of src (and a '\0') to dst and then returns a pointer to dst

    // dst CANNOT contain a string literal since it is modified 
    // src CAN contain a string literal since it is NOT modified
    
    // WARNINGS:
    // dst MUST be large enough to accommodate itself, the first len characters of src, and the terminating '\0' 
    // that strncat writes. if it is not, then dst will not be null-terminated and concatenation will continue 
    // into memory that is out of bounds. also, dst and src CANNOT overlap or else the behavior of the 
    // function is undefined.
```

## Comparing strings

```C
    int strcmp(char* const s1, char* const s2)
    
    // returns 0 if s1 and s2 are equal strings
    // returns >0 if s1 > s2
    // returns <0 if s1 < s2
    
    // can accept string literals for either s1 OR s2 since neither is modified
    
    // s1 > s2 means that the first differing character in s1 has a higher ASCII value than the first 
    // differing character in s2. in other words, s1 comes before s2 in lexicographic (dictionary) order.
    // vice versa for s2 > s1
    
    // example
    
    strcmp("abcdef", "ABCDEF"); 
    
    // returns >0. the first differing character between s1 and s2 is 'a' vs. 'A'. since the ASCII value 
    // of 'a' (97) is greater than 'A' (65), s1 > s2. thus, s1 comes before s2 lexicographically
```

```C
    int strcmp(char* const s1, char* const s2, size_t len);
    
    // returns 0 if the first len characters of s1 and s2 are equal
    // returns >0 if s1 > s2 for the first len characters of s1, s2
    // returns <0 if s1 < s2 for the first len characters of s1, s2
    
    // can accept string literals for either s1 OR s2 since neither is modified
```

## Finding a character

```C
    char* strchr(char const *str, int ch);
    
    // finds the FIRST occurrence of the character ch in str and returns a pointer to it (if it exists).
    // otherwise, returns NULL. e.g. strchr("HELLOH", 'H') returns a pointer to the FIRST character
    
    // can pass in a string literal as str since str is not modified
```

```C
    char* strrchr(char const *str, int ch);
    
    // finds the LAST occurrence of the character ch in str and returns a pointer to it (if it exists).
    // otherwise, returns NULL. e.g. strchr("HELLOH", 'H') returns a pointer to the LAST character
    
    // can pass in a string literal as str since str is not modified
```

## Finding a substring

```C
    char* strstr(char const *s1, char const *s2);
    
    // returns a pointer to the character in s1 at which the substring s2 begins or NULL if it doesn't exist.
    // returns a pointer to the first character of s1 if s2 points to an empty string
    // e.g. strstr("HELLO", "LLO") returns a pointer to the first L in "HELLO" since "LLO" begins from that
    // character

    // can pass in a string literal as either s1 or s2 since neither string is modified
```


