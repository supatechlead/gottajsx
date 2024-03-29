--- 
draft : false
date : 2024-02-09T14:37:18+01:00
title : "Mistakes to be Avoided Using useState"
description : "Four Critical Mistakes to Avoid Using usestate"
tags : ["React", "Hook"]
showTableOfContents : true
type: "post"
---

In this article, we are going to learn about the `useState()` hook.

## Mistake 1: Forgetting to Consider the Previous State

When working with React’s `useState` hook, a common mistake is not taking into account the most recent state when updating it. This oversight can lead to unexpected behaviors, particularly when you're dealing with rapid or multiple state updates.

### Understanding the Issue

Let’s imagine you’re building a counter in React. Your goal is to increase the count each time a button is clicked. A straightforward approach might be to simply add 1 to the current state value. However, this can be problematic.
```jsx
import React, { useState } from 'react';

const CounterComponent = () => {
  const [counter, setCounter] = useState(0);

  const incrementCounter = () => {
    setCounter(counter + 1); // Might not always work as expected
  };

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default CounterComponent;
```

In the above code, `incrementCounter` updates the counter based on its current value. This seems straightforward but can lead to issues. React might batch multiple `setCounter` calls together, or other state updates might interfere, resulting in the counter not being updated correctly every time.

### The Correction:

To avoid this issue, use the functional form of the `setCounter` method. This version takes a function as its argument, which React calls with the most recent state value. This ensures that you're always working with the latest value of the state.
```jsx
import React, { useState } from 'react';

const CounterComponent = () => {
  const [counter, setCounter] = useState(0);

  const incrementCounter = () => {
    setCounter(prevCounter => prevCounter + 1); // Correctly updates based on the most recent state
  };

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default CounterComponent;
```

In this corrected code, `incrementCounter` uses a function to update the state. This function receives the most recent state (`prevCounter`) and returns the updated state. This approach is much more reliable, especially when updates happen rapidly or multiple times in a row.

## Mistake 2: Neglecting State Immutability 

### Understanding the Issue

In React, state should be treated as immutable. A common mistake is directly mutating the state, especially with complex data structures like objects and arrays.

Consider this faulty approach with a stateful object:
```jsx
import React, { useState } from 'react';

const ProfileComponent = () => {
  const [profile, setProfile] = useState({ name: 'John', age: 30 });

  const updateAge = () => {
    profile.age = 31; // Directly mutating the state
    setProfile(profile);
  };

  return (
    <div>
      <p>Name: {profile.name}</p>
      <p>Age: {profile.age}</p>
      <button onClick={updateAge}>Update Age</button>
    </div>
  );
};

export default ProfileComponent;
```
This code incorrectly mutates the profile object directly. Such mutations don't trigger re-renders and lead to unpredictable behaviors.

### The Correction:

Always create a new object or array when updating state to maintain immutability. Use the spread operator for this purpose.
```jsx
import React, { useState } from 'react';

const ProfileComponent = () => {
  const [profile, setProfile] = useState({ name: 'John', age: 30 });

  const updateAge = () => {
    setProfile({...profile, age: 31}); // Correctly updating the state
  };

  return (
    <div>
      <p>Name: {profile.name}</p>
      <p>Age: {profile.age}</p>
      <button onClick={updateAge}>Update Age</button>
    </div>
  );
};

export default ProfileComponent;
```

In the corrected code, `updateAge` uses the spread operator to create a new `profile` object with the updated `age`, preserving state immutability.

## Mistake 3: Misunderstanding Asynchronous Updates

### Understanding the Issue

React’s state updates via `useState` are asynchronous. This often leads to confusion, especially when multiple state updates are made in quick succession. Developers might expect the state to change immediately after a `setState` call, but in reality, React batches these updates for performance reasons.

Let’s look at a common scenario where this misunderstanding can cause problems
```jsx
import React, { useState } from 'react';

const AsyncCounterComponent = () => {
  const [count, setCount] = useState(0);

  const incrementCount = () => {
    setCount(count + 1);
    setCount(count + 1);
    // Developer expects count to be incremented twice
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementCount}>Increment Count</button>
    </div>
  );
};

export default AsyncCounterComponent;
```

In this example, the developer intends to increment the count twice. However, due to the asynchronous nature of state updates, both `setCount` calls are based on the same initial state, resulting in the count being incremented only once.

### The Correction:

To handle asynchronous updates correctly, use the functional update form of `setCount`. This ensures that each update is based on the most recent state.
```jsx
import React, { useState } from 'react';

const AsyncCounterComponent = () => {
  const [count, setCount] = useState(0);

  const incrementCount = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);
    // Now each update correctly depends on the most recent state
  };
  // Optional: Use useEffect to see the updated state
  useEffect(() => {
    console.log(count); // 2
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementCount}>Increment Count</button>
    </div>
  );
};

export default AsyncCounterComponent;
```

In the above code, each call to `setCount` uses the most recent value of the state, ensuring accurate and sequential updates. This approach is crucial for operations that depend on the current state, especially when multiple state updates occur in quick succession.

## Mistake 4: Misusing State for Derived Data 📊

### Understanding the Issue

A frequent error is using state for data that can be derived from existing state or props. This redundant state can lead to complex and error-prone code.

For example:
```jsx
import React, { useState } from 'react';

const GreetingComponent = ({ name }) => {
  const [greeting, setGreeting] = useState(`Hello, ${name}`);

  return (
    <div>{greeting}</div>
  );
};

export default GreetingComponent;
```
Here, greeting state is unnecessary as it can be derived directly from name.

### The Correction:

Instead of using state, derive data directly from existing state or props.

```jsx
import React from 'react';

const GreetingComponent = ({ name }) => {
  const greeting = `Hello, ${name}`; // Directly derived from props

  return (
    <div>{greeting}</div>
  );
};

export default GreetingComponent;
```
In the corrected code, `greeting` is computed directly from the `name` prop, simplifying the component and avoiding unnecessary state management.