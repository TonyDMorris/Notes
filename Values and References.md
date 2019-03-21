---
title: Values and References
created: '2019-03-20T08:31:22.333Z'
modified: '2019-03-20T09:53:22.311Z'
tags: [fundamentals]
---

# Values and References

- data types
- mutables
- comparisons
- object destructuring


## data types 
primatives{
- numbers
- strings
- booleans
- undefined
- null typeof returns an object "is an error in javaScript"
- symbols they exist es 9?
}
- objects{
  - arrays
  - functions
  - objects
}

## primatives

### they are immutable

cannot replace particular values only replace the whole value
when copied a copy of the value at that point in time is made
### cannot add properties
as in object properties

### cannot overwrite properties on primatives also

## objects

### they are mutable

when you copy an object you only copy the reference to that object so any props added to the copy are also added to the original object.


when a comparison is made between an original object and a reference the comparison is made on the loaction in memory

so the reference must be the same.

if 2 identical objects are compared this returns false.

### arrays 

## chai eql just tests value equal is a strict test of memory reference 





