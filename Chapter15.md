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
    int fgetc(FILE *stream);   // reads a character from stream and returns it if operation worked (or EOF otherwise)
    int getchar(void);          // reads a character from stdin and returns it if operation worked (or EOF otherwise)
    
    // return value is int instead of char because EOF is denoted by a value outside the range of a character
```

### Output
```C
    int fputc(int character, FILE *stream);    // writes character to stream and returns EOF if operation fails
    int putchar(int character);                 // writes character to stdout and returns EOF if operation fails
```

### Undoing I/O
```C
    int ungetc(int character, FILE *stream);
    
    // Pushes back a character to a stream so that getc() will return that character again
    // Changing stream position with fseek() will discard ungotten characters
```

## Unformatted Line I/O

### Input
```C
    char *fgets(char * buffer, int buffer_size, FILE * stream);

    // fgets reads characters from stream and copies them into buffer until:
    // a) a newline character is read (and copied into the buffer) OR
    // b) buffer_size - 1 characters have been read (and copied into buffer)
    // in both cases, a NULL character is placed in buffer[buffer_size]
    
    // returns pointer to same array if operation is successful and NULL otherwise
```

### Output
```C
    int fputs(char const *buffer, FILE * stream);
    
    // fputs writes the characters in buffer as-is to stream
    // buffer MUST be a valid, NULL-terminated string
    // i.e. if buffer has 0, 1, or n newlines, fputs will write 0, 1, or n newlines
    
    // returns >=0 if operation is successful and EOF otherwise
```

## Formatted Line I/O

### scanf Family
```C
    int fscanf(FILE * stream, char const *format, ...);
    int scanf(char const *format, ...);
    int sscanf(char const *string, char const *format, ...);
    
    // fscanf: reads format from stream into given variables and returns number of variables successfully read into
    // scanf: reads format from stdin into given variables and returns number of variables successfully read into
    // sscanf: reads format from string into given variables and returns number of variables successfully read into
```

### printf Family
```C
    int fprintf(FILE *stream, char const *format, ...);
    int printf(char const *format, ...);
    int sprintf(char *buffer, char const *format, ...);
    
    // fprintf: writes variable-interpolated format string to stream
    // printf: writes variable-interpolated format string to stdout
    // sprintf: writes variable interpolated format string into buffer array (must ensure buffer is large enough)
```

## Binary I/0

### Input
```C
    size_t fread(void *buffer, size_t size, size_t count, FILE *stream);
    
    // reads count elements of size size from stream into buffer
    // buffer: array of one or more values
    // size: size of each value
    // count: number of elements to be read
    // returns the number of elements (NOT bytes) successfully read
```

### Output
```C
    size_t fwrite(void *buffer, size_t size, size_t count, FILE *stream);
    
    // writes count elements of size size from buffer into stream
    // buffer: array of one or more values
    // size: size of each value
    // count: number of elements to be read
    // returns the number of elements (NOT bytes) successfully written
```

## Flushing and Seeking

- `int fflush(FILE *stream)`: forces buffer to be written to stream even if buffer isn't full
- `long ftell(FILE *stream)`: returns offset from beginning of stream at which next read/write will occur (in bytes)
- `int fseek(FILE *stream, long offset, int from)`:
  - changes the position as the next read/write will occur
  - if `from` is `SEEK_SET`, `offset` bytes will be sought from beginning of stream (`offset` must be >=0)
  - if `from` is `SEEK_CUR`, `offset` bytes will be sought from current location in stream (`offset` can be positive or negative)
  - if `from` is `SEEK_END`, `offset` bytes will be sought from the end of the stream (`offset` may be positive or negative)
  - side effects:
    - EOF indicator of the stream is cleared
    - ugotten characters are forgotten
    - enables switching from reading/writing on streams opened for update

## Stream Error

- `int feof(FILE *stream)`: returns true if stream is at EOF
- `int clearerr(FILE *stream)`: resets error indication of stream
- `int ferror(FILE *stream)`: returns true if any read/write errors have occurred on stream
