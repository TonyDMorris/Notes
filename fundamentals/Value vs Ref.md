# Value vs Ref

## Data types

The latest ECMAScript standard defines seven data types:
Values can be partitioned into two main kinds: primitives and objects.

Seven data types, 6 are what we call primitive;
**primitives**: - Boolean - Null - Undefined - Number - String - Symbol (new in ECMAScript 6)

### Primitives are immutable;

Properties on primitive values cannot be created or overwritten.

```js
const fruit = 'banana';
fruit.age = 28; // write - ignored
console.log(fruit.age); // read - undefined

const name = 'sam';
name[0] = 'S'; // write - ignored
console.log(name); // read - 'sam'

const str = '123';
console.log(str.length); // 3
str.length = 100;
console.log(str.length); // 3
```

### Primitives are compared by value

To compare primitives, JS looks at their content. If the content(values) are the same, then they are considered equal.

```js
'hello' === 'hello'; // true
'hi' === 'hello'; // false
true === false; // false
undefined === null; // false
```

For primitives, variables contain the value which means that when you copy one variable into the other, it will create a copy of the content/ the **value**

```js
const a = 'woo';
const b = 'foo';
a === b; // false
```

```js
let a = 10;
const b = a;
a = a * 2;
console.log(a); // 20
console.log(b); // 10
```

## Object data types

Objects can be partitioned further:

    - Creatable by literals. The following literals produce objects that can also be created via constructor. Use literals whenever you can.
        - [] is the same as new Array()
        - {} is the same as new Object()
        - function() {} is the same as new Function()

### Objects are mutable by default:

```js
const me = { name: 'Sam' };
me.age = 29; // write
console.log(me.age); // read - 29
```

### Objects are compared by reference

For objects (including arrays and functions), JS stores a reference to where the object is stored in memory.

Every object you create via an expression such as a constructor or a literal is considered different from every other object; a fact that can be observed via the equality operator (===). That operator compares objects by reference: two objects are only equal if they have the same identity. It does not matter whether they have the same content or not.

So when we create an object and store it in one variable and then we copy that variable into a new variable, it doesn't copy the value like it did with primitives. Instead it copies the reference. The pointer to where the object is being stored in memory.

i.e.

```js
const sam = { name: 'Sam' };
sam2 = sam;
sam.age = 30;
sam2 === sam; // true
console.log(sam); // { name : 'Sam', age: 30 }
console.log(sam2); // { name : 'Sam', age: 30 }
```

`sam` points to an object and `sam2` points to the same obj so if `sam` changes so does `sam2`.

It does because they point to the same object. For 2 variables containing objects to be equal, they must be pointing to the same object.

Different objects containing same info

```js
const a = { name: 'Paul' };
const b = { name: 'Paul' };
a.age = 32;
a === b; // false
console.log(a); // { name : 'Paul', age: 32 }
console.log(b); // { name : 'Paul' }
```

There are two seperate objects in memory therefore are not the same as to be equal must point to the same object.

## Arrays

```js
const samsFilingCabinet = [];
const mitchsFilingCabinet = [];
samsFilingCabinet === michsFilingCabinet; // false
```

If we are using separate filing cabinets, if I add something to `mitchsFilingCabinet`, what will happen?

```js
mitchsFilingCabinet.push('p45');
console.log(mitchsFilingCabinet); // ['p45'];
console.log(samsFilingCabinet); // [];
```

Even if they _look_ the same, and have the same contents (i.e. nothing), clearly when I add something to mitchsFilingCabinet, it will not be reflected in samsFilingCabinet.

But one of mitch has house fire, and his cabinet is lost to the flames, for a while, he can stay at mine and share my filing cabinet.

```js
const samsFilingCabinet = ['books'];
const mitchsFilingCabinet = samsFilingCabinet;
samsFilingCabinet === mitchsFilingCabinet; // true
console.log(mitchsFilingCabinet);
console.log(samsFilingCabinet);
```

Now what happens when mitch adds something to his filing cabinet?

```js
mitchsFilingCabinet.push('a sandwich for later');
console.log(mitchsFilingCabinet); // ['books', 'a sandwich for later'];
console.log(samsFilingCabinet); // ['books', 'a sandwich for later'];
```

Because out possesions are sharing a filing cabinet, if I add, remove or modify anything in that filing cabinet, it changes everywhere.

