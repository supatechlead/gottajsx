---
title: "ReactJS useReducer Hook"
date: 2024-01-03T07:41:05+01:00
type: "post"
draft: false 
description: "Introduction to useReducer Hook for State Management"
showTableOfContents: true
tags: ["ReactJS", "State", "UseReducer"]
---

React's `useReducer` hook derives from the concept of a JavaScript Reducer. A reducer is a function that takes as input parameters the current state and an action containing a payload, then returns a new computed state.

## JavaScript Reducer

Reducers are there to manage state in an application. In the context of Redux, reducers and actions are fundamental concepts that play crucial role in state management

### Action Definition and Usage

An action is an object that describes a change you want to make in your application's state. Actions must have a `type` property, which is a string that describes the type of action being performed.

Actions may include additional data (payload) needed to make the state change.
Here's examples if `increment`and `decrement` action, with repectively `INCREMENT`and `DECREMENT`types, and respective 1 and 2 payloads
```javascript
const incrementAction = {
  type: 'INCREMENT',
  payload: 1,
};

const decrementAction = {
  type: 'DECREMENT',
  payload: 2,
};
```

### Reducer Definition and Usage

A reducer is a pure function that specifies how the state of your application should change in response to an action. It takes the current state and an action as arguments, and it returns the new state.

The reducer is responsible for handling different types of actions and determining how each action should impact the state.
It should not modify the existing state; instead, it should return a new state object.

Here's an example of a simple reducer that increment or decrement a counter with an action payload:
```javascript
const initialState = {
  count: 0,
};

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + action.payload };
    case 'DECREMENT':
      return { ...state, count: state.count - action.payload };
    default:
      return state;
  }
};
```

### Dispatching Actions

The `dispatch` function is a function that sends actions to the reducer. In our current example, we could use it as:
```javascript
dispatch({ type: 'INCREMENT' });
console.log(state); 

dispatch({ type: 'DECREMENT' });
console.log(state);
```

## useReducer Hook

The `useReducer` hook is a React hook that provides an alternative to the `useState` hook when dealing with more complex state logic. While `useState` is commonly used for managing simple state, `useReducer` is useful when the state transitions depend on the previous state or when the state logic is more complex.

### useReducer Syntax

We use `useReducer`with following syntax:
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```
* `state`: Represents the current state.
* `dispatch`: A function used to dispatch actions to the reducer.
* `reducer`: A function that specifies how the state should be updated in response to dispatched actions.
* `initialState`: The initial state of the component.

### useReducer Example 1 
A basic example of `useReducer`could be:
```jsx
import React, { useReducer } from 'react';

// Reducer function
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};

// Component using useReducer
const Counter = () => {
  // Initial state
  const initialState = { count: 0 };

  // Destructuring the state and dispatch function from useReducer
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
    </div>
  );
};

export default Counter;
```
In this example, the reducer function handles different actions (`INCREMENT` and `DECREMENT`) and updates the state accordingly. The `useReducer` hook returns the current state and a dispatch function, which is used to dispatch actions to the reducer.

### useReducer Example 2
The `dispatch` function in React's `useReducer` hook primarily uses the `type` property of the action object to determine which case to execute in the reducer. However, the entire action object is passed to the reducer, and it can contain other properties as well, such as `payload` to provide additional data or any other custom properties you might need.

So, while `type` is the conventionally used property to specify the action type, the `payload` or other properties can be included to convey additional information to the reducer. This is particularly useful when your actions need to carry data along with them to describe how the state should change.

Here's an example to illustrate:
```jsx

const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { state..., count: state.count + action.payload };
    case 'SET':
      return { state..., count: action.payload }; // Setting the count to a specific value
    default:
      return state;
  }
};

// ...

// Dispatching actions with payload
dispatch({ type: 'INCREMENT', payload: 5 }); // Increment the count by 5
dispatch({ type: 'SET', payload: 10 }); // Set the count to 10

```
In this example, the `payload` property is used to pass additional information to the reducer. 