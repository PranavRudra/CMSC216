# System Level I/O

## File Data Structures

- each process has a *file descriptor table* which maps integers to open files
  - standard input: file descriptor 0 or STDIN_FILENO (preferred)
  - standard output: file descriptor 1 or STDOUT_FILENO (preferred)
  - standard error: file descriptor 2 or STDERR_FILENO (preferred)
- kernel maintains *inode table* for each open file
  - shared across all processes
  - contains info about:
    - permissions
    - size
    - file type
- kernel maintains an *open file table* which contains info about each open file
  - shared across all processes
  - contains info about: 
     - file position: what position file descriptor is at in the file
     - reference count: number of file descriptors pointing to file
     - inode pointer: pointer to file's entry in kernel's inode table
  - file entries are removed from the open file table once reference count is 0

## System Calls

### open()
```C
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <fcntl.h>
    int open(const char *filename, int flags);
    
    // gives a process access to a file by returning a file descriptor to it
    // returns -1 on error and file descriptor for opened file on success
    // flags is bitwise OR of O_RDONLY, O_WRONLY etc...
```

### close()
```C
    #include <unistd.h>
    int close(int fd);
    
    // releases system resources used by file with file descriptor fd
    // returns -1 on error or 0 if operation is successful
    // errors can happen if you try to free a freed descriptor OR if something interrupts operation
```

### read()
```C
    ssize_t read(int fd, void *buffer, size_t n);
    
    // reads up to n bytes into the buffer
    // returns -1 on error, 0 on EOF, or number of bytes read (may be < n)
```

### write()
```C
    ssize_t write(int fd, const void *buffer, size_t n);
    
    // writes up to n bytes from buffer to fd
    // returns -1 on error or number of bytes written (may be < n)
```