Each array literal creates a unit of storage, which is referenced by an address in memory. This array can be stored in multiple variable names, or even inside other array, but when I ask for a change to occur at that address, it changes _everywhere_.

## Copying an object without copying its reference

I can create a new array, with a new reference and modify it without modifying the original by using slice.

There are several ways to do this - what are they?

- for loop
- for of loop
- slice
- map/filter/reduce (not recommended - the array creation is usually incidental to the rest of the functionality)
- the spread operator

slice

```js
const people = ['Sam', 'Mauro', 'Harriet', 'Mitch', 'Jonny'];

const tutors = people.slice();

tutors.push('JD');
```

spread

=

```js
const people = ['Sam', 'Mauro', 'Harriet', 'Mitch', 'Jonny'];

const tutors = [...people, 'JD'];
```

## Objects

To do this with an object, I need to use object.assign.
Object.assign takes two arguments, a target object to copy into, and a source object to copy from.

```js
const me = { name: 'paul' };
const me2 = Object.assign({}, me);
me2.age = 32;
console.log(me); // { name: 'paul' }
console.log(me2); // { name: 'paul', age: 32 }
```

We can also use our spread syntax in a very similar way to how we would copy an array. We can create a new object literal and spread the object into it to achieve the same result. Because we are spreading to an object literal, this is a brand new object, with it's own reference.

```js
const me = { name: 'paul' };
const me2 = { ...me, age: 32 };
console.log(me); // { name: 'paul' }
console.log(me2); // { name: 'paul', age: 32 }
```

So let's say I have the following data available. I want to change age to date of birth without mutating the original array.

Let's work this out together.

```js
const people = [
  {
    name: 'Sam',
    age: 29,
  },
  {
    name: 'Jonny',
    age: 31,
  },
  {
    name: 'Mitch',
    age: 27,
  },
];

const newPeople = people.map((person) => {
  const newPerson = Object.assign({}, person);
  newPerson.dob = 2017 - newPerson.age;
  delete newPerson.age;
  return newPerson;
});
```

## Spread syntax with object literals.

The same thing done with the spread operator.

```js
const newPeople = people.map((person) => {
  const newPerson = { ...person, dob: 2017 - person.age };
  delete newPerson.age;
  return newPerson;
});
```

## Exporting Multiple values

So far we've been using require and module.exports to make variables available in other files. It is very common however to need to export multiple values from one file. We can only export one thing, so to solve this problem we export an object, with multiple keys.

Say we want to export both foo and bar from a file:

index.js

```js
function foo() {
  // logic...
}

function bar() {
  // logic...
}
```

We can export an object with two keys that contain foo and bar respectively. For clarity we'll give them the same name.

```js
module.exports = { foo: foo, bar: bar };
```

We can now use some object shorthand to achieve the same thing.

```js
module.exports = { foo, bar };
```

Both of these do the same thing. By putting a value into an object literal, it will create a key of the variables name, with a value of the variables value.

Now when we require this file, we get an object

index.spec.js

```js
const indexExports = require('../index.js'); // { foo: [Function: foo] , bar: [Function: bar] }
```

We can now define both functions using require:

```js
const foo = require('../index.js').foo;
const bar = require('../index.js').bar;
```

This could get quite long however, so we can use object destructuring to shorten it. When declaring a variable we can put curly brackets around the variable name to **destructure** that variable. This makes it equal to the property of the same name from the assigned object.

This makes our example look like:

```js
const { foo } = require('../index.js');
const { bar } = require('../index.js');
```

Better, but still on two lines. When destructuring, we can declare multiple variables at the same time.

```js
const { foo, bar } = require('../index.js');
```

Well ain't that much neater :D

n.b. If you are destructuring the variables, you must be exporting an object.
This means our exports should look the same as the require. If we want to add another export, just add it to the object. Lovely stuff! :D

```js
// index.js
module.exports = { foo, bar };

// index.spec.js
const { foo, bar } = require('../index.js');
```

## Learning Objectives

- What are primitive values in JS?
- What other data types are there?

- When you copy a primitive, it copies the value into the new variable.
- When you copy an object, it copies the reference into the new variable.
- For primitives to be ===, the value must be the same. i.e. 'banana' === 'banana'.
- For objects to be ===, the reference must be pointing to the exact same object
- There is no native deep equal function in javascript.
- If you are destructuring a variable from require you must be exporting an object.

## Reading

- http://2ality.com/2011/03/javascript-values-not-everything-is.html
