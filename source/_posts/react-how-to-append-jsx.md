---
title: "React: How To Append JSX"
date: 2022-03-28 21:27:22
categories: [Tech, React]
tags:
  - react
---

Use array!
```javascript
const collect = []
items.map(item => {
    collect.push(<li>{item}</li>)
})
```

React example
```javascript
import React from "react";
import ReactDOM from "react-dom";

class App extends React.Component {
  render() {
    const items = [1,2,3]
    const collect = [<li>first row</li>];
    items.map(item => collect.push(<li>{item}</li>))
    return <div>{collect}</div>;
  }
}

ReactDOM.render(<App />, document.getElementById("container"));
```

Will print out this：
- first row
- 1
- 2
- 3