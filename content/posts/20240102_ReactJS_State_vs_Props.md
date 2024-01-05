---
title: "ReactJS Props vs State"
date: 2024-01-02T07:41:05+01:00
type: "post"
draft: false 
description: "Description of Differences Between ReactJS State and Props"
showTableOfContents: true
tags: ["ReactJS", "State", "Props", "Pass Props to Parent"]
---

In React, both state and props are mechanisms for managing data in a component, but they serve different purposes and have some key differences:

## Differences Between Props and States

### Definition:

**State:** State is an internal data storage mechanism used by a component to manage its own data. It is declared and initialized within the component, typically in the constructor for class components or using the useState hook in functional components.\
**Props (Properties):** Props are external inputs to a component that are passed to it by its parent component. Props are immutable and are used to pass data from a parent component to a child component.

### Mutability:

**State:** State is mutable and can be changed by the component that owns it. The component can use the setState method (for class components) or the state update function (for functional components using the useState hook) to modify its own state.\
**Props:** Props are immutable, meaning they cannot be modified by the component that receives them. They are passed down from parent to child and should not be changed within the receiving component.

### Ownership:

**State:** Each component manages its own state. It is internal to the component and is not directly accessible or modifiable by other components.\
**Props:** Props are passed from a parent component to a child component. The parent component owns and controls the props it passes, while the child component receives and uses them.

### Initialization:

**State:** State is initialized within the component, often in the constructor for class components or using the useState hook in functional components.\
**Props:** Props are initialized in the parent component and passed down to child components as attributes.

### Purpose:

**State:** Used for managing and storing dynamic data that can change over time within a component. State triggers re-renders when updated.\
**Props:** Used for passing data from a parent component to a child component. Props provide a way for components to communicate and share information.

## Passing State as Props

While props are just a vehicle to pass information down the component tree, state can be changed over time to create interactive user interfaces. But the below example shows how state can become props when it is passed to a child component. Even though the state becomes props in the child component, it can still get modified in the parent component as state via the state updater function. Once modified, the state is passed down as "modified" props:

```jsx {hl_lines=[4,"11-13", 21]}
import * as React from 'react';

const App = () => {
  const [greeting, setGreeting] = React.useState('Welcome to React');
  const [isShow, setShow] = React.useState(true);

  const handleToggle = () => {
    setShow(!isShow);
  };

  const handleChange = (event) => {
    setGreeting(event.target.value);
  };

  return (
    <div>
      <button onClick={handleToggle} type="button">
        Toggle
      </button>

      <input type="text" value={greeting} onChange={handleChange} />

      {isShow ? <Welcome text={greeting} /> : null}
    </div>
  );
};

const Welcome = ({ text }) => {
  return <h1>{text}</h1>;
};

export default App;
```

## Passing props to Parent Component

In React, data generally flows from parent to child components through props. However, if you need to pass data from a child component to its parent, you can achieve this by passing a function from the parent to the child as a prop. The child component can then call this function with the data as an argument.

In the following example, `ParentComponent` has a state variable `dataFromChild` and a function `handleDataFromChild` that updates the state when called. This function is then passed down to `ChildComponent` as a prop named `onDataFromChild`. 
```jsx
import React, { useState } from 'react';
import ChildComponent from './ChildComponent';

const ParentComponent = () => {
  const [dataFromChild, setDataFromChild] = useState(null);

  const handleDataFromChild = (data) => {
    setDataFromChild(data);
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <p>Data from Child: {dataFromChild}</p>
      <ChildComponent onDataFromChild={handleDataFromChild} />
    </div>
  );
};

export default ParentComponent;
```
 In `ChildComponent`, when the button is clicked, the `sendDataToParent` function is called, which in turn calls the function received from the parent with the data you want to pass.
```jsx
import React, { useState } from 'react';

const ChildComponent = (props) => {
  const [dataToSend, setDataToSend] = useState('Data from Child');

  const sendDataToParent = () => {
    // Call the function passed from the parent with the data
    props.onDataFromChild(dataToSend);
  };

  return (
    <div>
      <h2>Child Component</h2>
      <button onClick={sendDataToParent}>Send Data to Parent</button>
    </div>
  );
};

export default ChildComponent;
```
