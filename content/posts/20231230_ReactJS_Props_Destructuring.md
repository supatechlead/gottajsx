---
title: "ReactJS Props Destructuring"
date: 2023-12-30T07:41:05+01:00
type: "post"
draft: false 
description: "How to Destructure Props"
showTableOfContents: true
tags: ["ReactJS", "Props", "Destructuring", "Spread Operator", "Rest Operator"]
---
But destructuring is really a simple concept that can make code more readable and clear, especially when passing down props in React.

## Destructuring

### Destructuring in Vanilla Javascript

Destructuring was introduced in ES6. It’s a JavaScript feature that allows us to extract multiple pieces of data from an array or object and assign them to their own variables.

Imagine you have a `person` object with the following properties:
```javascript
const person = {
  firstName: "Lindsay",
  lastName: "Criswell",
  city: "NYC"
}
```

Before ES6, you had to access each property individually:
```javascript
console.log(person.firstName) // Lindsay
console.log(person.lastName) // Criswell
console.log(person.city) // NYC
```

Destructuring lets us streamline this code:
```javascript
const { firstName, lastName, city } = person;
```
is equivalent to
```javascript
const firstName = person.firstName
const lastName = person.lastName
const city = person.city
```
So now we can access these properties without the person. prefix:
```javascript
console.log(firstName) // Lindsay
console.log(lastName) // Criswell
console.log(city) // NYC
```
### Destructuring when Passing Props

When you define a functional component, you often receive props as an argument. Destructuring props allows you to extract specific properties directly in the function signature. 

Here's an example, we write a small app with a main component calling components with and without destructuring:
```jsx
const App = () => {
  return (
    <div>
      <h1>Components without destructuring</h1>
      <ExampleComponentWithoutDestructuring name="John" age={25} />

      <h1>Components with destructuring</h1>
      <ExampleComponentWithDestructuring name="Jane" age={30} />
    </div>
  );
};

```


The component that does not use destructuring will be:
```jsx
const ExampleComponentWithoutDestructuring = (props) => {
  const name = props.name;
  const age = props.age;

  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
};
```

The component that uses destructuring will be:
```jsx
const ExampleComponentWithDestructuring = ({ name, age }) => {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
    </div>
  );
};
```

## Spread Operator

### Spread Operator in Vanilla Javascript

The spread operator (`...`) is a syntactic feature in JavaScript that allows an iterable, such as an array or an object, to be expanded or spread into individual elements. It is used in various contexts for copying, combining, or spreading the elements of an iterable.

Here are some common use cases for the spread operator:
1. Array Spreading:
```javascript
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];
const combinedArray = [...array1, ...array2];
console.log(combinedArray); // [1, 2, 3, 4, 5, 6]
```
2. Object Spreading:
```javascript
const object1 = { a: 1, b: 2 };
const object2 = { c: 3, d: 4 };
const combinedObject = { ...object1, ...object2 };
console.log(combinedObject); // { a: 1, b: 2, c: 3, d: 4 }
```
3. Copying Arrays:
```javascript
const originalArray = [1, 2, 3];
const copiedArray = [...originalArray];
console.log(copiedArray); // [1, 2, 3]
```

4. Copying Objects:
```javascript
const originalObject = { x: 1, y: 2 };
const copiedObject = { ...originalObject };
console.log(copiedObject); // { x: 1, y: 2 }
```

5. Function Arguments:
```javascript
function exampleFunction(arg1, arg2, arg3) {
  console.log(arg1, arg2, arg3);
}
const args = [1, 2, 3];
exampleFunction(...args); // Equivalent to exampleFunction(1, 2, 3)
```


### Spread Operator whan passing Props

In the context of React, the spread operator is often used to pass props to a component or to merge objects and arrays efficiently. It's a convenient way to handle multiple values and keep the code concise.
```jsx
const props = { prop1: 'value1', prop2: 'value2' };

<MyComponent {...props} />
```
As example:
```jsx {hl_lines=[11,16]}
import * as React from 'react';

const App = () => {
  const greeting = {
    title: 'React',
    description: 'Your component library for ...',
  };

  return (
    <div>
      <Welcome {...greeting} />
    </div>
  );
};

const Welcome = ({ title, description }) => {
  return (
    <div>
      <Headline title={`Welcome to ${title}`} />
      <Description paragraph={description} />
    </div>
  );
};

const Headline = ({ title }) => <h1>{title}</h1>;
const Description = ({ paragraph }) => <p>{paragraph}</p>;

export default App;
```

## Rest Operator

### Rest Operator in Vanilla Javascript

The `rest` operator can be considered a form of destructuring, specifically when used in the context of destructuring assignment. The `rest` operator allows you to collect the remaining elements of an array or properties of an object into a single variable. This can be particularly useful in scenarios where you want to extract some elements and gather the rest.

Here's an example of the rest operator in destructuring assignment with an array:

```javascript
const numbers = [1, 2, 3, 4, 5];

// Using destructuring with rest operator
const [first, second, ...rest] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

Similarly, you can use the rest operator in object destructuring:
```javascript
const person = {
  firstName: 'John',
  lastName: 'Doe',
  age: 30,
  city: 'New York'
};

// Using destructuring with rest operator
const { firstName, lastName, ...otherInfo } = person;

console.log(firstName);  // 'John'
console.log(lastName);   // 'Doe'
console.log(otherInfo);  // { age: 30, city: 'New York' }
```

### Rest Operator when Passing Props

The `rest` operator can be useful in React when you want to capture some specific props and pass the rest of them down to a child component. Here's an example:

```jsx
import React from 'react';

// Parent component
const ParentComponent = ({ prop1, prop2, prop3, ...restProps }) => {
  // prop1, prop2, and prop3 are captured individually
  // restProps will contain the rest of the props

  return (
    <div>
      <h1>Parent Component</h1>
      <p>prop1: {prop1}</p>
      <p>prop2: {prop2}</p>
      <p>prop3: {prop3}</p>

      {/* Passing the rest of the props down to ChildComponent */}
      <ChildComponent {...restProps} />
    </div>
  );
};

// Child component
const ChildComponent = ({ prop4, prop5 }) => {
  // prop4 and prop5 are captured from the rest of the props

  return (
    <div>
      <h2>Child Component</h2>
      <p>prop4: {prop4}</p>
      <p>prop5: {prop5}</p>
    </div>
  );
};

// Usage of ParentComponent in the app
const App = () => {
  return (
    <div>
      <ParentComponent
        prop1="Value 1"
        prop2="Value 2"
        prop3="Value 3"
        prop4="Value 4"
        prop5="Value 5"
        prop6="Value 6"
      />
    </div>
  );
};

export default App;
```
In this example:

* `ParentComponent` receives specific `props` (`prop1`, `prop2`, `prop3`) and uses the `rest` operator (`...restProps`) to collect the rest of the props.
* The `rest` of the props are then passed down to the `ChildComponent` using the spread operator `({...restProps})`.
