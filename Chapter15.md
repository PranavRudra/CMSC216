# Chapter 15: Input/Output Functions

## Error Reporting

- whenever an error occurs, C library functions save an error code in `errno` (an `extern` variable defined in `errno.h`)
- `perror(char const *message);` prints `message` followed by `: ` and then the message associated with the `errno` code
   - message must be non-NULL and point to a valid string
- `void exit(int status);` terminates program execution with the status code of `status`

## I/O Concepts

### Streams

- abstraction for I/O device interaction presented as a sequence of data concerned with input or output
- *text stream*: sequence of characters terminated by a newline (on UNIX machines)
   - on systems with differing definitions of text, library functions translate between representations
- *binary stream*: sequence of bytes
   - binary data is NOT modified by library functions (this guarantee does not exist with text data)

### Buffers

- area of memory data is copied into before being *flushed* or written to final destination (increases I/O speed)
- types:
  - *unbuffered*: data is written directly to final destination without buffer
  - *line buffering*: buffer is flushed when newline character is read
  - *block buffering*: buffer is flushed in blocks of arbitrary size

### FILEs

- data structure giving a program access to a stream
- access to 3 streams is guaranteed
  - `stdin`: `FILE *` for standard input
  - `stdout`: `FILE *` for standard output
  - `stderr`: `FILE *` for standard error

## Stream I/O

- declare a `FILE *` for each file that must be simultaneously opened
- call `fopen()` and set your `FILE *` above to the return value
- process the file
- call `fclose()` on the `FILE *` to release it for reuse with another file

```C
    FILE * fopen(char const *name, char const *mode);
    
    // opens a file and associates a stream with it
    // return value is NULL if the operation fails
    
    // name: name of the file
    // mode: 'r', 'w', 'a' (read, write, append) for text files or 'rb', 'wb', 'ab' for binary files
    
    // example
    FILE *input;
    
    input = fopen("data.txt", 'r');
    if (input == NULL) {
        perror("data.txt");
        exit(EXIT_FAILURE);
    }
    // the error message could look like: data.txt: No such file or directory
````

```C
    int fclose(FILE *f);
    
    // flushes the buffer before the file is closed
    // returns 0 if there were no errors
    
    // f: FILE * returned by fopen()
    
    //example
    FILE *input;
    ...
    
    if (fclose(input) != 0) {
        perror("fclose");
        exit(EXIT_FAILURE);
    }
```

## Character I/O

### Input
```C
    int fgetc(FILE * stream);   // reads a character from stream and returns it if operation worked (or EOF otherwise)
    int getchar(void);          // reads a character from stdin and returns it if operation worked (or EOF otherwise)
    
    // return value is int instead of char because EOF is denoted by a value outside the range of a character
```

### Output
```C
    int fputc(int character, FILE * stream);    // writes character to stream and returns EOF if operation fails
    int putchar(int character);                 // writes character to stdout and returns EOF if operation fails
```

### Undoing I/O
```C
    int ungetc(int character, FILE * stream);
    
    // Pushes back a character to a stream so that getc() will return that character again
    // Changing stream position with fseek() will discard ungotten characters
```
