---
title: async
created: '2019-04-01T09:02:09.036Z'
modified: '2019-04-01T10:04:13.652Z'
---

# async

## node
was originally invented to execute code away from the browser ,
one of the primary reasons was I/O

node fs has built in async and snc functions for file system
dealing with a sync we have continuation passing style.{
  instead of return ing we pass around call back functions to deal with output of async functions
}


these functions will always have the following form, (error, result) this means error comes first.




## non-blocking I/O
any time cpu communicates with something external be it over a network or locally 

setTimeOut

task queue will not push items to the execution stack until global execution context is poped off.

all blocking code must be completed before the setTimeOut will be pushed to the stack.

##blocking
```js
const doMoreWork = ()=>{
  console.log('doing more work)
}

const blockFor3Seconds= ()=>{
  Date.now()
}
```

## callback functions

readFile('path', 'utf8),()=>{})callback function!!!!
