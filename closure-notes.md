---
title: closure-notes
created: '2019-03-22T22:28:39.532Z'
modified: '2019-03-30T21:39:24.537Z'
tags: [fundamentals]
---

# Closure

## Re-cap

- Higher order functions are functions that either take a function as an argument or return a function.
- An understanding of how varaibles are looked up in javascript according to the rules of scope.
- Understanding that js is a lexically scoped language

## Learning Objectives

- Learn how functions can be returned out of functions
- Define a closure in JS.
- Functions can carry a set of private variables in their COVE (closed over variable enviroments) that only the inner functions have access to.

## Re-cap example

When we've been through examples in the past, where we've looked at the call stack, the thread and memory.

So let's go over this one.

```js
function incrementCounter() {
  let count = 0;
  return ++count;
}

incrementCounter();
incrementCounter();
```

The function `incrementCounter` is invoked in two different places.  In each place a fresh execution context is created and a fresh `count` is declared, a local variable in that function call.


Javascript has something in the background called the garbage collector, which comes along and sweeps up all the data that isn't being used. Javascript does this automatically, but lots of other languages don't. If you end up programming in another language, you may have to do this manually!


## Returning Functions

Now, lets consider the concept of higher order functions: this is when functions can take other functions as arguments or return functions.  We've dealt with the side of higher order functions where we pass functions as arguments, but we have yet to return any. So, lets consider an example of a function that can return a function:


1. We create a function called `createTimesTwo` in the global VE
2. We create a variable called `timesTwo` in the global VE. It's value is currently undefined as it is being assigned to a function invocation.  In other words, a local execution context will be opened and the return value of `createTimesTwo` will be saved into the variable `doubleNum`
3. The return value of `returnTimesTwo` is a function, namely a reference to the function `multiplyBy2`
4. On line 67, we are saving the return value of `doubleNum` invoked with an argument of 50, into the variable `result`


```js
function createTimesTwo() {
  function multiplyBy2(n) {
    return n * 2;
  };
  return multiplyBy2;
};

const doubleNum = returnTimesTwo();
const result = doubleNum(50);
```

## Closure Enlightenment

Closure is not a special feature of the language that you choose to use when convenient. Closure is all around you in JavaScript, you just have to recognise it and embrace it. Closure happens as a result of writing code that relies on lexical scope. It just happens.

So, let's show an example by adapting what we've just written.

```javascript
function createMultiplier(mult) {
    function multiplyNum(num) {
        return num * mult;
    };
    return multiplyNum;
}

const mutiplyBy5 = createMultiplier(5);
```

So what does this show us? When we define a function, it MUST have access to the variable environment within its scope. This is called a COVE, or a closed over variable environment. In a course I watched, this was described as a little backpack that follows the function around, which is a nice way of thinking about it. 

**Definition:** Closure is when a function is able to remember and access its lexical scope even when that function is executing outside of the execution context where it was declared.  Simply define a function inside another function and expose the inner function. The inner function will have access to the variables in the outer function’s scope.


Here we have created a count function, whose count is persistent in its COVE. Every time we call it, it looks inside its lexical scope, finds the count and increments it by one.

Can we modify this counter? We cannot modify it in any way other than by using the function. It's now what's known as a private variable.

What happens if I call a new makeCounter? It will have a new lexical scope, in which the counter will once again be zero. So we can have two separate counters.

Finally, how would I make a function that takes a function as its argument, and allows it to only be called once?

```javascript
function callOnceOnly(func) {
  let isFirstTime = true;

  return function() {
    if (isFirstTime) {
      isFirstTime = false;
      return func();
    }
  };
}

function logger() {
  console.log('Hello world');
}

const singleLog = callOnceOnly(logger);

singleLog(); // Hello world
singleLog();
```

Can we access `firstTime`?
Can someone change it from the outside?

In what lexical scope is `singleLog` called?
In what lexical scope is `singleLog` defined?


### Example:

Underscore’s `once()` and `after()`

### Sources:

- [Master the JS Interview: What is a Closure? by Eric Elliot](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#.szp9dswih)
`
