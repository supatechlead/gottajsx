---
title: "Zustand State Management"
date: 2024-01-05T07:41:05+01:00
type: "post"
draft: false 
description: "A simple and Modern State Management for React"
showTableOfContents: true
tags: ["ReactJS", "State", "Zustand"]
---

Zustand is a fast and scalable state management. Zustand is known for its simplicity, using hooks to manage states without boilerplate code.

## Setting up an app

The first step is to create a new React application and install the Zustand dependency. To do this, run the following commands:
```npx
npx create-react-app zustand
cd zustand 
npm install zustand
```
## Creating the Store

Now, we will need to define a store that will contain all states and their functions used by the application. We will do this in our `store.js` file:
> store.js
```jsx
import create from 'zustand';

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

export default useStore;
```
The above `create` function takes a callback function as its argument. This callback function receives a `set` function as its argument, which is used to update the state.

Inside the callback function, an object is returned. This object represents the initial state of the store and contains methods to perform state updates. In this case:

* `count` is the initial state variable set to 0.
* `increment` is a method that, when called, uses the set function to increment the count by 1.
* `decrement` is a method that, when called, uses the set function to decrement the count by 1.

## Accessing the Store


To access the store you've created using Zustand, you can use the `useStore` hook within your React components. Here's an example of how you can access and use the store:

Assuming you have a component file, let's call it `MyComponent.js`:
> MyComponent.js
```jsx
import React from 'react';
import useStore from './store'; // Assuming 'store' is the file where you created the Zustand store

const MyComponent = () => {
  // Using the useStore hook to access the state and actions
  const { count, increment, decrement } = useStore();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

export default MyComponent;
```

In this example, `useStore()` returns an object containing the current state (`count`) and the actions (`increment` and `decrement`). You can then use these values and functions within your component as needed.

Make sure that you've set up your project structure correctly and that the `store.js` file (or whatever you named it) is in the correct location and properly exports the `useStore` hook.

When you use `MyComponent` in your application, it will automatically manage its state using the Zustand store. Any changes to the state triggered by the `increment` or `decrement` functions will cause a re-render of the component, updating the displayed count.

## Working with Asynchronous Data

When dealing with asynchronous data fetching in Zustand, you might want to use asynchronous actions to fetch data and update the state. Here's an example of how you can use Zustand to handle asynchronous data:

Assume you have a store file named `store.js`:
> store.js
```jsx
import create from 'zustand';

const useStore = create((set) => ({
  data: null,
  isLoading: false,

  fetchData: async () => {
    set({ isLoading: true });

    try {
      // Simulating an asynchronous API call (replace with your actual API call)
      const response = await fetch('https://api.example.com/data');
      const data = await response.json();

      set({ data, isLoading: false });
    } catch (error) {
      console.error('Error fetching data:', error);
      set({ isLoading: false });
    }
  },
}));

export default useStore;
```
In this example:
* `data` is the state variable that will hold the fetched data.
* `isLoading` is a state variable to track whether data is currently being loaded.
* `fetchData` is an asynchronous action that fetches data from an API.

Now, you can use this store in a component to fetch and display the data. Here's an example component:
> Component.js
```jsx
import React, { useEffect } from 'react';
import useStore from './store';

const MyComponent = () => {
  const { data, isLoading, fetchData } = useStore();

  useEffect(() => {
    fetchData(); // Trigger the data fetch when the component mounts
  }, []); // Empty dependency array ensures the effect runs only once

  return (
    <div>
      {isLoading ? (
        <p>Loading...</p>
      ) : (
        <>
          {data ? (
            <div>
              <h2>Data:</h2>
              <pre>{JSON.stringify(data, null, 2)}</pre>
            </div>
          ) : (
            <p>No data available.</p>
          )}
        </>
      )}
    </div>
  );
};

export default MyComponent;
```
In this component:

* The `useEffect` hook is used to trigger the `fetchData` action when the component mounts.
* The component conditionally renders content based on the `isLoading` and `data` states.
When `fetchData` is called, it sets `isLoading` to `true`, performs the asynchronous data fetch, and updates the state with the fetched data or an error message. The component responds to these state changes and renders accordingly.