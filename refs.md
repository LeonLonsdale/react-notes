# Refs

## Overview

- a Ref is an object with a property named `current` on it that is **mutable**, and persistant across renders.
- Refs have 2 use cases:
  - Creating a variable that we need to keep the same between renders.
  - Select and store DOM elements.
- Refs are for data that is not rendered. They're usually used in event handlers or effects and not in JSX (otherwise use State).
- You are not allowed to read or write the .current in render logic.
- Updating a ref does not cause a re-render like State does.
- Unlike state, a ref value is available immediately after setting it.
- Like useState and useEffect, it must be imported

```js
import { useState, useEffect, useRef } from "react";
```

[Back to Contents](./README.md) - [Back to Top](#)

## Using Refs

- We initialise a ref in a similar way to state, only we do not need to destructure an array into.
- Calling useRef returns a single object, which we can name whatever we want:

```js
const myRef = useRef();
```

- We can pass in an initial Ref value or leave it undefined - or even set it to null.

```js
const myRef = useRef();
const myRef = useRef(0);
const myRef = useRef(null);
```

- If we are using the Ref to store a DOM element, it's common to set it to `null` by default, and we need to let react know which element it references by adding a ref property to the element, and setting to the ref variable.

```js
const SomeComponent = () => {
  const myRef = useRef(null);

  return <domElement ref={myRef} />;
};
```

- We then manipulate or otherwise make use of the ref in a useEffect or eventHandler. For example, to set a DOM element as active/in focus:

```js
const SomeComponent = () => {
  const domElement = useRef(null);

  useEffect(() => {
    domElement.current.focus();
  }, []);

  return <domElement ref={domElement} />;
};
```

[Back to Contents](./README.md) - [Back to Top](#)

## Use Cases

- Storing DOM elements
- Recording user or page statistics that we don't need to display in the DOM, such as:
  - How many times did they click something?
  - How long did it take from page render to select an item?
  - How many times did the page re-render?
- Storing the previous state value.

```js
// keep a record of the previous state value
export default function App() {
  const [name, setName] = useState("");
  const prevName = useRef("");

  useEffect(() => {
    prevName.current = name;
  }, [name]);

  return (
    <p>
      My name is {name}. It used to be {prevName.current}
    </p>
  );
}
```

[Back to Contents](./README.md) - [Back to Top](#)
