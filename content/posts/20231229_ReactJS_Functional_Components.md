---
title: "ReactJS Functional Components and Props"
date: 2023-12-29T07:41:05+01:00
type: "post"
draft: false 
description: "Short Introduction to JavaScript Functions being React Components"
showTableOfContents: true
tags: ["ReactJS", "Function Component", "Props", "Import", "Export"]
---
In the past, there have been various React Component Types, but with the introduction of React Hooks it's possible to write your entire application with just functions as React components.

## ReactJS Function Component Example

Here is a simple example of a Functional Component in React defined as App which returns JSX:
```jsx
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <h1>{greeting}</h1>;
}

export default App;
```
It is also possible to render a React Component inside a Function Component by defining another component and render it as HTML element with JSX within the other component's body:

```jsx {hl_lines=[4,"7-11"]}
import React from 'react';

function App() {
  return <Headline />;
}

function Headline() {
  const greeting = 'Hello Function Component!';

  return <h1>{greeting}</h1>;
}

export default App;
```
## ReactJS Import and Export

React enables to treat each React Component as a module itself. Thus, it is possible to import/export React Components and is one of the basic operations to be performed. In React we use the keyword import and from to import a particular module or a named parameter. Let us now see the different ways we can use the import operation in React

Eventually you will separate components into their own files. Since React Components are functions (or classes), you can use the standard import and export statements provided by JavaScript. For instance, you can define and export a component in one file as in example below:
>src/components/Headline.js
```jsx
import React from 'react';

const Headline = (props) => {
  return <h1>{props.value}</h1>;
};

export default Headline;
```
Then we import it in another file:
>src/components/App.js
```jsx
import React from 'react';

import Headline from './Headline.js';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};

export default App;
```

## ReactJS Props

Props are the React Function Component's parameters.
```jsx {hl_lines=[4,6,"9-11"]}
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
}

function Headline(props) {
  return <h1>{props.value}</h1>;
}

export default App;
```
Since props are always coming as object, and most often you need to extract the information from the props anyway, we can directly use Javascript object destructuring in the function signature for the props object:
```jsx {hl_lines=["9-11"]}
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
}

function Headline({ value }) {
  return <h1>{value}</h1>;
}

export default App;
```
> Note: If you forget the JavaScript destructuring and just access props from the component's function signature like `function Headline(value1, value2) { ... }`, you may see a *"props undefined"*-messages. It doesn't work this way, because props are always accessible as first argument of the function and can be destructured from there: `function Headline({ value1, value2 }) { ... }`.

## Props Default Value

In some case, you may want to passe default values as props. In modern Javascript, you can use the default when using destructuring:
```jsx {hl_lines=[1]}
const Welcome = ({ title = 'Earth', description }) => (
  <div>
    <Title title={`Welcome to ${title}`} />
    <Description description={description} />
  </div>
);
```