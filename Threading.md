# Concurrent Programming

## POSIX Threads

- execution is interleaved via *context switches*

### Unique

- runtime stack (including stack pointer, frame pointer, program counter)
- register values
- thread ID

### Shared

- process ID
- heap memory
- file descriptor table
- virtual address space
- global/static memory

## Functions (pthread.h)

- must link pthread library at compile time with `-lpthread`

```C
    int pthread_create(pthread_t *tid, pthread_attr_t *attr, void *(*func)(void *), void *arg);
    
    // returns 0 on success and nonzero on error
    
    // pthread_t *tid: pointer to variable in which thread ID will be stored
    // pthread_attr_t *attr: attribute of the created thread (we'll use NULL)
    // void *(*func)(void *): pointer to function to run (MUST have a single void * argument and void * return value)
    // void *arg: pointer to function argument (if you need to pass in multiple things, make a struct and pass in a pointer)
```

```C
    pthread_t pthread_self(void);
    
    // returns the thread ID of the caller
```

```C
    void pthread_exit(void *retval);
    
    // terminates the calling thread and returns retval
    // retval can be obtained by the reaping thread
```

```C
    int pthread_cancel(pthread_t tid);
    
    // requests termination of another thread with thread ID tid
    // returns 0 on success and nonzero on error
```

## Thread Termination

- *implicit termination* when thread finishes running the function it's supposed to run
- *explicit termination* by calling the `pthread_exit()`
- *thread cancellation* by calling `pthread_cancel` to terminate a specific thread
- *process termination* where entire process (and all threads) are terminated if any thread calls `exit()`

## Reaping Threads

```C
    int pthread_join(pthread_t tid, void **retval);
    
    // reaps the thread with thread ID tid and frees memory resources associated with it
    // blocks until thread with thread ID tid terminates (has no effect if already terminated)
    // returns 0 on sucess and nonzero on error
    
    pthread_t tid: the thread ID of the thread to be reaped
    void **retval: stores void * return value of reaped thread
```

## Thread Detachment

- *joinable threads*: default where threads can be killed or reaped by other threads
- *detached threads*: cannot be killed or reaped by other threads 
  - memory resources are freed upon thread's termination

```C
    int pthread_detach(pthread_t tid);
    
    // detaches thread with thread ID tid
    // returns 0 on success, nonzero on error
    // thread can detach itself if pthread_self() passed in
```

## Semaphores (semaphore.h)

- *critical section*: instructions that should NOT have execution of other threads interleaved with them
- *race conditions*: program depends on a thread reaching a point X before another thread reaches a point
- *semaphores* are special global variables with integer counters that are changed by two atomic operations
  - P(s) or *wait operation*
    - blocks calling thread until the counter of s is greater than 0
    - if counter of s is already greater than 0, does NOT block
    - either way, decrements the counter of s
  - V(s) or *post operation*
    - increments the counter of s
    - may cause a thread blocked to unblock

```C
    int sem_init(sem_t *sem, 0, unsigned int value);        // initializes initial value to value (use 1 for binary semaphore)
    int sem_wait(sem_t *sem);                               // performs the P(s) wait operation on the semaphore
    int sem_post(sem_t *sem);                               // performs the V(s) post operation on the semaphore
    
    // all functions return 0 on sucess and nonzero on error
```

## Synchronization

- *mutual exclusion*: one thread executing a critical section blocks the others (implemented via binary semaphores)
- *condition synchronization*: thread can only access critical section if it satisfies some condition
  - usually implemented via non-binary semaphores