# Process Control

## OS background

### Process
- abstraction consisting of a program's thread(s) and its (their) associated *address space(s)*
- address space consists of:
  - *text segment*:
    - unused space
    - program text
  - *data segment*:
    - heap memory
    - runtime stack
- *program counter* (PC) keeps track of the instruction to be executed next by process
- address spaces of processes are independent and generally cannot be modified by other processes

### Kernel
- *kernel*: core part of operating system that is always in memory
  - program must switch from *user mode* to *kernel mode* to execute *privileged instructions* (i.e. system calls)
  - kernel essentially jumps from user's code (user mode) to execute system call (kernel mode) and then back

### Multitasking
- *multitasking*: kernel swaps out processes periodically giving the illusion that they are all running simultaneously
  - process's state is stored in the process's *context*, which consists of:
    - *page table*: stores information about the process's address space
    - *process table*: contains information about the process
    - *file table*: lists files opened by process

### Signals

- message to process to notify it of an event
- sample kernel-sent signals
  - `SIGSEGV`: segmentation fault
  - `SIGILL`: illegal instruction
- sample user-sent signals
  - `SIGINT`: user-interrupt (Ctrl-C)
  - `SIGTSTP`: terminal stop (Ctrl-Z)

## Linux Commands

### Viewing Processes

- `pgrep [name]`: lists ID's of processes with name in their name
- `Ctrl-Z, bg, &`: run the current process in the background
- `fg [job]`: brings background job to foreground
- `jobs`: lists processes running in background
- `ps`: lists the current processes

### Killing Processes

- `pkill (optional -9) [name]`: kills processes with name in their names
- `kill (optional -9) [pid]`: kills process with the given process ID

## System Calls

### fork()

- clones the calling process
  - child process gets its own private address space
  - child process gets its own unique process ID
  - child process inherits parent's open file descriptors
- called once but returns twice
  - returns nonnegative process ID of child to parent
  - returns 0 to the child process
  - returns -1 to parent if error occurred
```C
    #include <sys/types.h>
    #include <unistd.h>
    pid_t fork(void);
    
    pid_t getpid(void);     // returns ID of current process
    pid_t getppid(void);    // returns ID of parent process
```