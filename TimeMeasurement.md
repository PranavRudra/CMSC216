# Measuring Time

## Types

- *wall time*: total elapsed time taken by the program
- *process time*: amount of time processor executed process
  - *system time*: amount of time program was executed in kernel mode
  - *user time*: amount of time program was executed in user mode
  - system time + user time = process time
- *interval time*: less accurate, measures elapsed time
- *clock cycles*: more accurate, measured in terms of CPU cycles

## Tracking Time

- UNIX keeps all times in UTC so there is no timezones or daylight saving to deal with
- UNIX uses a reference starting point called *epoch* - midnight 1/1/1970

## Functions (time.h)

```C
    clock_t clock(void);                                // returns process time since start of program execution
    time_t time(time_t *val);                           // fills val with current time (implementation-dependent format)
    char *ctime(time_t *val);                           // returns string representation of time in val
    double difftime(time_t time1, time_t time2);        // returns number of seconds between time1 and time2
    struct tm *gmtime(time_t *val);                     // converts time to UTC time
    struct tm *localtime(time_t *val);                  // converts time to local time
    time_t mktime(struct tm *timeptr);                  // converts a struct tm to time_t (as seconds past epoch) 
```
