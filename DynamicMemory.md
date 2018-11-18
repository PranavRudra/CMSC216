# Dynamic Memory Allocation

## Heap Management

- *free list* keeps track of which memory regions are free and which are in use
- `void sbrk(int incr)`: system call that adds >= incr bytes to data segment

### Manager Goals
  - average of requests must be fast (its OK if occasional calls are slow)
  - minimize fragmentation (wasted space in heap)
    - *internal fragmentation*: memory wasted within a memory allocation
    - *external fragmentation*: memory wasted between memory allocations

### Allocation Strategy

- *first fit* is storing data in the first possible free block 
- *best fit* is storing data in the free block minimizing fragmentation
- *coalescing* merges adjacent free blocks by updating first block's to include the total space available

## Virtual Memory

- hardware memory is called *physical memory* while program memory is called *virtual memory*
- hardware memory is divided up into *pages* (each containing 4096 bytes on GRACE)

### Address Translation

- OS's *page table* stores virtual -> physical memory mapping
- hardware's *memory management unit* translates between the two
- address translation performed whenever memory location referenced
  - *low order bits* (i.e. rightmost 12 on GRACE) give offset into page
  - *high order bits* (i.e. remaining left bits on GRACE) used to index into page table

```C
    // example
    
    // given memory address 0x00200068
    // low order bits: 068 (104 in base-10)
    // high order bits: 200 (i.e. 512 in base-10)
    
    // if virtual memory address 512 (base-10) stored at physical address 4000 (base-10) then
    // the referenced memory location must be located at 4104 (base-10), OS transacts with this
    // memory location
```
