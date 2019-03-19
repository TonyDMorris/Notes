---
title: testing-101
created: '2019-03-19T20:32:06.353Z'
modified: '2019-03-19T20:38:20.015Z'
tags: [fundamentals]
---

# Testing 101

- As developers we spend more time debugging code than actually writing it.
- The code we write is not just for us. Our colleagues and potentially our users will see it, use it and try to understand it.
- Code also needs to be maintaine.
- Importance of writing readable, maintainable code.

- Importance of debugging through examples:

  - [First record of a bug by Grace Hopper (1945)](https://thenextweb.com/shareables/2013/09/18/the-very-first-computer-bug/#.tnw_KA5QF5IB)
  - USS Yorktown submarine had a division by zero error that brought down the network and the propulsion system
  - [A \$125M Mars Rover crashed in 1999 because of conflicts between metric and imperial units.](https://www.wired.com/2010/11/1110mars-climate-observer-report/)

- Broad difference between an error and a bug:

  - Errors are incorrect code. They are fairly straight forward to find and fix because they generally stop the execution of our code.
  - Bugs are code that is valid but doesn't behave the way we expected.

- Key concept: expectations vs. reality.

- Ways to prevent bugs:

  - Static typing (typed languages, TypeScript, Flow): the compiler lets us know of problems with data types
  - Linting: statically analyse code (not running it!) to catch formatting errors that could lead to run-time errors. Limited efficacy due to static nature but good for consistent code style.
  - Testing: a series of automated tasks that check the result or behaviour of our code against expectations.

- Types of tests:
  - Unit: input-output tests of small units of code (generally functions)
  - Integration: testing multiple units working together
  - End to end: pretending you are the user. Test the application flow as one single system.
- Cost and Speed: unit < integration < E2E

### Example #1 `sum.js`

```js
function sum(a, b) {
  return +a + +b;
}

module.exports = sum;
```

- How do we know that the code we just wrote does what we intended?
- Beginner's approach: log results to the console, see if they are correct
  - Writing readable, maintainable code
  - Code is for other humans
  - Programming as an iterative process

* Show "vanilla" testing.
* Refactor to use `chai.expect` and run with `mocha`

## Unit testing

- Approach testing from use cases, not particular lines of code.
- ^Same as test functionality not implementation details
- Testing a particular implementation is brittle because if you change the way you do something, your tests will need to change too.
- You want to see your tests fail to avoid false positives
- Tests after all are also code that could contain bugs
- For that reasons you want to keep your tests straight forward, simple, DRY, avoid places for typos with variables defined up top, use factory functions to simplify creation of test data.

### Example #2 `sumArgs`

```js
function sumArgs(...args) {
  return args.reduce((acc, num) => acc + num, 0);
}

module.exports = sumArgs;
```

1. We first decide what functionality we want to implement.
2. Write a test for it, watch it fail (red)
3. Write the minimum amount of code that will make that test pass (green)
4. Refactor code if necessary
5. Repeat.

```js
const sumArgs = require('../index');
const chai = require('chai');
const expect = chai.expect;

describe('sumArgs', () => {
  it('returns a single passed number', () => {
    const actual = sumArgs(1);
    const expected = 1;
    expect(actual).to.equal(expected);
  });
  // tests increasing in complexity...
});
```

### Example #3 `checkPasswordStrength`

```js
function checkPasswordStrength(password) {
  if (password.length === 0) {
    return false;
  }
  if (/[0-9]/.test(password) && password.length >= 8) {
    return true;
  } else {
    return false;
  }
}

module.exports = checkPasswordStrength;
```

## TDD on existing code

- Whenever you find a bug, don't just fix it.
- Write a test for it and watch it fail
- Then fix it

* The advantages of TDD include:

  - We ensure that we are always writing code under solid foundations because we always write new code on top of previously tested code.
  - By writing the minimum amount of code to pass each test we make sure that our functions are short and to the point, they should do one thing and do it well. This is a very sound approach to writing readable and maintainable code.
  - Our tests act as documentation to our code, they describe its correct behaviour, making it objectively easier to understand.

* Writing minimal amount of code and keeping the RGR loop tight helpss us clearly see which parts of our code break stuff.
* Less code is better! Simple code is easier to debug!

### Misconceptions about TDD

- Test code is simple, repetitive and not _real_ code.
- Writing tests slows down the development process.
- Writing tests is boring and optional. It's a non-essential nice-to-have.

### Reality

- Testing software is a complex and far from trivial task.
- If you think it's boring and not real code it probably means that you are doing it wrong.
- Research studies demonstrate that TDD and pair-programming are the two techniques that have the most impact in the quality of the code developers write.
- We write code for the future. TDD practices have the potential to slightly slow down the development now, but the time savings in maintenance later completely outweight them.

Remember this:

> TDD practices are so mature in the industry that, in a production environment, untested code will be treated as code that doesn't work correctly.
