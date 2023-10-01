# Memoisation

## What is memoisation?

Optimisation technique that executes a pure function once, and saves the result in memory.

If we try to execute the function again with the **`same arguments as before`**, the previously saved result will be returned, **`without executing the function again`**.

In react we can perform memoisation by:

- Using `memo` for components
- Using `useMemo` for objects
- Useing `useCallback` for functions

## memo

`memo` is a feature of React that is used to create a component that will `not re-render when its parent rerenders`, as long as the `props stay the same between renders`. The component will still re-render when its `own state changes` or when a `context that it's subscribed to changes`.

It therefore only makes sense to use `memo` whenever the component, with the same props:

- is slow rendering (`heavy`)
- re-renders often

To use memo, we need to import it from react:

```js
import { memo } from "react";
```

To use it, we pass our component into reacts `memo()` function as an argument, and save the output to a variable with the same name as the component.

The component argument can have the same name as the new memo name or can be an anonymous function.

```js
// in App.jsx

return <MyComponent show={true} />;

// in our Component file

import { memo } from "react";

const MyComponent = memo(function MyComponent({ show }) {
  return jsx;
});

// OR

const MyComponent = memo(function ({ show }) {
  return jsx;
});
```

When using memo in a component within its own file, and is being exported, we can simply memo the export:

```js
export default memo(MyComponent);
```

There are some limitations to `memo` however. As memo only works if the props remain the same, this means that the props cannot be objects or arrays - they'll be recreated on every rerender, and will therefore be a different memory reference - or a different prop.

We can use `useMemo` and `useCallback` to overcome this limitation.

## useMemo & useCallback

- Use to memoise values (including objects and arrays)
- Use to memoise functions
- Values passed into useMemo and useCallback will be stored into memory and returned in subsequent re-renders, as long as the dependencies remain the same.
  - Dependancies are similar to the dependencies passed as an array into `useEffect`.
  - The value will be re-created only whenever one of these dependencies change.

The three main use cases are:

1. To prevent wasted renders (working in conjunction with `memo`).
2. To avoid expensive re-calculations on every render.
3. To memoise values used in the dependency array of other hooks.

### useMemo

useMemo accepts a callback function. We use the callback function to return the value we are memoising.

```js
// in App.jsx
import { useMemo } from "react";

const myOptions = useMemo(() => {
  return {
    show: true,
  };
}, []);

function App() {
  return <MyComponent show={myOptions} />;
}

// in our Component file

import { memo } from "react";

const MyComponent = memo(function MyComponent({ show }) {
  return jsx;
});
```

### useCallback

This works in the same way as useMemo but accepts a function:

```js
import { useCallback } from 'react';

const handleClick = useCallback(function handleClick() { ... } , []);

```
