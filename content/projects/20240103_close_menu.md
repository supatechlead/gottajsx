--- 
draft : false
date : 2024-03-04T14:37:18+01:00
title : "Closing the Context Menu on Outside Click Using ReactJS:"
showTableOfContents : true
type: "post"
---


Here's how to achieve the functionality of closing the context menu on outside click using ReactJS:

## useRef Hook:

We'll use the useRef hook to create a reference to the menu component itself. This allows us to check later if a click event originated from outside the menu.

```jsx
import { useRef } from 'react';

const MyComponent = () => {
  const menuRef = useRef(null);
  // ... rest of your component code
}
```

## handleClickOutside Function:

Define a function that handles clicks outside the menu. It will check if the clicked element is not part of the menu using `menuRef.current.contains(event.target)`.

```jsx
const handleClickOutside = (event) => {
  if (menuRef.current && !menuRef.current.contains(event.target)) {
    setShowMenu(false);
  }
};
```

## Adding Event Listener:

Use the `useEffect` hook to add a click event listener to the document body when the component mounts and remove it when the component unmounts.

```jsx
useEffect(() => {
  const handleOutsideClick = handleClickOutside;
  document.addEventListener('click', handleOutsideClick);
  return () => document.removeEventListener('click', handleOutsideClick);
}, []);
```

## Rendering the Menu:

Wrap your menu content with a `div` element and assign the `menuRef` to it. This allows us to check clicks against the menu element later.

```jsx
{showMenu ? (
  <div className="absolute bg-white rounded-lg py-1.5 w-[200px] shadow-xl border top-[40px] right-0" ref={menuRef}>
    {/* Your menu buttons here */}
  </div>
) : null}
```

## Complete Example:

```jsx
import { useRef, useEffect } from 'react';

const MyComponent = () => {
  const [showMenu, setShowMenu] = useState(false);
  const menuRef = useRef(null);

  const handleClickOutside = (event) => {
    if (menuRef.current && !menuRef.current.contains(event.target)) {
      setShowMenu(false);
    }
  };

  useEffect(() => {
    const handleOutsideClick = handleClickOutside;
    document.addEventListener('click', handleOutsideClick);
    return () => document.removeEventListener('click', handleOutsideClick);
  }, []);

  return (
    <div>
      <button onClick={() => setShowMenu(!showMenu)}>Open Menu</button>
      {showMenu ? (
        <div className="absolute bg-white rounded-lg py-1.5 w-[200px] shadow-xl border top-[40px] right-0" ref={menuRef}>
          {/* Your menu buttons here */}
          <button onClick={() => { router.push(`/profile/${userContext?.user?.id}`); setShowMenu(false); }}>
            <BiUser size="20" />
            <span className="pl-2 font-semibold text-sm">Profile</span>
          </button>
          {/* ... other buttons */}
        </div>
      ) : null}
    </div>
  );
};
```

This code ensures that the menu disappears whenever a click happens outside the menu area.