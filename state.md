# useState()

## Intro to useState()

`useState()` is a react hook that allows us to add a state variable to a component. The hook accepts a value, and then returns an array containing that value and a function to change the value. Changing the value using this function prompts react to re-render the component. This allows us to have interactive modules.

Before we can use it in a component, we have to import it:

```js
import {useState} from 'react';
```

## Using useState()

We declare our state at the top level of our component. It must be within the component function and cannot be at the top level of the document. It also cannot be declared within loops or conditions.

We pass in our initial state, and the hook returns an array containing the initial state and a function that allows us to change the state. We can therefore deconstruct these two items at declaration. Conventionally the function is named beginning with `set`. The set function allows us to update the state to a new value and trigger the component to re-render, showing the new value in the UI.

```js
// syntax
const [something, setSomething] = useState(initialState);

// the 'something' variable will always store the current state.

setSomething(newState);
```

We can declare as many states as we like to a component.

```js
export default function MyComponent() {
  const [name, setName] = useState('Leon');
  const [age, setAge] = useState(39);
}
```

When updating the state with our `set` function, we can either pass in the new state directly or with an `update function`:

```js
setName('Steve');
setAge((age) => age + 10); // this is called an updater function
```
