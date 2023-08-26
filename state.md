# useState()

## Intro to useState()

- `useState()` is a react hook that allows us to add a state variable to a react component.
- Before we can use it in a component, we have to import it:

```js
import { useState } from "react";
```

## Initialising State

- We can only declare our state within a component function or custom hook, and only at the top level.
- State should neve be declared within loops or conditions.
- The hook returns an array containing a state variable and a function to update that variable.
- We can deconstruct this array on declaration.

```js
const [state, setState] = useState();
```

- It is convention to name the setter function beginning with `set`.
- We can set our initial state by passing in the value, a variable storing the value, or a callback function:

```js
const [state, setState] = useState(10);

// or

const initialValue = 10;
const [state, setState] = useState(initialValue);

// or

const [state, setState] = useState(() => {
  callback;
});

// or

const callback = () => {
  // do stuff
  return value;
};
```

- When using a callback to declare state, the callback

  - must not be immediately called
  - must not accept arguments
  - must be pure
  - must return a value

- The function will only be run on first render, and will be ignored on subsequent renders.
- We can declare as many states as we like to a component.

```js
export default function MyComponent() {
  const [name, setName] = useState("Tim");
  const [age, setAge] = useState(50);
}
```

## Setting State

- We update state with our setter function. From the various examples above, this would be:
  - `setState`
  - `setName`
  - `setAge`
- When using our setter function, we pass in the new value.

```js
setState(NewState);
```

- If the new state is based on the value of the current state, we pass in an updater function (which is basically a callback)

```js
setIsOpen((currentIsOpen) => !currentIsOpen);
```

- Since arrays and objects point to a memory reference rather than the content, we must use updater functions to rebuild a new array or object.

```js
const newName = 'Some Name';
setNames( (currentNames) => [...currentNames, newName])

setScores( (currentScores) => {...currentScores, [Team1]: currentScores[Team1] + 1})

setTodos( (currentTodos) => currentTodos.filter((todo) => todo.id !== id));
```

## When does react re-render?

- React only re-renders when it identifies that the state has changed.
- This does not mean whenever the setter function is called, as it may be called to set to the same value, which would be an unnecessary re-render.
- React will check whether the value has changed an requires a re-render during the rendering phase.
  - see: [How React Works](./how-react-works.md#recap)
- State is also not reset to it's original value on re-renders. The initial state is ignored on rerenders.

## State Batching & Callbacks

- React does not perform the render phase until the functions that have called state changes have completed.
- This results in batching state changes, so that a function changing 3 states does not trigger 3 re-renders - they will all be done at the same time. State values are not updated until render.
- This can have some unexpects consequences. Imagine:

```js
const reset = () => {
  setName("");
  console.log(name);
  setEmail("");
  setPassword("");
};
```

- Consider the example and remember that state is stored in the Fiber Tree during rendering
- At the time of the `console.log` call, the re-render has not yet taken place, the `name` state will still be the previous state and not the new state.
- This is called `stale state`.
- This is true even if a single state is being updated.
- This is the reason callbacks are used when setting state based on the current state:

```js
setIsTrue((cur) => !cur);
```

- These callbacks, as with standard JS behaviour, are added to a callback queue to be actioned later. This means they're actioned after the function that called them, at which point the state has been updated.
- If automatic batches is an issue at any point, the state can be wrapped in `ReactDOM.flushSync()`.
