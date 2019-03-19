## Introduction to NodeJS

### What is NodeJS?

- NodeJS is a runtime environment for JavaScript. Essentially it's a program that allows our computer to run programs in JavaScript through the Command Line Interface (a.k.a. CLI or terminal).
- Up until some years ago, the only environment in which we could run JavaScript code was a web browser. We know our browser understands JS because we can run it in the console (show example)
- It was used for client side scripting, code embedded in HTML documents and exacuted in the web browser, much like you're been doing in the precourse. Meanwhile serverside code, code that dealt with going to a database and inserting or retreiving information, would be coded in a different language such as Java, Ruby or C#.
- However, in 2009 someone called Ryan Dahl created NodeJS. He was frustrated by some of the limitations of the most popular web server at the time, the Apache HTTP server, and thought that by taking the JavaScript engine from the Chrome web browser and adding a few more specific features he could create a better environment for executing code on the server.
- What does 'on the server' mean? It means on someone's computer - but not necessarily their web browser. In our case, it means running code simply through the terminal.
- One reason why NodeJS gained such popularity is because it's built on top of Google's V8 JavaScript engine*, which is the same engine that the Chrome web browser uses and it's generally considered the best one (different browsers have different implementations of a JavaScript engine).
  *In this context, an engine refers to the program that interprets and executes JavaScript code.

- With the rise of NodeJS's popularity as an environment for running code, JavaScript started to move towards the server-side of the web too and now we can build whole web applications, front and back end, in JavaScript by running our front-end code in the browser and back-end code with Node.

### What will we use NodeJS for?

- NodeJS will be our main development tool for the entirety of the course. We will be using it extensively on a daily basis so it's a good idea to get familiar with it and have an interest in what we can do with it.
- We will mainly use NodeJS to execute (or run) JavaScript programs, to communicate between the server and the client side of our projects and to test our code.
- NodeJS has innumerable uses: it's very well suited for networking, we will read and write files in our computer, we will communicate with databases, etc.

### Running JavaScript code with NodeJS

- Once you have NodeJS correctly installed, you can type the `node` command in your Terminal to open the NodeJS REPL.
- REPL stands for Read-Evaluate-Print-Loop. It's essentially a program that waits for the user's input (a line of code), reads it, evaluates it and prints the result. It's a loop because after evaluating each line it will wait for another one.
- Another example of a REPL is the JavaScript console in any web browser, we can do exactly the same things in both: talk JavaScript to our computer.

```javascript
> 3 + 3
6    // <-- This is what the JS engine understands
> 21 * 2
42
>    // <-- This is the REPL waiting for a command
```

- Another way of running JavaScript code is to save a program into a file with a `.js` extension and running it like so:

```
$ node path/to/file/my-program.js
```

- This command will evaluate the code in the file, run it and stop.

Let's try running this file with NodeJS:

```javascript
// main.js
3 + 3;
```

- Why don't we see anything?
- Unlike the REPL, running a JS file with Node won't print everything the JS interpreter reads (the number 6 gets evaluated, but it disappears from memory immediately). Unless we do it explicitly:

```javascript
// main.js
console.log(3 + 3);
```

- Running this file correctly outputs the number `6` into the terminal.

<!--### Node JavaScript vs. Web Browser JavaScript

- Node's implementation of JavaScript is very similar to the one from a web browser, but it has slight differences.
  - The `global` object.
  - The `__dirname` global variable.
  - The `module` global variable.
  - The `require` function.
  - The `process` global variable.-->

### The `global` object

- It's where everything you have access to in your program lives. If you define a variable `myVar`, it will live as a property of the `global` object, so we can also access it as `global.myVar`.
- You can explore what's available on the `global` object by printing it: `console.log(global)`
- In Node, the `global` object comes with "preloaded" functions and variables like `console`, `process`, `__dirname`, `module` or `require`.

Let's have a look at what some of them do.

By the end of today you should be able to:

- Evaluate JavaScript code in the Node REPL
- Run a .js file with Node and see the results on the terminal.
- Pass arguments to a file and use them inside the program.
- Import and export modules between different files.

## NPM

- NPM (which weirdly doesn't stand for Node Package Manager) is a registry of open source JavaScript libraries that we refer to as "packages".
- It's the place where we can see, and more importantly download and use in our own projects, code that other people have written.
- One of the unwritten rules of programming is "do not reinvent the wheel" and that's the problem NPM tries to solve: if we need a function to encrypt data and we don't know how to make our own, we can quickly search for someone else's and add it as a dependency of our project.
- NPM allows us to install packages at two different levels: global (available to our entire computer) and local (only available to the current project).
- We normally install our common tools globally (for example our test runner, `mocha`) and project-specific packages locally (e.g. a Facebook API helper library).
- Initialising NPM in a project with the `npm init` command generates a file called `package.json` that keeps tracks of all the dependencies and other meta-data of our project.
- Because our dependencies can get quite large, when we share a project over version control we only share our `package.json` file, which has all the information NPM needs to re-download and re-install all the dependencies.

Let's see an example of how we will use NPM together with Node during the rest of the course.

By the end of today you should be able to:

- Initialise NPM on a project.
- Add dependencies globally and locally.
- Install the dependencies listed in a `package.json` file.
- Import project dependencies into a .js file and use them.

## NPM Scripts

In the Package.json file there is a section called scripts. It comes initialised with a test script because testing is something so common you're likely to want to do.

At a basic level, the scripts section allows us to group together scripts that we're going to use regularly in our project for easy access. For example, let's add a script to run our project.

We keep key value pairs in the scripts section. The key is the name/shortcut to the script, and the value is the actual terminal command you want to run.

For example, let's create a script to run our project. We'd usually call this a 'start' script. The terminal command for this would be `node index.js`. Then over in our terminal, instead of running `node index.js` we run `npm start` and npm looks for the start script in the package.json and runs it.

We can call a script anything we want, such as greet and the command is `echo \"Hello world"` but NPM only recognises by default a selection of script names (show link) so for anything else like greet we can type `npm run greet` and by saying `run` npm will look for greet in the scripts section.

[NPM scripts](https://docs.npmjs.com/misc/scripts)

What's the point? Isn't it just as easy to write `node index.js` in the terminal?

- Here's a test command that I used the other day. `"mocha ./src/**/*spec.js --compilers js:babel-core/register"`. That's not something I want to type out by hand every time!

- Someone else working on your project might not know whether your main file is called index.js or app.js or where that file is. Are they going to go searching through your codebase for it? Or could they just run `npm start` instead and get it running immediately?

- Another difference: If a script relies on an NPM package that is installed LOCALLY then you will ONLY be able to run it through an NPM script.

#### Common Scripts

Point out how you can use scripts that run at certain times in a project's lifecycle, such as posttest.

Point out a dependency called Husky that allows you to hook into the git lifecycle. If we clone a project from Northcoders you will see how this is set up for you to ensure you follow best practices.

[Husky](https://github.com/typicode/husky)

[Git hooks from Husky](https://github.com/typicode/husky/blob/master/HOOKS.md)

## module.exports

Another useful thing NodeJS gives us is the ability to bring in code from other files. This is called importing and exporting.

- Why

  - Seperate code into files to seperate concerns. i.e functions that have different purposes, are stored seperately.
  - Makes it easier to read

- What

  - You can export anything from a file but it must be one thing!

- How

  - module.exports
  - exports

  - The file will export whatever module.exports is.
  - exports initially points at the same thing as module.exports
  - module.exports starts as an empty object