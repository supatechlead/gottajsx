---
title: "React Redux Tutorial"
date: 2024-01-04T07:41:05+01:00
type: "post"
draft: false 
description: "Introduction to React Redux"
showTableOfContents: true
tags: ["ReactJS", "State", "Redux"]
---

Redux is a state management library that can be used with any UI layer, but it is commonly associated with React. It provides a predictable state container, making it easier to manage and manipulate the state of your application. The state in Redux is stored in a single JavaScript object called the "store."

## Javascript Redux Reminders

Redux library has already be introduced in a previous blog article. As a reminder, we briefly bring to ming the some basic concepts related to Action, Reducer and Store

### Redux Action

An action in Redux is a JavaScript object. It has a type and an optional payload. The type is often referred to as action type. While the type is a string literal, the payload can be anything from a string to an object.
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
Executing an action is called dispatching in Redux. You can dispatch an action to alter the state in the Redux store. You only dispatch an action when you want to change the state.

### Reducer

A reducer is a pure function. A reducer has two inputs: state and action. The state is always the global state object from the Redux store. The action is the dispatched action with a type and optional payload.

```javascript
function reducer(state, action) {
  switch(action.type) {
    case 'INCREMENT' : {
      // do something and return new state
    }
    case 'DECREMENT' : {
      // do something and return new state
    }
    default : return state;
  }
}
```

### Store

In Redux, a store is an object that holds the state of your application. It plays a central role in managing the state and serves as a single source of truth. The store holds the complete state tree of your application, and you can access the state, dispatch actions to change the state, and subscribe to changes.

Here's how you can create a Redux store:

1. Create a Reducer:
> reducer.js
```javascript
const initialState = 0;

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};

export default counterReducer;
```

2. Create a Store: Once you have your reducer, you can create a Redux store using the createStore function from the Redux library.
> index.js
```javascript
// index.js or main file in your application
import { createStore } from 'redux';
import counterReducer from './reducer'; // Assuming your reducer is in a file called reducer.js

// Create the Redux store
const store = createStore(counterReducer);
```

3. Accessing the state: You can access the current state of the store using the getState method:
```javascript
const currentState = store.getState();
console.log(currentState); // Outputs the current state of the store
```

4. Dispatching Actions: To update the state, you dispatch actions using the dispatch method:
``` javascript
store.dispatch({ type: 'INCREMENT' });
```
This action will be processed by the reducer, and the state will be updated accordingly.

## React Redux

To integrate Redux with React, you'll need the `react-redux` library, which provides the Provider component and the connect function to connect your React components to the Redux store. Here's a step-by-step guide.

### Install react-redux:
First, install the `react-redux` library alongside redux.
```bash
npm install react-redux
```

or

```bash
yarn add react-redux
```

### Create a Reducer:
As mentioned before, you'll need a reducer to specify how the state changes in response to actions. 
> reducer.js
```jsx
const initialState = 0;

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
};

export default counterReducer;
```

### Create a Redux Store and Wrap Your App with Provider:
In your main file (e.g., `index.js`), create the Redux store and wrap your React app with the `Provider` component.

> index.js
```jsx
import React from 'react';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import counterReducer from './reducer';
import App from './App'; // Replace with your main App component

// Create the Redux store
const store = createStore(counterReducer);

// Wrap your app with the Provider and pass in the store
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### Connect Your React Component:
In your React component, you can use the `connect` function to connect the component to the Redux store. This is typically done in a separate file from your component.

> Counter.js
```jsx
import React from 'react';
import { connect } from 'react-redux';

const Counter = ({ count, increment, decrement }) => {
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

// Map state to props
const mapStateToProps = (state) => ({
  count: state,
});

// Map dispatch to props
const mapDispatchToProps = (dispatch) => ({
  increment: () => dispatch({ type: 'INCREMENT' }),
  decrement: () => dispatch({ type: 'DECREMENT' }),
});

// Connect the component to the Redux store
export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```
Now, your Counter component can access the Redux state (`count` in this case) and dispatch actions (`increment` and `decrement`) as props.

## SheetCheats

### How to Dispatch Action with React Redux ?

