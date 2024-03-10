--- 
draft : false
date : 2024-03-06T14:37:18+01:00
title : "Multiple Checkbox in React and TypeScript"
tags : ["gui", "react", "nextjs"]
showTableOfContents : true
type: "post"
---

Here's step by step reminder to implement multiple checkbox in React and Typescript

## React Implementation

The following `App` component is a usage example. It renders a `MultiCheckbox` component with three options:

```jsx
Sure, here are the comments in English for the following code:

import React, { useState } from 'react';

function MultiCheckbox({ options }) {
  // State to track the selected options
  const [selectedOptions, setSelectedOptions] = useState([]);

  // Change handler for the checkboxes
  const handleCheckboxChange = (option) => {
    if (selectedOptions.includes(option)) {
      // If the option is already selected, remove it
      setSelectedOptions(selectedOptions.filter((selected) => selected !== option));
    } else {
      // Otherwise, add it to the selected options
      setSelectedOptions([...selectedOptions, option]);
    }
  };

  return (
    <div>
      {/* Dynamically generate the checkboxes */}
      {options.map((option) => (
        <label key={option}>
          <input
            type="checkbox"
            value={option}
            checked={selectedOptions.includes(option)}
            onChange={() => handleCheckboxChange(option)}
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Usage example
function App() {
  const checkboxOptions = ['Option 1', 'Option 2', 'Option 3'];

  return (
    <div>
      <h2>Choose multiple options:</h2>
      <MultiCheckbox options={checkboxOptions} />
    </div>
  );
}

export default App;
```
* The `MultiCheckbox` component takes an `options` prop, which is an array of strings. The component renders a checkbox for each option.
* The `useState` hook is used to track the state of the checkboxes. The `selectedOptions` state variable stores an array of the currently selected options.
* The `handleCheckboxChange` function is called when the value of a checkbox changes. This function updates the `selectedOptions` state variable to reflect the new value.
* The render function returns the HTML for the component. The options array is iterated over, and a checkbox is rendered for each option. The checked prop of the checkbox is set to `true` if the option is currently selected.


## TypeScript Implementation

Let's implement the previous example with TypeScript

```jsx
import React, { useState } from 'react';

// Interface to define the props expected by the MultiCheckbox component
interface MultiCheckboxProps {
  options: string[]; // An array of strings representing the checkbox labels
}

function MultiCheckbox({ options }: MultiCheckboxProps) {
  // State to manage the selected options
  const [selectedOptions, setSelectedOptions] = useState<string[]>([]);

  // Function to handle checkbox changes
  const handleCheckboxChange = (option: string) => {
    if (selectedOptions.includes(option)) {
      // If the option is already selected, remove it
      setSelectedOptions(selectedOptions.filter((selected) => selected !== option));
    } else {
      // Otherwise, add it to the selected options
      setSelectedOptions([...selectedOptions, option]);
    }
  };

  return (
    <div>
      {/* Dynamically generate the checkboxes */}
      {options.map((option) => (
        <label key={option} className="block text-gray-800 my-2">
          <input
            type="checkbox"
            value={option}
            checked={selectedOptions.includes(option)}
            onChange={() => handleCheckboxChange(option)}
            className="mr-2 focus:outline-none"
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Example of how to use the MultiCheckbox component
function App() {
  const checkboxOptions = ['Option 1', 'Option 2', 'Option 3'];

  return (
    <div>
      <h2>Choose multiple options:</h2>
      <MultiCheckbox options={checkboxOptions} />
    </div>
  );
}

export default App;
```
1. Imports:
* `import React, { useState } from 'react';`
This line imports the necessary libraries: React for building the UI components and `useState` hook for managing component state.

2. `MultiCheckboxProps` interface:
* `interface MultiCheckboxProps { ... }`
  * This defines an interface called `MultiCheckboxProps` that specifies the expected props for the `MultiCheckbox` component.
* `options: string[];`
  * This prop is an array of strings, where each string represents the label for a checkbox option.

3. MultiCheckbox component:

