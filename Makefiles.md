# Makefiles

## Algorithm

```make
    make x:
    
        for every d in x's dependencies where the makefile contains a target named d
            perform "make d"
            
        if (x does not exist) OR (x's dependencies has an element newer than x)
            perform "x's action"
```
