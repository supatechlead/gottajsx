--- 
draft : false
date : 2024-02-04T14:37:18+01:00
title : "Using React useRef Hook "
description : "Understanding the React useRef Hook"
tags : ["React", "useRef"]
showTableOfContents : true
type: "post"
---

In this article, we will delve into the depths of the useRef hook, demystifying its purpose, functionality, and best practices.

## UseRef Hook Usage

The `useRef` hook serves two main purposes: 
* storing mutable values that do not trigger a new render when updated 
* storing references to DOM elements. Let's take a closer look at how it works.

When a component is rendered in React, its state and other variables are generally reset. However, there are cases where you need to retain certain values even when the component is rendered again. This is where the `useRef` hook comes into play. It allows you to create a reference to a value that will persist between renders, ensuring that the value remains unchanged even if other parts of the component change.

Furthermore, the `useRef` hook is essential for working with DOM elements. In React, direct access and modification of DOM elements can be challenging, especially without the `useRef` hook.

## Implementation of useRef in React

To begin using the `useRef` hook in your React project, import it from the React package:
```javascript
import { useRef } from 'react';
```
Once imported, you can declare a `ref` variable in your functional component using the `useRef` hook:

```jsx
const myRef = useRef();
```
Now you have a `ref` object, `myRef`, that you can use to store and access values. To use the `myRef` variable with any element, assign it to the `ref` property of the element:

```jsx
<div ref={myRef}>This is an example element</div>
```

In the example above, you assign the `ref` property to the `div` element. This allows you to reference and access the element using the `myRef` variable elsewhere in the component.

To access the value stored in the created reference, you can use the `.current` property of the `myRef` object:

```jsx
const myRefValue = myRef.current;
console.log(myRefValue); // <div>This is an example element</div>
```


##DOM Manipulation with the useRef Hook

DOM manipulation is a common task in web development, as it allows you to dynamically modify and update the content, structure, and appearance of a web page.

In traditional JavaScript development, accessing DOM elements and manipulating them requires the use of methods such as `getElementById`, `querySelector`, or `getElementsByClassName` to select specific elements in the document. Once the element is selected, you can update the content, modify styles, or attach event listeners.

```jsx
// HTML
<div>
  <input type="text" id="myInput" />
  <button id="focusButton">Focus Input</button>
</div>
// JavaScript
<script>
      const inputRef = document.getElementById('myInput');
      const focusButton = document.getElementById('focusButton');
      const handleFocus = function() {
        inputRef.focus();
      };
      focusButton.addEventListener('click', handleFocus);
</script>
```

However, when working with DOM elements in a React component, the process is not the same due to the component's virtual DOM and the need to manage updates efficiently.

With the introduction of the `useRef` hook in React, the process of working with DOM elements within components has been significantly streamlined. The `useRef` hook provides a straightforward way to create a reference to a DOM element, making it easily accessible and manipulable within the component context.

```jsx
import { useRef } from 'react';

const FocusComponent = () => {
  const inputRef = useRef(null);

  const handleFocus = () => {
    // accessing the input element
    let inputElement = inputRef.current;

   // modify the DOM element
   inputElement.focus();
  };
 
 return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
}
```

Dans cet exemple, le hook `useRef` est utilisé pour créer une référence `inputRef` qui pointe vers l’élément `input`. Lorsque vous cliquez sur le bouton « Focus Input », la fonction `handleFocus` utilise `inputRef.current.focus()` pour mettre l’accent sur l’élément d’entrée directement. Ceci démontre comment le hook `useRef` simplifie le processus de travail avec les éléments DOM dans React.

Autre exemple : vous souhaitez manipuler une page `div en` changeant son arrière-plan lorsqu’un bouton est cliqué :

```jsx
import { useRef } from 'react';

const ExampleComponent = () => {
  const divRef = useRef();

  const handleClick = () => {
    divRef.current.style.backgroundColor = 'red';
  };

  return (
    <div>
      <div ref={divRef}>This is a sample div</div>
      <button onClick={handleClick}>Change Color</button>
    </div>
  );
}
```

Dans cet exemple, vous créez une référence avec le hook `useRef` appelée `divRef`. Vous assignez cette référence à la propriété `ref` de l’élément `div`.

Lorsque vous cliquez sur le bouton « Change Color », la fonction `handleClick` est invoquée. Dans la fonction, vous pouvez accéder à l’élément `div` avec `divRef.current`. Dans ce cas, vous modifiez la couleur d’arrière-plan de l’élément `div` en mettant à jour sa propriété `style.backgroundColor` avec la valeur `red`.

## Preserving values across re-renders

Preserving values across re-renders is a powerful use case for the `useRef` hook. It is especially useful when you have values that need to persist throughout the component's lifecycle without triggering a new render.

To better understand this concept, let's compare the `useRef` hook with the `useState` hook using real examples:

Example with the `useState` hook:
```jsx
import { useState } from 'react';

function CounterComponent() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```
n this example, you use the `useState` hook to manage the state variable `count`. Whenever the `increment` function is called, the `state` count is updated using `setCount`. This triggers a new rendering of the component, reflecting the updated value of `count`

Example with the useRef hook:
```jsx
import React, { useRef } from 'react';

function CounterComponent() {
  const countRef = useRef(0);

  const increment = () => {
    countRef.current = countRef.current + 1;
    console.log('Count:', countRef.current);
  };

  return (
    <div>
      <p>Count: {countRef.current}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

In this example, you use the `useRef` hook to create a variable `countRef`, initialized with an initial value of 0. Whenever the `increment` function is called, you directly update the value of `countRef.current` without triggering a new render. The updated value is logged to the console.

The page does not re-render with the `useRef` hook.

By using the `useRef` hook, the component is not re-rendered when the value of `countRef.current` changes. This can be advantageous in scenarios where you have values that need to be modified but do not affect the rendering process.

Note that when you use the `useRef` hook in this way, changes to the `ref` value do not automatically trigger new renders. If you need to reflect the updated value in the user interface, you can either manually handle the update or combine the `useRef` hook with other hooks or state variables to achieve the desired behavior.