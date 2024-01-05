---
title: "ReactJS State"
date: 2024-01-01T07:41:05+01:00
type: "post"
draft: false 
description: "Short Introduction to ReactJS Function Component State"
showTableOfContents: true
tags: ["ReactJS", "State"]
---
The UI of a frontend application is a representation of its state. State is just a snapshot in time. If a user changes state by interacting with your application, the UI may look completely different afterward, because it's represented by this new state rather than the old state.

## ReactJS State Utility


The main utility of state in React is to manage and store the dynamic data within a component. State allows components to keep track of information that can change during the lifecycle of the component, such as user interactions, asynchronous data fetching, or updates triggered by other parts of the application.

Here are some key aspects of the main utility of state in React:

1. Dynamic UI Updates: State enables components to react to changes in data and update the user interface accordingly. When the state of a component changes, React automatically re-renders the component to reflect the updated state.

2. User Interaction: State is often used to handle user interactions and store information related to user input. For example, a form component might use state to keep track of user input before submitting the form.

3. Asynchronous Data: When fetching data asynchronously (e.g., from an API), state is used to store and manage the data once it is retrieved. Updating the state triggers a re-render with the new data, updating the UI.

4. Component Reusability: State allows components to be reusable and flexible. By managing internal state, components can encapsulate their logic and be used in different parts of an application without relying on external data.

5. Local Component Logic: State helps in encapsulating the local logic of a component. Each component can have its own state, making it self-contained and independent of other components.

6. Stateful Behavior: State allows components to exhibit stateful behavior, meaning they can maintain and remember certain information over time. This is particularly important for creating interactive and responsive user interfaces.

## ReactJS State Basic Management

### ReactJS State Object

State can be various things, as examples :

1. a boolean which tells the UI that a dialog/modal/popover component is opened or closed.
2. an user object which reflects the currently signed in user of the application.
3. Data from a remote API (e.g. an object/list of users), that is fetched in React and displayed in your UI.

```jsx
// 1)
const isOpen = true;

// 2)
const user = {
  id: '1',
  firstName: 'Robin',
  lastName: 'Wieruch',
  email: 'hello@robinwieruch.com',
};

// 3)
const users = {
  2: {
    firstName: 'Dennis',
    lastName: 'Wieruch',
    email: 'hello@denniswieruch.com',
  },
  3: {
    firstName: 'Thomas',
    lastName: 'Wieruch',
    email: 'hello@thomaswieruch.com',
  },
};
```
Each of these states could be managed by a single React component which is mainly doing three things:

1. storing the state
2. enabling the user to modify the state
3. updating the UI once the state has been changed
This can be done within a React component with React Hooks. 

### ReactJS useState Hook

The `useState` hook is a feature in React that allows functional components to manage state. Introduced in React version 16.8, the `useState` hook provides a way for functional components to have local state variables, enabling them to handle and update their state over the course of the component's lifecycle. 

It is particularly useful for managing simple to moderately complex state logic without the need for class components. The `useState` hook takes an initial state value as an argument and returns an array with two elements: the current state value and a function that can be used to update the state. This function triggers a re-render of the component with the new state value, ensuring that the UI reflects the most recent changes. 

### useState Hook First Example

In below example, the `useState` hook takes an initial state as argument, just for the first time the React component renders, and returns an array with two values: the current state and the state update function. Whereas the current state is used to display it somewhere within your React component, the state update function is used to change the current state (e.g. HTML button onClick).

```jsx
import React from 'react';

const App = () => {
  const [counter, setCounter] = React.useState(42);

  const handleClick = () => {
    setCounter(counter + 5);
  };

  return (
    <>
      <p>{counter}</p>
      <button type="button" onClick={handleClick}>
        Increase by 5
      </button>
    </>
  );
};
```

### usestate Hook Second Example

Taking it one step further, it cannot be just used to increase an integer, but also to capture more dynamic state of an input HTML element when typing into it. 

```jsx
import React from 'react';

const App = () => {
  const [text, setText] = React.useState('Hello React');

  const handleChange = event => {
    setText(event.target.value);
  };

  return (
    <>
      <p>{text}</p>
      <input type="text" value={text} onChange={handleChange} />
    </>
  );
};
```