* `function MultiCheckbox({ options }: MultiCheckboxProps) { ... }`
  * This defines the `MultiCheckbox` component that takes a single prop named `options` with the type specified by the `MultiCheckboxProps` interface.

4. State management:
* `const [selectedOptions, setSelectedOptions] = useState<string[]>([]);`
  * This line uses the `useState` hook to create a state variable called `selectedOptions`. It's initialized as an empty array (`[]`) of strings to keep track of the currently selected options. The `setSelectedOptions` function is used to update this state.

5. Checkbox change handler:
* `const handleCheckboxChange = (option: string) => { ... }`
  * This function is called whenever the value of a checkbox changes. It takes the `option` (checkbox label) as an argument.
* `if (selectedOptions.includes(option)) { ... } else { ... }`
  * This conditional statement checks if the clicked checkbox's label (`option`) is already present in the `selectedOptions` array.
* The logic within the `if` and `else` blocks remains the same as before, handling adding or removing the option from the `selectedOptions` state based on its current selection status.

6. Rendering the checkboxes:
* The structure for rendering the checkboxes remains the same as before, using `options.map` to iterate and create checkboxes for each option.
* The code includes additional CSS class names (`block text-gray-800 my-2 mr-2 focus:outline-none`) likely defined elsewhere to style the checkboxes and their labels.

7. App component:
* This component remains unchanged, demonstrating how to use the `MultiCheckbox` component with an array of options.

## TypeScript Implementation with Additional Props

Let's modify the previous code with `selectedOptions` and `onCheckboxChange` as props

```jsx
JavaScript
import React, { useState } from 'react';

// Interface to define the props expected by the MultiCheckbox component
interface MultiCheckboxProps {
  options: string[]; // An array of strings representing the checkbox labels
  selectedOptions: string[]; // An array of strings representing the currently selected options
  onCheckboxChange: (selectedOptions: string[]) => void; // A function to be called when the selected options change
}

function MultiCheckbox({ options, selectedOptions, onCheckboxChange }: MultiCheckboxProps) {
  // Function to handle checkbox changes (local to the component)
  const handleCheckboxChange = (option: string) => {
    const updatedOptions = selectedOptions.includes(option)
      ? selectedOptions.filter((selected) => selected !== option)
      : [...selectedOptions, option];

    // Call the function provided by the onCheckboxChange prop
    onCheckboxChange(updatedOptions);
  };

  return (
    <div>
      {options.map((option) => (
        <label key={option} className="block text-gray-800 my-2">
          <input
            type="checkbox"
            value={option}
            checked={selectedOptions.includes(option)}
            onChange={() => handleCheckboxChange(option)}
            className="mr-2 focus:outline-none"
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Example of how to use the MultiCheckbox component
function App() {
  const checkboxOptions = ['Option 1', 'Option 2', 'Option 3'];
  const [selectedOptions, setSelectedOptions] = useState<string[]>([]);

  // Function to handle checkbox changes in the App component
  const handleCheckboxChange = (updatedOptions: string[]) => {
    // Update the state with the selected options
    setSelectedOptions(updatedOptions);

    // Perform additional actions if needed (e.g., logging)
    console.log('Selected options:', updatedOptions);
  };

  return (
    <div>
      <h2>Choose multiple options:</h2>
      {/* Pass the MultiCheckbox component and the handleCheckboxChange function as props */}
      <MultiCheckbox
        options={checkboxOptions}
        selectedOptions={selectedOptions}
        onCheckboxChange={handleCheckboxChange}
      />
    </div>
  );
}

export default App;
```
1. Imports:
* No changes here (imports React and useState).

2. `MultiCheckboxProps interface`:
* Defines the expected props for the `MultiCheckbox` component.
* `options`: Array of strings for checkbox labels.
* `selectedOptions`: Array of strings for currently selected options (passed from the parent component).
* `onCheckboxChange`: A function to be called by the `MultiCheckbox` component when the selected options change. This function takes the updated `selectedOptions` array as an argument.

