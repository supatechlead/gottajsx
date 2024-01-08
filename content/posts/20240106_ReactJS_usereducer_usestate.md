---
title: "Using useReducer and useState Together"
date: 2024-01-06T07:41:05+01:00
type: "post"
draft: false 
description: "How to Use useReducer and useState Together"
showTableOfContents: true
tags: ["ReactJS", "State", "useReducer", "useState"]
---

Combining useState and useReducer in a single React component is a common practice. This combination allows you to handle different aspects of your component's state with appropriate tools. Here's an example:
```jsx
import React, { useReducer, useState } from 'react';

// Reducer function for complex state transitions
const complexReducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };

    case 'DECREMENT':
      return { count: state.count - 1 };

    default:
      return state;
  }
};

const MyComponent = () => {
  // Use state for simple, independent state updates
  const [text, setText] = useState('');

  // Use reducer for complex state transitions
  const [complexState, dispatch] = useReducer(complexReducer, { count: 0 });

  const handleIncrement = () => {
    dispatch({ type: 'INCREMENT' });
  };

  const handleDecrement = () => {
    dispatch({ type: 'DECREMENT' });
  };

  const handleChangeText = (event) => {
    setText(event.target.value);
  };

  return (
    <div>
      <p>Text: {text}</p>
      <input type="text" value={text} onChange={handleChangeText} />

      <p>Count: {complexState.count}</p>
      <button onClick={handleIncrement}>Increment</button>
      <button onClick={handleDecrement}>Decrement</button>
    </div>
  );
};

export default MyComponent;
```

In this example:

* `useState` is used to manage the text state, which represents a simple string.
* `useReducer` is used to manage the complexState state, which involves more complex transitions (incrementing and decrementing a count).

This approach allows you to use the simplicity and convenience of `useState` for straightforward, independent state updates, and leverage the predictability and maintainability of `useReducer` for more complex state logic. Depending on your specific requirements, you can choose where to use each approach within your component.
