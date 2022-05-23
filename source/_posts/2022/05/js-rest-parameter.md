---
title: "Javascript: Rest Parameter"
date: 2022-05-18 16:46:49
categories: [Tech, Javascript]
tags:
  - javascript
---
```javascript
// rest.js
const row = {
    name: "Amy",
    age: 32,
    email: "amy@gamil.com",
    job: "cook"
};

const { job: a, none: b, ...rest_props } = row;

console.log(a); // maps to property "job"
console.log("---")
console.log(b); // property "none" doesn't exist so b is undefined
console.log("---")
console.log(rest_props); // row but without "job" property 
```
run the code:
```bash
$ node rest.js                                                                                           
cook
---
undefined
---
{ name: 'Amy', age: 32, email: 'amy@gamil.com' }
```

### More
- [rest parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) is not [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- https://www.freecodecamp.org/news/javascript-rest-vs-spread-operators/
- [JavaScript 里三个点 ... 的用法](https://cloud.tencent.com/developer/article/1891988)
