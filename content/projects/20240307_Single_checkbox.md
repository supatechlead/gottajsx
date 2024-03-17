--- 
draft : false
date : 2024-03-07T14:37:18+01:00
title : "Single Checkbox in React and TypeScript"
tags : ["gui", "react", "nextjs"]
showTableOfContents : true
type: "post"
---

Previous post deeply explained how to create Multiple checkbox component

## Multiple Checkbox Component

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
* `import React, { useState } from 'react';`
This line imports the necessary libraries: React for building the UI components and `useState` hook for managing component state.

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

4. `MultiCheckboxContainer` component:
* This component defines two separate groups of checkboxes.
* It has separate state variables (`selectedOptionsGroup1` and `selectedOptionsGroup2`) to track the selected options for each group.
* It defines two separate `handleCheckboxChange` functions (`handleCheckboxChangeGroup1` and `handleCheckboxChangeGroup2`) to handle changes in each group. These functions update the corresponding state variable and optionally log the selected options.
* It renders two `MultiCheckbox` components, each with its own options, selected options, and the appropriate `handleCheckboxChange` function as a prop.

## Unique Checkbox Component

Here's how to transform multiple checkboxes component to single checkbox component:

```jsx
import React, { useState } from 'react';

// Interface to define the props expected by the SingleCheckbox component
interface SingleCheckboxProps {
  options: string[]; // An array of strings representing the checkbox labels
  selectedOption: string | null; // A string representing the currently selected option, or null if none selected
  onCheckboxChange: (updatedOption: string) => void; // A function to be called when the selected option changes
}

function SingleCheckbox({ options, selectedOption, onCheckboxChange }: SingleCheckboxProps) {
  // Function to handle checkbox changes (local to the component)
  const handleCheckboxChange = (option: string) => {
    // Call the function provided by the onCheckboxChange prop
    onCheckboxChange(option);
  };

  return (
    <div>
      {options.map((option) => (
        <label key={option} className="block text-gray-800 my-2">
          <input
            type="radio"
            value={option}
            checked={selectedOption === option}
            onChange={() => handleCheckboxChange(option)}
            className="mr-2 focus:outline-none"
          />
          {option}
        </label>
      ))}
    </div>
  );
}

// Parent function that renders a group of radio buttons
function SingleCheckboxContainer() {
  // State variable to track selected option
  const [selectedOption, setSelectedOption] = useState<string | null>(null);

  // Function to handle radio button change
  const handleCheckboxChange = (updatedOption: string) => {
    setSelectedOption(updatedOption);
    console.log('Selected option:', updatedOption);
  };

  const checkboxOptions = ['Option 1', 'Option 2', 'Option 3'];

  return (
    <div>
      <h2>Choose One:</h2>
      <SingleCheckbox
        options={checkboxOptions}
        selectedOption={selectedOption}
        onCheckboxChange={handleCheckboxChange}
      />
    </div>
  );
}

export default SingleCheckboxContainer;
```

The modifications made include:

1. Renaming the component and its props to reflect that it's now for single-choice selection (`SingleCheckbox` and `SingleCheckboxProps`).

2. Replacing checkboxes (`<input type="checkbox" />`) with radio buttons (`<input type="radio" />`), ensuring only one choice can be selected at a time.

3. Updating the component props to use `selectedOption` to track the selected option instead of a list of selected options.

4. Modifying the logic for handling checkbox changes to directly update the selected option with the new value rather than filtering a list of selected options.

With these changes, the code now allows for single-choice selection.