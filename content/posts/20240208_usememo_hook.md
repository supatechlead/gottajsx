--- 
draft : false
date : 2024-02-10T14:37:18+01:00
title : "Using React useMemo Hook "
description : "Understanding the React useMemo Hook""
tags : ["React", "Hook"]
showTableOfContents : true
type: "post"
---

React’s `useMemo` hook is a memoization function that caches a value returned by an expensive function. The goal of the optimization concept is that the execution of the expensive function runs only on the first render invoked, or it executes only when a referenced input parameter has changed its value.

## Example

Let’s take the following React Component for an example:

```jsx
import React, { useState } from 'react';

const expensiveFunction = (inputValue) => {
  let expensiveValue = inputValue * 42;
  
  ... lots and lots of computing depending on inputValue ...
  
  expensiveValue = 'World';
  return expensiveValue;
};

const MyComponent = ({ something }) => {
  const [inputValue, setInputValue] = useState('');
  const expensiveValue = expensiveFunction(inputValue);
  return <h1>Hello {expensiveValue}</h1>;
};

export default MyComponent;
```

From the code above, we can conclude that the `expensiveFunction()` will execute on every render of `MyComponent`, which is triggered by a state change of the `inputValue` or even something as a prop.

To solve this, we need to introduce `useMemo` to the `expensiveFunction()` and `inputValue` as a dependent parameter that will execute the `expensiveFunction` only if its value changes:
```jsx
import React, { useState, useMemo } from 'react';

const expensiveFunction = (inputValue) => {
  let expensiveValue = inputValue * 42;
  
  ... lots and lots of computing including inputValue ...
  
  expensiveValue = 'World';
  return expensiveValue;
};

const MyComponent = ({ something }) => {
  const [inputValue, setInputValue] = useState('');
  const expensiveValue = useMemo(
    () => expensiveFunction(inputValue), 
    [ inputValue ]
  );
  return <h1>Hello {expensiveValue}</h1>;
};

export default MyComponent;
```

The first argument to `useMemo` is a creator function (factory) that executes the `expensiveFunction` (can be written inline) and returns the `expensiveValue`, where as the second parameter is an array of the dependent parameters that will trigger/execute the calculation when `inputValue` changes.

So as long as the dependent parameter `inputValue` does not change, the `expensiveFunction` will only be executed on the first `MyComponent` render. Every consecutive render will get the cached `expensiveValue`. This way we hgit ave an optimized React Component that is light to render.



