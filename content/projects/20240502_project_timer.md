--- 
draft : false
date : 2024-01-08T14:37:18+01:00
title : "Timer Project"
description : "A Simple Timer Component With Button Outside"
showTableOfContents : true
type: "post"
---

## Goal

The goal of this project is to create a component that can start and stop a timer in another component 

## Layout

For layout, we want the screen be split in upper and lower part.

> App.js
```jsx
function App() {
  return (
    <div style={{ margin: 0, height: "100vh", display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center" }}>
      <div style={{ flex: 1, borderBottom: "5px solid #ccc", padding: "10px" }}>
        <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
          UPPER PART
        </div>
      </div>

      <div style={{ flex: 1, padding: "10px" }}>
        <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
          LOWER PART
        </div>
      </div>
    </div>
  );
}

export default App;
```

## Timer Component

To create a timer that increments by 1 every second in a React component, you can use the `useState` hook to manage the state of the timer value and the `useEffect` hook to update the timer at a regular interval. Here's how:

> Timer.js
```jsx
import React, { useState, useEffect } from 'react';

const Timer = () => {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    // Function to increment the timer every second
    const intervalId = setInterval(() => {
      setSeconds((prevSeconds) => prevSeconds + 1);
    }, 1000);

    // Cleanup function to clear the interval when the component unmounts
    return () => clearInterval(intervalId);
  }, []); // Empty dependency array ensures the effect runs only once (on mount)

  return (
    <div>
      <h1>Timer: {seconds} seconds</h1>
    </div>
  );
};

export default Timer;
```
In upper code:
* The `useState` hook is used to create a `seconds` state variable with an initial value of 0.
* The `useEffect` hook is used to set up an interval that increments the timer every second using `setSeconds`.
* The `clearInterval` function is used in the cleanup function of `useEffect` to clear the interval when the component is unmounted.

## RunningToggle Component

A component owning a single button must be create to switch timer from running or not running state:
> runningToggle.js
```jsx
import { useState } from 'react';


function RunningToggle() {
  const [isRunning, setIsRunning] = useState(true)

  const handleToggle = () => {
      setIsRunning((prevIsRunning)=>!prevIsRunning)
  }
  return (
        <button onClick={handleToggle}>{isRunning ? 'Stop' : 'Start'}</button>
  );
}

export default RunningToggle;
```

## App.js Update

Now that we own both `RunningToggle` and `Timer` modules, we can insert it inside `App.js`
> App.js
```jsx
function App() {
  return (
    <div style={{ margin: 0, height: "100vh", display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center" }}>
      <div style={{ flex: 1, borderBottom: "5px solid #ccc", padding: "10px" }}>
        <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
          <RunningToggle />
        </div>
      </div>

      <div style={{ flex: 1, padding: "10px" }}>
        <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
          <Timer />
        </div>
      </div>
    </div>
  );
}

export default App;
```

## Using Context

### Using Context Inside App.js

we must create a `RunningContext` context inside `App.js` and export it:
```javascript
export const RunningContext = createContext({
  isRunning: false,
  setIsRunning: () => {},
})
```

Then, we define `isRunning` and `setIsRunning` inside `App` function in order to use them as context provider values:
```jsx
function App() {

  const [isRunning, setIsRunning] = useState(false);

  return (
    <RunningContext.Provider value={{isRunning, setIsRunning}}>
      <div style={{ margin: 0, height: "100vh", display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center" }}>
        <div style={{ flex: 1, borderBottom: "5px solid #ccc", padding: "10px" }}>
          <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
            <RunningToggle />
          </div>
        </div>

        <div style={{ flex: 1, padding: "10px" }}>
          <div style={{ display: 'flex', alignItems: 'center', justifyContent: 'center', height: '100%' }}>
            <Timer />
          </div>
        </div>
      </div>
    </RunningContext.Provider>
  );
}
```
### Using Context Inside Timer Component

We can modify `Timer` component for using the previously defined `RunningContext` context as:
```jsx`
import React, { useState, useEffect, useContext } from 'react';
import { RunningContext } from './App';

const Timer = () => {
    const { isRunning } = useContext(RunningContext);
    const [seconds, setSeconds] = useState(0);

    useEffect(() => {
        let intervalId

        if(isRunning) {
            intervalId = setInterval(() => {
                setSeconds((prevSeconds) => prevSeconds + 1);
            }, 1000);
        }

        return () => clearInterval(intervalId);
    }, [isRunning]);

    return (
        <h1>Timer: {seconds} seconds</h1>
    );
};

export default Timer;
```

### Using Context Inside RunningToggleComponent

The same way, can can modify `RunningToggle` component
```jsx
import { useContext } from 'react';
import { RunningContext } from './App';

const RunningToggle = () => {
  const { isRunning, setIsRunning } = useContext(RunningContext)

  const handleToggle = () => {
      setIsRunning((prevIsRunning)=>!prevIsRunning)
  }
  return (
        <button onClick={handleToggle}>{isRunning ? 'Stop' : 'Start'}</button>
  );
}

export default RunningToggle;
```

## Alternative Writing

As alternative way, of creating a context, we could create a provider component that will wrap the application and create a custom hook to consume the context:
```jsx
import React, { createContext, useState, useContext } from 'react';

// Create a context with an initial value for isRunning and setIsRunning
const RunningContext = createContext({
  isRunning: true,
  setIsRunning: () => {},
});

// Create a provider component that will wrap your application
export const RunningProvider = ({ children }) => {
  const [isRunning, setIsRunning] = useState(true);

  return (
    <RunningContext.Provider value={{ isRunning, setIsRunning }}>
      {children}
    </RunningContext.Provider>
  );
};

// Create a custom hook to consume the context
export const useRunning = () => {
  const context = useContext(RunningContext);

  if (!context) {
    throw new Error('useRunning must be used within a RunningProvider');
  }

  return context;
};
```
Now we can use the `RunningProvider` component at the top level of the application and use the `useRunning` hook in any component that needs access to the `isRunning` state and `setIsRunning` function.

Example usage:

```jsx
import React from 'react';
import { RunningProvider, useRunning } from './RunningContext';

const App = () => {
  return (
    <RunningProvider>
      <ChildComponent />
    </RunningProvider>
  );
};

const ChildComponent = () => {
  const { isRunning, setIsRunning } = useRunning();

  // Use isRunning and setIsRunning in your component
  // ...

  return (
    <div>
      <p>isRunning: {isRunning.toString()}</p>
      <button onClick={() => setIsRunning(!isRunning)}>
        Toggle isRunning
      </button>
    </div>
  );
};

export default App;
```

