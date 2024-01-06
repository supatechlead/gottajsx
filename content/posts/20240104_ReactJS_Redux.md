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

### How to Dispatch Actions with React Redux

#### Dispatching Actions Directly

If your action is simple and doesn't require any additional data, you can dispatch it directly:
```jsx
import React from 'react';
import { connect } from 'react-redux';

const MyComponent = ({ dispatch }) => {
  const handleClick = () => {
    // Dispatching the 'INCREMENT' action directly
    dispatch({ type: 'INCREMENT' });
  };

  return (
    <div>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
};

export default connect()(MyComponent);
```
In this example, `dispatch({ type: 'INCREMENT' })` sends an action to the Redux store with the type `'INCREMENT'`.

#### Using Action Creators:
Action creators are functions that create and return action objects. They make your code more modular and readable. Here's an example:
̀```jsx
// actionCreators.js
export const increment = () => ({
  type: 'INCREMENT',
});

// MyComponent.js
import React from 'react';
import { connect } from 'react-redux';
import { increment } from './actionCreators';

const MyComponent = ({ dispatch }) => {
  const handleClick = () => {
    // Using the 'increment' action creator
    dispatch(increment());
  };

  return (
    <div>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
};

export default connect()(MyComponent);
``` 
In this example, the `increment` action creator is imported and used to dispatch the `'INCREMENT'` action.

### How to Access State with React Redux

#### Using mapStatetoProps

The `mapStateToProps` function allows you to map the state from the Redux store to props in your component.

```jsx
// MyComponent.js
import React from 'react';
import { connect } from 'react-redux';

const MyComponent = ({ count }) => {
  return (
    <div>
      <p>Count: {count}</p>
    </div>
  );
};

// Map state to props
const mapStateToProps = (state) => ({
  count: state, // assuming your state has a 'count' property
});

// Connect the component to the Redux store
export default connect(mapStateToProps)(MyComponent);
```
In this example, the `mapStateToProps` function is used to map the `count` property from the Redux state to the count prop in your component.

#### Accessing State Directly from this.props
The mapStateToProps function allows you to map the state from the Redux store to props in your component.

If your component is connected to the store using connect without any argument, the entire state will be available in this.props. You can access it directly without using mapStateToProps.

```jsx
// MyComponent.js
import React from 'react';
import { connect } from 'react-redux';

const MyComponent = ({ dispatch, count }) => {
  return (
    <div>
      <p>Count: {count}</p>
    </div>
  );
};

// Connect the component to the Redux store
export default connect()(MyComponent);
``` 
In this example, both `dispatch` and the entire state are available in `this.props`.

#### How to Pass State from Parent to Child Component

In a React Redux application, you can pass the `count` (or any other state) down to child components as props. If `MySecondComponentChild` is a child component of `MyComponent`, you can pass the `count` prop from `MyComponent` to `MySecondComponentChild` like this:

```jsx
// MyComponent.js
import React from 'react';
import { connect } from 'react-redux';
import MySecondComponentChild from './MySecondComponentChild';

const MyComponent = ({ count }) => {
  return (
    <div>
      <p>Count: {count}</p>
      {/* Pass 'count' as a prop to MySecondComponentChild */}
      <MySecondComponentChild count={count} />
    </div>
  );
};

// Map state to props
const mapStateToProps = (state) => ({
  count: state,
});

// Connect the component to the Redux store
export default connect(mapStateToProps)(MyComponent);
```
Now, in `MySecondComponentChild`, you can access the `count` prop directly:

```jsx
// MySecondComponentChild.js
import React from 'react';

const MySecondComponentChild = ({ count }) => {
  return (
    <div>
      <p>Count in MySecondComponentChild: {count}</p>
      {/* Your child component rendering and logic */}
    </div>
  );
};

export default MySecondComponentChild;
```
By passing the `count` prop down from the parent `MyComponent` to the child `MySecondComponentChild`, you make the state available to the child component. This is a common pattern in React for passing data from parent to child components.

## React Redux Middleware

In a React-Redux application, middleware is a way to extend the behavior of the Redux store. It provides a third-party extension point between when an action is dispatched and when it reaches the reducer. Middleware can be used for various purposes such as logging, asynchronous operations, and more.

Here's a simple example using Redux middleware to log actions:

```jsx
// Install necessary packages
// npm install react react-dom redux react-redux

// Import necessary libraries
import React from 'react';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';

// Define a simple reducer
const rootReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

// Define a middleware function for logging
const loggerMiddleware = store => next => action => {
  console.log('Dispatching:', action);
  const result = next(action);
  console.log('Next State:', store.getState());
  return result;
};

// Create the Redux store with middleware
const store = createStore(rootReducer, applyMiddleware(loggerMiddleware));

// Simple React component
const Counter = () => {
  return (
    <div>
      <button onClick={() => store.dispatch({ type: 'INCREMENT' })}>Increment</button>
      <span>Count: {store.getState().count}</span>
      <button onClick={() => store.dispatch({ type: 'DECREMENT' })}>Decrement</button>
    </div>
  );
};

// Render the React app with Redux store provider
const App = () => {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
};

export default App;
```

In this example:

1. We have a simple Redux store with a reducer that manages a count.
2. We've defined a middleware function called `loggerMiddleware` that logs the action being dispatched and the next state after the action is processed.
3. The middleware is applied using `applyMiddleware` when creating the Redux store.

Now, when you dispatch actions (e.g., clicking the "Increment" or "Decrement" buttons), the middleware will log information about the action and the resulting state to the console.



