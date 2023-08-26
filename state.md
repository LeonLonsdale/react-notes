# useState()

## Intro to useState()

- `useState()` is a react hook that allows us to add a state variable to a react component.
- Before we can use it in a component, we have to import it:

```js
import { useState } from "react";
```

component. The hook accepts a value, and then returns an array containing that value and a function to change the value. Changing the value using this function prompts react to re-render the component. This allows us to have interactive modules.

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

When updating the state with our `set` function, we can either pass in the new state directly or with function. Where the new state is based on the previous state, we should use an `updater function` as seen below:

```js
setName("Steve");
setAge((age) => age + 10); // this is called an updater function
```

## When does react re-render?

Put simply, react will only re-render a component where it identifies that the
value of the state has changed, or if its parent is re-rendered. It won't therefore re-render unnecessarily just because the `set` function has been called by the value remains the same. When the set function is called, it prompts react to run a comparison against the current state and new state, and if they are both the same, it will not re-render. (see: [How React Works](./how-react-works.md#recap))

It's additionally worth noting that when react does re-render, it does not reset the state back to the original value as you might expect, considering the entire component code is re-run. Instead react knows that it's a re-render and not first load, and so maintains the current state.

## React and Objecs with State

The comparison mentioned above causes a bit of a problem when our state is an object. It's going to try to see if the state has changed as normal, but our state is an object, and the object variable is a memory address, which does not change when we make changes to the content of the object - the object has not changed, only the content has.

The way we handle this is to update the state to a completely new object in memory.

```js

const initScores = { p1Score: 0, p2Score: 0 };
const [scores, setScores] = useState(initScores);
const handleClick = (e) => {
  const {id} = e.target;
  setScores(oldScores => { ...oldScores, [id]: oldScores[id] + 1 })
}
return (
  <button id="p1SCore" onClick={handleClick}>+1 P1 Score</button>
  <button id="p2Score" onClick={handleClick}>+1 P2 Score</button>
)
```

Here we use the update function syntax to create a new object rather than updating the existing object.

## React and Arrays with State

### Adding items

Arrays have the same problem as Objects - they're a reference to memory rather than the array itself.

As such, we also need to create a new array and pass that into our setter function. Again this can use the spread syntax

```js
setArray((oldArray) => [...oldArray, "new items"]);
```

### Removing items

The easiest way to remove items is to use the `Array.filter()` method. However, this best works using the Key prop id.

This means that we'll need to implement a different way to call a function with `onClick` such that we can pass the key prop id in. We do this with a call back:

```js
onClick={() => handleClick(element.id)} // assumes a property called ID.
```

From here we can now use the filter method:

```js
const handleClick = (id) => setArray(oldArray => oldArray.filter((e) => e.id !=== id));
```

## State Batching & Callbacks

Essentially, the Render & Commit doesn't start until the function or event that requests the change has completed operations - all state changes will be batched and completed at the same time at the end. That is to say, if an Event Handler changes 3 states, each state is not updated one at a time and thus triggering 3 Render & Commits. They'll all be updated before triggering a single Render & Commit.

This can have some unexpects consequences. Imagine:

```js
const reset = () => {
  setName("");
  console.log(name);
  setEmail("");
  setPassword("");
};
```

In the example above, since state is stored in the Fiber Tree during rendering, and at the time of the `console.log` call, the re-render has not yet taken place, the `name` state will still be the previous state and not the new state. This is called `stale state`. This is true even if a single state is being updated.

This is the reason callbacks are used when setting state based on the current state:

```js
setIsTrue((cur) => !cur);
```

These callbacks, as with standard JS behaviour, are added to a callback queue to be actioned later. This means they're actioned after the function that called them, at which point the state has been updated.

If automatic batches is an issue at any point, the state can be wrapped in `ReactDOM.flushSync()`.

## Initialising State with Callbacks

Whenever our initial state should be set to some value that may, for example, be held in a database, or even... localStorage, the best way to set this initial state is with callback function.

This callback should not accept any arguments, and should return the value. We should never call a function when setting state, only pass the function in.

```js
// this is wrong. The function will be called on every render.
const [state, setState] = useState(getInitialState());

// this is wrong, the function should not accept arguments:
const [state, setState] = useState((arguments) => {
  // do stuff
});

// this is correct:
const [state, setState] = useState(() => {
  // do stuff
});
```
