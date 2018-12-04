# Testing

## Types

- *white box testing*: testing when you know have access to the source code
- *black box testing*: testing when you don't have access to the source code
  - black box tests usually check things like functionality
- *regression testing*: testing to ensure that new additions do not break the build

## Coverage

- *line (statement) coverage*: each line of code is run at least once in testing
- *branch coverage*: each block of code is run at least once in testing
- *path coverage*: each flow of program execution is covered at least once in testing

```C
    // weakness in line coverage
    int *p;
    if (n > 0) {
        p = malloc(sizeof(*p));
    }
    *p = n;                             // line coverage-oriented tests would not test what happens if malloc failed
    
    // weakness in branch coverage
    if ((w && x) || (y && f(z)) {       // branch coverage-oriented tests would not check all of the possible ways
        ...                             // the if condition could evaluate to true. for example, tests might only
    }                                   // hit the (w && x) condition without covering the (y && f(z)) condition etc...
```
