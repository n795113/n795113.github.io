---
title: >-
  Hexo: Accessing non-existent property 'xxx' of module exports inside circular dependency
date: 2022-03-18 10:08:04
categories: troubleshooting
tags:
  - hexo
  - troubleshooting
---

## Error
I got this error when I run `hexo server`. The site still works but it made feel unsatisfied...
```                           
(node:87224) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:87224) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:87224) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:87224) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:87224) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:87224) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
```

### My dependencies:
- node: v16.8.0
- hexo: 6.1.0
- hexo-cli: 4.3.0

---

## Solution

Append below code to `package.json` then run `yarn install` or `npm install`. The error should be fixed.
```
"resolutions": {
  "stylus": "^0.54.8"
}
```

---

## Reference

- https://www.haoyizebo.com/posts/710984d0/