3. `MultiCheckbox` component:
* Takes props including `options`, `selectedOptions`, and `onCheckboxChange`.
* `handleCheckboxChange` (local function):
  * This function handles checkbox changes within the `MultiCheckbox` component.
  * It updates the `updatedOptions` based on the clicked checkbox's selection state.
  * It calls the `onCheckboxChange` function provided as a prop, passing the `updatedOptions` array.
* The component renders checkboxes based on the options and sets the checked state for each checkbox using `selectedOptions.includes(option)`.

4. `App` component:
* Defines the `checkboxOptions` array.
* Uses `useState` to manage the `selectedOptions` state in the `App` component.
* `handleCheckboxChange` (function in `App`):
  * This function is called when the `onCheckboxChange` prop function is triggered by the `MultiCheckbox` component.
  * It updates the `selectedOptions` state in the `App` component with the received `updatedOptions`.
  * It can perform additional actions here, such as logging the selected options.
* Renders the `MultiCheckbox` component, passing the `options`, `selectedOptions`, and `handleCheckboxChange` function as props.



## Additional Checkboxes

Let's suppose we have additional checkboxes with different options to choose

```jsx
import React, { useState } from 'react';

// Interface to define the props expected by the MultiCheckbox component
interface MultiCheckboxProps {
  options: string[]; // An array of strings representing the checkbox labels
  selectedOptions: string[]; // An array of strings representing the currently selected options
  onCheckboxChange: (updatedOptions: string[]) => void; // A function to be called when the selected options change
}

function MultiCheckbox({ options, selectedOptions, onCheckboxChange }: MultiCheckboxProps) {
  // Function to handle checkbox changes (local to the component)
  const handleCheckboxChange = (option: string) => {
    const updatedOptions = selectedOptions.includes(option)
      ? selectedOptions.filter((selected) => selected !== option)
      : [...selectedOptions, option];

    // Call the function provided by the onCheckboxChange prop
    onCheckboxChange(updatedOptions);
  };

  return (
    <div>
      {options.map((option) => (
        <label key={option} className="block text-gray-800 my-2">
          <input
            type="checkbox"
            value={option}
            checked={selectedOptions.includes(option)}
            onChange={() => handleCheckboxChange(option)}
            className="mr-2 focus:outline-none"
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Parent function that renders two groups of checkboxes
function MultiCheckboxContainer() {
  const checkboxOptionsGroup1 = ['Option 1', 'Option 2', 'Option 3'];
  const checkboxOptionsGroup2 = ['Choice A', 'Choice B', 'Choice C'];

  // State variables to track selected options for each group
  const [selectedOptionsGroup1, setSelectedOptionsGroup1] = useState<string[]>([]);
  const [selectedOptionsGroup2, setSelectedOptionsGroup2] = useState<string[]>([]);

  // Function to handle checkbox changes in group 1
  const handleCheckboxChangeGroup1 = (updatedOptions: string[]) => {
    setSelectedOptionsGroup1(updatedOptions);
    console.log('Selected options for group 1:', updatedOptions);
  };

  // Function to handle checkbox changes in group 2
  const handleCheckboxChangeGroup2 = (updatedOptions: string[]) => {
    setSelectedOptionsGroup2(updatedOptions);
    console.log('Selected options for group 2:', updatedOptions);
  };

  return (
    <div>
      <h2>Group 1:</h2>
      <MultiCheckbox
        options={checkboxOptionsGroup1}
        selectedOptions={selectedOptionsGroup1}
        onCheckboxChange={handleCheckboxChangeGroup1}
      />

      <h2>Group 2:</h2>
      <MultiCheckbox
        options={checkboxOptionsGroup2}
        selectedOptions={selectedOptionsGroup2}
        onCheckboxChange={handleCheckboxChangeGroup2}
      />
    </div>
  );
}

export default MultiCheckboxContainer;
```
1. Imports:
* No changes here (imports `React` and `useState`).

2. `MultiCheckboxProps` interface: (remains the same)

3. `MultiCheckbox` component: (remains the same)

