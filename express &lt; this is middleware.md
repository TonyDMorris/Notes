---
title: express &lt; this is middleware
created: '2019-04-08T07:42:53.286Z'
modified: '2019-04-08T07:53:44.505Z'
---

# express <----- this is middleware

## install express and install nodemon

```js
const express = require('express')
const app =  express()
module.exports = app

app.get('/',(request, response)=>{
  res.send('hello kid')
})
```
