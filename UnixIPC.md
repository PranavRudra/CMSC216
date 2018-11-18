# UNIX Interprocess Communication

## Pipes

- unidirectional IPC channel with a read and a write end
- pipe's file descriptors only visible to process that created pipe and its descendants
- reads on pipe without write end open return `EOF` while writes on pipe w/o open read end cause error
- pipe I/O is blocking
  - `read()` doesn't read data until data is in pipe 
  - `write()` doesn't write until enough space exists

```C
    #include <unistd.h>
    int pipe(int fd[2]);
    
    - fd MUST have two elements
    - fd[0] is file descriptor for read end
    - fd[1] is file descriptor for write end
    - returns 0 on success and -1 on error
    
    // read https://bit.ly/2Kbn3ED for explanation of why to close the unused ends of a pipe
```

## I/O Redirection

```C
    #include <unistd.h>
    int dup2(int old_fd, int new_fd);
    
    // system call copies old_fd's open file table entry into new_fd's open file table entry
    // new_fd is closed first if it is already opened
    // returns -1 on error and new_fd on success
```