4. `MultiCheckboxContainer` component:
* This component defines two separate groups of checkboxes.
* It has separate state variables (`selectedOptionsGroup1` and `selectedOptionsGroup2`) to track the selected options for each group.
* It defines two separate `handleCheckboxChange` functions (`handleCheckboxChangeGroup1` and `handleCheckboxChangeGroup2`) to handle changes in each group. These functions update the corresponding state variable and optionally log the selected options.
* It renders two `MultiCheckbox` components, each with its own options, selected options, and the appropriate `handleCheckboxChange` function as a prop.

## Factorizing HandleCheckboxChange Functions

Lastly, we factorize `handleCheckboxChangeGroup1` and `handleCheckboxChangeGroup2` functions:

```jsx
import React, { useState } from 'react';

// Interface to define the props expected by the MultiCheckbox component
interface MultiCheckboxProps {
  options: string[]; // An array of strings representing the checkbox labels
  selectedOptions: string[]; // An array of strings representing the currently selected options
  onCheckboxChange: (updatedOptions: string[]) => void; // A function to be called when the selected options change
}

function MultiCheckbox({ options, selectedOptions, onCheckboxChange }: MultiCheckboxProps) {
  // Function to handle checkbox changes (local to the component)
  const handleCheckboxChange = (option: string) => {
    const updatedOptions = selectedOptions.includes(option)
      ? selectedOptions.filter((selected) => selected !== option)
      : [...selectedOptions, option];

    // Call the function provided by the onCheckboxChange prop
    onCheckboxChange(updatedOptions);
  };

  return (
    <div>
      {options.map((option) => (
        <label key={option} className="block text-gray-800 my-2">
          <input
            type="checkbox"
            value={option}
            checked={selectedOptions.includes(option)}
            onChange={() => handleCheckboxChange(option)}
            className="mr-2 focus:outline-none"
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Parent function that renders two groups of checkboxes
function MultiCheckboxContainer() {
  // Function to create a generic checkbox change handler
  const createCheckboxHandler = (setSelectedOptions: React.Dispatch<React.SetStateAction<string[]>>) => (updatedOptions: string[]) => {
    setSelectedOptions(updatedOptions);
    console.log('Selected options:', updatedOptions);
  };

  const checkboxOptionsGroup1 = ['Option 1', 'Option 2', 'Option 3'];
  const checkboxOptionsGroup2 = ['Choice A', 'Choice B', 'Choice C'];

  // State variables to track selected options for each group
  const [selectedOptionsGroup1, setSelectedOptionsGroup1] = useState<string[]>([]);
  const [selectedOptionsGroup2, setSelectedOptionsGroup2] = useState<string[]>([]);

  // Create handlers for each group using the generic createCheckboxHandler function
  const handleCheckboxChangeGroup1 = createCheckboxHandler(setSelectedOptionsGroup1);
  const handleCheckboxChangeGroup2 = createCheckboxHandler(setSelectedOptionsGroup2);

  return (
    <div>
      <h2>Group 1:</h2>
      <MultiCheckbox
        options={checkboxOptionsGroup1}
        selectedOptions={selectedOptionsGroup1}
        onCheckboxChange={handleCheckboxChangeGroup1}
      />

      <h2>Group 2:</h2>
      <MultiCheckbox
        options={checkboxOptionsGroup2}
        selectedOptions={selectedOptionsGroup2}
        onCheckboxChange={handleCheckboxChangeGroup2}
      />
    </div>
  );
}

export default MultiCheckboxContainer;
```

1. Imports:
* No changes here (imports `React` and `useState`).

2. `MultiCheckboxProps` interface: (remains the same)

3. `MultiCheckbox` component: (remains the same)

4. `MultiCheckboxContainer` component:
* Introduces a new function `createCheckboxHandler`.
  * This function takes a `setSelectedOptions` function (used to update the state) as an argument.
  * It returns a new function that takes `updatedOptions` (the new selected options) as an argument.
  * The returned function updates the state using `setSelectedOptions` and logs the selected options.
* This approach avoids code duplication by creating a generic handler that can be reused for both groups.
* The component defines `handleCheckboxChangeGroup1` and `handleCheckboxChangeGroup2` using `createCheckboxHandler` with the appropriate `setSelectedOptions` function for each group.