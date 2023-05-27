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

When updating the state with our `set` function, we can either pass in the new state directly or with function. Where the new state is based on the previous state, we should use an `updater function` as seen below:

```js
setName('Steve');
setAge((age) => age + 10); // this is called an updater function
```

## State Initialiser Functions

These are simply functions used to generate the initial state where the state is not something as simple as just a number or word.

```js
functio generateGameBoard() {
  console.log('Creating Game Board!');
  return Array(5000);
}

export default function TicTacToe() {
  const [board, setBoard] = useState(generateGameBoard);
  return (
    <button>Click me to change state</button>
  )
}
```

Only the function name needs to be passed in to useState and React knows the execute the function when the state is first being set. Any subsequent re-render will not cause the function to be rerun.

## When does react re-render?

Put simply, react will only re-render a component where it identifies that the
value of the state has changed. It won't therefore re-render unnecessarily just because the `set` function has been called by the value remains the same. When the set function is called, it prompts react to run a comparison against the current state and new state, and if they are both the same, it will not re-render.

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
