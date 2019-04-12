---
title: Recursion
created: '2019-03-25T10:08:07.861Z'
modified: '2019-03-30T21:39:24.596Z'
tags: [fundamentals]
---

# Recursion

## functions that call themselves

- factorials

- all the numbers leading up to a number multiplied by one another 

- e.g 5 = 5x4x3x2x1

- 1=1

```js
function factorial(){
  let total = 1;
  for(let i = n; i > 1; i--){
    total *=i
  }
  return total
}

const result = factorial(3)
```

```js
function recursiveFactorial(n){
if(n === 1){return 1}
//recursive case
return n* recursiveFactorial(n-1)
}


```

## Base case
- dont need recursion to work out the answer
- the point where it will stop recurring
## Recursive case
- defined as when the function is going to call itself
- must do 2 things 1. actually call itself 2. closer to the base case
- usualy lead to neater code but are more computationally expensive when compared to iteration.
- add a termination case for error handling

## Writing recursive functions
- when using TDD always use the base case as a starting point
```js
function sum(n){
  if(n = 1){return 1}
  return n + sum(n-1)
}
```

- count length

```js
function countString(str){
 if(str === '') {return 0}
 return 1 + countLength(str.slice(1))
}

``` 
- count the mitch
```js
const tutors = ['Mitch']

function countTutors(tutors, name){
  const total = 0 
  for (let i = 0; i < tutors.length; i ++){
    // base case
    if(tutors[i]=== name){
      total ++
    }
    ///// recursive case
    if(Array.isArray(tutors[i])){
      total+= countTutors(tutors[i], name)
    }
    
  }
return total
}

```













