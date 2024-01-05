---
title: "Introducing JSX"
date: 2023-12-28T17:41:05+01:00
type: "post"
draft: false 
description: "This article will introduce what JSX is"
showTableOfContents: true
tags: ["ReactJS", "JSX"]
---
JSX stands for JavaScript syntax extension. It is a JavaScript extension that allows us to describe React’s object tree using a syntax that resembles that of an HTML template. It is just an XML-like extension that allows us to write JavaScript that looks like markup and have it returned from a component.

## JSX Code Example

Here is an example of a basic JSX code:
```jsx
const App = () => {
   return (
     <div>
       <p>Header</p>
       <p>Content</p>
       <p>Footer</p>
     </div>
   ); 
}
```
## JSX Rules

There are few rules to keep it mind while working with JSX:
1. A React component name must be capitalized. Component names that do not begin with a capital letter are treated like built-in components.
2. JSX allows you to return only one element from a given component. This is known as a parent element.
If you want to return multiple HTML elements, simply wrap all of them in a single `<div></div>`, `<React.fragments><React.fragments/>`, `<></>` or any semantic tag, then  encapsulate everything inside `( )`.
```jsx
const App = () => {
  return (
    <div>
      <h1>Hello World!</h1>
      <p>Tanishka here!</p>
    </div>
  );
}
```
3. In JSX, every tag, including self closing tags, must be closed. In case of self closing tags you have to add a slash at the end (for example `<img/>`, `<hr/>`, and so on).
```jsx
const App = () => {
  return (
    <>
      <img src="./assets/react.svg" alt="" />
    </>
  );
}
```
4. Since JSX is closer to JavaScript than to HTML, the React DOM uses the camelCase naming convention for HTML attribute names. For example: `tabIndex`, `onChange`, and so on.

5. `"class"` and `"for"` are reserved keywords in JavaScript, so use `"className"` and `"forHTML"` instead, respectively.

## How to use CSS in JSX

To add style to our components, we can either use inline CSS or external CSS.

### Inline CSS

JSX represents objects, that is key-value pairs – like a property name and its value. Always write the value in `" "` as we do in objects.

```jsx
const App = () => {
  return (
    <>
      <h1 style={ { color: "Red" } }>Hello World!</h1>
      <p style={ { fontSize: "20px" } }>Tanishka here!</p>
    </>
  );
}
```
* The value of the style attribute is wrapped in a set of curly braces `{}`. This is how you pass a Javascript expression in JSX.
* There is a second set of curly braces inside, indicating the object containing the CSS properties and values.
* The CSS property `font-size` is typed as fontSize. Hyphens don't play nice with JSX, so any CSS property with a hyphen must be converted to camelCase to work.
* The property values are wrapped in quotes. While this isn't necessarily required in a CSS stylesheet, we do need to pass the values as strings in most cases. If you are passing a numeric value, such as for width or margin, it will default to pixels, so if you DON'T want that, you'll need to pass a string like `"40%"`.

### External Stylesheet

You can create a new CSS file in your project directory and add your CSS inside it. It can then be imported into your React component or class. The following code is used to import an external CSS stylesheet.

```jsx
import "./styles.css";
```

## JavaScript in JSX

To add JavaScript code inside JSX, we need to write it in curly brackets like this:
```jsx
const App = () => {
 const number = 10;
 return (
  <div>
   <p>Number: {number}</p>
  </div>
 );
};
```
## Conditional Operators in JSX Expressions

React allows us to write conditional operators, like ternary operators as well as the logical short circuit `&&` operator like this:
```jsx
<p>{a > b ? "Greater" : "Smaller"}</p>
<p>{shouldShow && "Shown"}</p>
```
Below example uses the exact same logic:
```jsx
  return (
    <div>
      {condition ? (
        <p>The condition is true!</p>
      ) : (
        <p>The condition is false.</p>
      )}
    </div>
  );
```