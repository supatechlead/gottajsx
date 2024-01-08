---
title: "Using useContext Hook"
date: 2024-01-07T07:41:05+01:00
type: "post"
draft: false 
description: "How to Use useContext Hook and Combine it with useReducer"
showTableOfContents: true
tags: ["ReactJS", "State", "useContext", "useReducer"]
---

React's `useContext` hook isn't related to state. It makes it just more convenient to pass props down the component tree. Normally React props are passed from parent to child components; however, React's Context API allows it to tunnel React components in between. Thus it's possible to pass props from a grandfather component to a grandchild component without bothering the other React components in between of the chain

## useContext Hook

The `useContext` hook is used in React to subscribe to React context without introducing nesting. Context provides a way to pass data through the component tree without having to pass props down manually at every level.

Here's a general explanation of how useContext works:

### Create a Context

First, you need to create a context using the `createContext` function. This function returns an object with `Provider` and `Consumer` components.
```jsx
const MyContext = React.createContext();
```

### Provide the Context
Wrap your components (or the component tree where you want to use the context) with the `MyContext.Provider` component. This will make the context available to all the nested components.

```jsx
const MyComponent = () => {
  return (
    <MyContext.Provider value={/* some value */}>
      {/* Your components */}
    </MyContext.Provider>
  );
};
```

For examples, we can provide context as following:
> Providing a string
```jsx
import React, { createContext } from 'react';

const MyContext = createContext();

const App = () => {
  return (
    <MyContext.Provider value="Hello, this is a string value">
      {/* Other components */}
    </MyContext.Provider>
  );
};
```

> Providing an Object
```jsx
import React, { createContext } from 'react';

const MyContext = createContext();

const App = () => {
  const contextValue = {
    username: 'JohnDoe',
    isLoggedIn: true,
    role: 'user',
  };

  return (
    <MyContext.Provider value={contextValue}>
      {/* Other components */}
    </MyContext.Provider>
  );
};
```

> Providing a Function
```jsx
import React, { createContext } from 'react';

const MyContext = createContext();

const App = () => {
  const handleButtonClick = () => {
    console.log('Button clicked!');
  };

  return (
    <MyContext.Provider value={{ handleClick: handleButtonClick }}>
      {/* Other components */}
    </MyContext.Provider>
  );
};
```

> Providing State and Dispatch with useReducer
``` jsx
import React, { createContext, useReducer } from 'react';

const MyContext = createContext();

const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

const App = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <MyContext.Provider value={{ state, dispatch }}>
      {/* Other components */}
    </MyContext.Provider>
  );
};
```
In this last example, components wrapped by `MyContext.Provider` can access both `state` and `dispatch` from the `useReducer` hook, allowing for more complex state management.


### Consume the Context using useContext
Now, in any component within the context, you can use the `useContext` hook to access the current value of the context.

```jsx
import React, { useContext } from 'react';

const MyChildComponent = () => {
  const contextValue = useContext(MyContext);

  // Now you can use contextValue in your component
  // ...
};
```
The `useContext` hook takes the context object created with `createContext` as its argument and returns the current context value.

Here's an example of how to use `useContext`:

```jsx
import React, { createContext, useContext } from 'react';

// Create a context
const MyContext = createContext();

// A component that uses the context
const ChildComponent = () => {
  // Consume the context value using useContext
  const contextValue = useContext(MyContext);

  return (
    <div>
      <p>Value from context: {contextValue}</p>
    </div>
  );
};

// The main component that provides the context value
const ParentComponent = () => {
  // Value to be provided to the context
  const contextValue = 'Hello from the parent component';

  return (
    // Provide the context value using MyContext.Provider
    <MyContext.Provider value={contextValue}>
      {/* ChildComponent can access the context value */}
      <ChildComponent />
    </MyContext.Provider>
  );
};

// The App component that renders the parent component
const App = () => {
  return (
    <div>
      <h1> useContext Example </h1>
      <ParentComponent />
    </div>
  );
};

export default App;
```

In this example:

* `MyContext.Provider` in `ParentComponent` provides the context value ('Hello from the parent component').
* `ChildComponent` uses `useContext(MyContext)` to access the provided value from the nearest `MyContext.Provider` ancestor (`ParentComponent`).
* When `App` is rendered, it will display the value from the context in the `ChildComponent`.

## Combining useContext and useReducer

Combining `useContext` with `useReducer` is a common pattern in React applications. This combination allows you to manage complex state logic in a more organized and scalable way. Here's a general guide on how you can achieve this:

### Create a Context:

Start by creating a context using `createContext`. This context will be used to provide the `state` and `dispatch` function to the components that need them.

```jsx
import React, { createContext, useReducer, useContext } from 'react';

// Create a context with an initial value (can be null)
const MyContext = createContext(null);
```

### Create a Reducer:

Define a `reducer` function that will handle state transitions based on dispatched actions.

```jsx
const initialState = { /* your initial state here */ };

const reducer = (state, action) => {
  switch (action.type) {
    case 'UPDATE_DATA':
      // handle the state update based on the action
      return { ...state, data: action.payload };

    // Add more cases as needed

    default:
      return state;
  }
};
```

### Create a Context Provider:

Use the `useReducer` hook to create a `state` and `dispatch` function. Wrap your components with the `MyContext.Provider` and provide the `state` and `dispatch` function.

```jsx
const MyContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <MyContext.Provider value={{ state, dispatch }}>
      {children}
    </MyContext.Provider>
  );
};
```

### Use useContext in Components:

In the components where you need access to the `state` or `dispatch` function, use the `useContext` hook to consume the context.

```jsx
const MyComponent = () => {
  const { state, dispatch } = useContext(MyContext);

  // Now you can use state and dispatch in your component
  // ...

  return (
    <button onClick={() => dispatch({ type: 'UPDATE_DATA', payload: newData })}>
      Update Data
    </button>
  );
};
```

Here, dispatch is a function you can use to send actions to the reducer, and state is the current state of your application.

### Wrap your App with the Context Provider:

Finally, wrap your main component (often `App`) with the `MyContextProvider` to make the context available throughout your application.
```jsx
const App = () => {
  return (
    <MyContextProvider>
      {/* The rest of your components */}
    </MyContextProvider>
  );
};
```

This structure allows you to manage state and actions in a centralized way, making it easier to maintain and update your application's state logic.