# Concurrent Programming

## POSIX Threads

- unique:
  - runtime stack (including stack pointer, frame pointer, program counter)
  - register values
  - thread ID
- shared:
  - process ID
  - heap memory
  - file descriptor table
  - virtual address space
  - global/static memory

## Functions (pthread.h)

- must link pthread library at compile time with `-lpthread`

```C
    int pthread_create(pthread_t *tid, pthread_attr_t *attr, void *(*func)(void *), void *arg);
    
    returns 0 on success and nonzero on error
    
    pthread_t *tid: pointer to variable in which thread ID will be stored
    pthread_attr_t *attr: attribute of the created thread (we'll use NULL)
    void *(*func)(void *): pointer to function to run (MUST have a single void * argument and void * return value)
    void *arg: pointer to function argument (if you need to pass in multiple things, make a struct and pass in a pointer)
```

```C
    pthread_t pthread_self(void);
    
    returns the thread ID of the caller
```

```C
    void pthread_exit(void *retval);
    
    terminates the calling thread and returns retval
    retval can be obtained by the reaping thread
```

```C
    int pthread_cancel(pthread_t tid);
    
    requests termination of another thread with thread ID tid
    returns 0 on success and nonzero on error
```

## Thread Termination

- *implicit termination* when thread finishes running the function it's supposed to run
- *explicit termination* by calling the `pthread_exit()`
- *thread cancellation* by calling `pthread_cancel` to terminate a specific thread
- *process termination* where entire process (and all threads) are terminated if any thread calls `exit()`
