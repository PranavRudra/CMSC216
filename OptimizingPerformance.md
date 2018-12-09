# Optimizing Program Performance

## Modern Processors

- *cache*: small amount of memory keeping copies of memory locations near one that was recently accessed
  - *cache hit*: data that has been cached is request
  - *cache miss*: data that has NOT been cached is requested
- *superscalar processor*: executes two or more operations simultaneously
- *instruction pipelining*: executing parts of multiple instructions simultaneously
  - i.e. decoding one instruction while loading another one from memory
- *branch prediction*: processor predicts which way a branch will go, enabling pipeline to stay full

## Compiler Optimizations

- *constant folding*: evaluating expressions with constant operands at compile time (i.e. return 8 instead of return 3 + 5)
- *dead code elimination*: compiler will delete code that is unreachable or unused code (i.e. unused variable assignments)
- *loop unrolling*: increasing the size of the loop body (and reducing iteration count) to enable the pipeline to full

## Programmer Optimizations

- *code motion*: moving code out of a loop
- reducing unneeded memory references 
- share common subexpressions
- reducing function calls (especially in recursion)

## Principle of Locality

- *spatial locality*: programs tend to use data and instructions NEAR those used recently
- *temporal locality*: programs tend to reuse recently referenced data and instructions