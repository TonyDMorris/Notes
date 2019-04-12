---
title: Recursion_Day_2_Notes
created: '2019-03-26T15:35:12.444Z'
modified: '2019-03-30T21:39:24.589Z'
tags: [fundamentals]
---

# Recursion 102 (traversing objects)

## Prior Knowledge

- Understand key features of recursive functions.
- Understand and identify situations where recursion is advisable / necessary

## Learning Objectives

- be more confident in applying recursion to nested collections
- use VSCode's debugger to debug recursive problems
- understand potentials and drawback of other recursive structures

## Background

As discussed in the previous lecture, a common use case for recursion is to query, analyse or manipulate nested collections (arrays and objects).

Consider the following situation: three vet practices are merging their clients and so their databases need merging too, but they have all structured their data in different ways. There's more data than can be analysed by human eye, so any attempts to query the data will have to be programmatic. Here's an example of three collected cat records, from different origins, presented as an array:

```javascript
const catRecords = [
  {
    basic: {
      name: 'Bernard',
      age: 12,
      breed: 'Maine Coone',
      sex: 'male',
    },
    owner: {
      name: 'Mrs Jones',
      contact: {
        address: '55 Stretford Road, Manchester, M16 0GH',
        telephone: 07734123123,
      },
      notes: 'Payment plan B, prefers morning appointments',
    },
  },
  {
    cat: {
      name: 'Jemima',
      age: 6,
      breed: 'Tabby',
      sex: 'female',
    },
    owner: {
      name: 'Mrs Robesco',
    },
    contact: {
      address: '54 Stretford Road, Liverpool, M16 0GH',
      telephone: 07734123124,
    },
    notes: 'Payment plan B, prefers morning appointments',
  },
  {
    basic: {
      name: 'Soopy',
      age: 3,
      breed: 'Tiger',
      sex: 'male',
    },
    address: {
      houseNum: 53,
      road: 'Stretford Road',
      city: 'Leeds',
      postCode: 'M16 0GH',
    },
  },
];
```

Imagine we wanted to tally which cities our cats lived in - there is no predictable way to access this data. The data we want to access isn't even nested at a predictable level, and there's no guarantee at what level of nesting we will find it.

This is a classic problem for recursion because we can define a function that looks for a search term in a simple (flat) object and keep on re-using this function internally until we're done.

To approach a more complex recursive problem like this, break the problem down:

- what should happen on each recursive call?
- what are we trying to construct at the end of this?
- how do we get closer to our final answer?

#### Instructions

We are going to solve the `tallyCity` problem. To make it simpler, we will provide an initial value that includes the names of the cities we are looking for. Each stage created here can be tested.

1. Make a function that examines the values of a non-nested object. This is the basis of how our tally will increment - if we can solve this, we can build a recursive solution around it.

```javascript
function addCountToTally(catRecord, tally) {
  for (let recordKey in catRecord) {
    for (let city in tally) {
      if (catRecord[key].includes(city)) {
        tally[city]++;
      }
    }
  }
  return tally;
}
```

2. Currently, we asume that all values in our object are strings or arrays - values that have an `.includes` method. However, we want to account for other objects too, which may hold the desired data in. We can build a recursive case into our iteration.

```javascript
function addCountToTally(catRecord, tally) {
  for (let recordKey in catRecord) {
    for (let city in tally) {
      if (catRecord[key].constructor === 'Object') {
        tally = addCountToTally(catRecord[key], tally);
      } else if (catRecord[key].includes(city)) {
        tally[city]++;
      }
    }
  }
  return tally;
}
```

3. We can now build this function into an iteration through all the cat records in the initial array:

```javascript
function tallyCities(catRecords, initialTally) {
  return catRecords.reduce((tallySoFar, catRecord) => {
    return addCountToTally(catRecord, tallySoFar);
  }, initialTally);
}
```

By breaking down the problem into the desired behaviour, the recursive nature and the iterative approach, we have created a testable solution that shouldn't feel too overwhelming.

NB - using loops in recursion is a common solution, because there may be situation where it is optimal to break out of the loop. Array methods like `.reduce` and `.forEach` do not allow you to break from the loop.

### Debugger

VS Code has a useful debugging tool, indicated by the 'no bug entry' icon on the left hand side. It is an alternative to execution context diagrams, describing the state of the call stack and the local execution context.

It can be useful for recursive problems, as it can describe the individual parameters and return values for each invocation. This can make it easier to keep track of how the final answer is constructed.

To use it, set **breakpoints** in your code by click the red dot to the right of a line number. This indicates that you want to 'break' the code at this point, and keep track of the thread yourself. A sensible place to add a breakpoint is on the first line inside the function.

Press the green play button at the top of the debug panel. This will the break code and show the current state of the execution context, as well as the call stack.

Using the debugger can feel like an art rather than a science sometimes. In the navigation at the top you had the opportunity to _step into_ or _step over_ your code. _Into_ will complete the next evaluation and move on to the next statement. Use _over_ when you want to move past an execution context, as you may well wish to do for code which is not your own (for example, `console.log` is a remarkably in-depth process).

Whilst in debugging mode, you may wish to pay attention to:

- the call stack, which will tell you about your current invocation and any previous invocations that are yet to resolve
- the variables in the local scope or current execution context, including computed return values
- when you hover your mouse over a variable in your code, you can see its current value at this point in the execution context

## Passing Variables with Recursion

Our examples so far have shown us building up or totalling a result as the recursive invocations return after the base case. Sometimes it is more appropriate to pass your 'accumulated' result through to your next recursive call, so the entire object can be returned through the call stack when the base case is finally reached.

Contrast these solutions for `countWhiteSpace()`:

```js
function countWhiteSpace(str) {
  if (str === '') return 0;
  return str[0] === ' '
    ? 1 + countWhiteSpace(str.slice(1))
    : countWhiteSpace(str.slice(1));
}
```

or

```js
function passedWhiteSpace(str, count = 0) {
  if (str === '') return count;
  if (str[0] === ' ') count++;
  return passedWhiteSpace(str.slice(1), count);
}
```

This approach plays on our tendency to want to keep a counter in such situations. In a recursive function, it is pointless to declare a counter within the function scope, as it will reset on each invocation.

The solution is to keep track of our count by passing it as an argument from one execution context to the next.
We use this approach to build a result on each call and when we hit the base case, the return value will be the number of whitespaces. For the sake of the first invocation, we pass a default parameter (`count = 0`) so their is no type error from incrementing and it's not necessary to pass in an initial value.

A further approach would be to define a counter _out_ of scope. To do this in the global would be exceedingly bad practice as this would render our function impure, expose its behaviour to bugs, and make it difficult to test. Alternatively, you could keep the count in the scope of a helper function and modify it from within the recursive function.

```js
function helperWhiteSpace(str) {
  let count = 0;
  recursiveCount(str);
  return count;

  function recursiveCount(str) {
    if (str === '') return;
    if (str[0] === ' ') count++;
    return recursiveCount(str.slice(1));
  }
}
```

This approach is better in that it keeps our function pure and doesn't expose the count, but it does lose the concise nature and readability of a contained recursive function.
