--- 
draft : false
date : 2024-03-17T14:37:18+01:00
title : "Nextjs Date Picker"
tags : ["gui", "react", "nextjs"]
showTableOfContents : true
type: "post"
---

To create a date picker in a Next.js application, you can utilize various libraries. One of the popular libraries for date picking in React applications is `react-datepicker`. Here's a step-by-step guide to integrating `react-datepicker` into your Next.js application:

## Set Date in Child Component

### Install react-datepicker
Run the following command in your terminal to install `react-datepicker`:
```
npm install react-datepicker
```

### Create a Date Picker Component
Create a new file for your date picker component. For example, you can name it `DatePicker.js`:
```jsx
import React, { useState } from 'react';
import DatePicker from 'react-datepicker';
import 'react-datepicker/dist/react-datepicker.css';

const DatePickerComponent = () => {
  const [selectedDate, setSelectedDate] = useState(null);

  const handleChange = date => {
    setSelectedDate(date);
  };

  return (
    <DatePicker
      selected={selectedDate}
      onChange={handleChange}
      dateFormat="MM/dd/yyyy" // Adjust date format as needed
    />
  );
};

export default DatePickerComponent;
```

### Use the Date Picker Component
Import and use the `DatePickerComponent` in any of your Next.js pages or components:
```jsx
import React from 'react';
import DatePickerComponent from '../components/DatePicker';

const MyPage = () => {
  return (
    <div>
      <h1>Date Picker Example</h1>
      <DatePickerComponent />
    </div>
  );
};

export default MyPage;
```

### Styling

`react-datepicker` comes with basic styling out of the box, but you can customize its appearance using CSS or by providing custom styles through its props.

### Additional Configuration:

You can explore the `react-datepicker` documentation for more customization options such as `minDate`, `maxDate`, custom date formatting, localization, etc.
If you need time selection along with the date, `react-datepicker` also supports time selection through its `showTimeSelect` prop.

## Set Date in Child Component


If you want to manage the state of the selected date in the parent component rather than the `datepicker` component itself, you can lift the state up. Here's how you can do it:

1. Move State to Parent Component: Move the state that manages the selected date from the datepicker component to its parent component.

2. Pass State and Handler as Props: Pass the selected date state and its handler function (onChange in this case) from the parent component to the datepicker component.

3. Update Parent State: Implement the handler function in the parent component to update the selected date state when the `datepicker` value changes.

Here's how you can modify the previous example to implement this:

> ParentComponent.js
```jsx
import React, { useState } from 'react';
import MyDatePicker from './MyDatePicker';

const ParentComponent = () => {
  const [selectedDate, setSelectedDate] = useState(null);

  const handleDateChange = date => {
    setSelectedDate(date);
  };

  return (
    <div>
      <h1>Date Picker Example</h1>
      <MyDatePicker selectedDate={selectedDate} handleDateChange={handleDateChange} />
      <p>Selected Date: {selectedDate ? selectedDate.toDateString() : 'No date selected'}</p>
    </div>
  );
};

export default ParentComponent;
```

> MyDatePicker.js:
```jsx
import React from 'react';
import DatePicker from 'react-datepicker';
import 'react-datepicker/dist/react-datepicker.css';

const MyDatePicker = ({ selectedDate, handleDateChange }) => {
  return (
    <div>
      <DatePicker
        selected={selectedDate}
        onChange={handleDateChange}
        dateFormat="dd/MM/yyyy"
      />
    </div>
  );
};

export default MyDatePicker;
```

In this setup:

1. `selectedDate` and `handleDateChange` are passed as props to the `MyDatePicker` component from its parent `ParentComponent`.
2. `MyDatePicker` component now receives these props and uses them to manage the selected date and update it when the user selects a new date.
3. The `ParentComponent` holds the state for the selected date and defines the handler function (`handleDateChange`) to update this state. It also displays the selected date.

With this approach, the selected date state is managed in the parent component, allowing you to easily access and manage the selected date across your application.