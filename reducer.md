# The useReducer Hook

## Using useReducer

The useReducer hook is a more advanced way to manage state.

As ever, the hook must be imported

```js
import { useReducer } from "react";
```

The hook is initiated in a similar way to the standard useState hook with a few differences:

1. Rather than a setState function, it returns a `dispatch function` in addition to the current state.
2. As well as the initial state, it accepts a `reducer function`.

```js
const [state, dispatch] = useReducer(reducerFunc, "initialState");
```

We create the reducer function according to the actions we want it to perform before returning the state. The reducer function always receives the initial state, and an action. The action is the value passed into the dispatch function.

```js
function reducer(state, action) {
  // do something
  // return something
}
```

So the action is passed into the dispatch function, and can then be used within the reducer function to return the updated state. The action passed in may, and typically is, an object. It is somewhat of a convention for that object to be in the form of `{ type: 'type', payload: 'payload' }` For a fuller example:

```js
// initiate the reducer function
function reducer(state, action) {
  // deconstruct the action. Payload are optional.
  const { type, payload? } = action;
  // conditionally update the current state based on the parameters
  // of the action
  if (type === "increase") return state + payload ? payload : 1;
  if (type === "decrease") return state - payload ? payload : 1;
}

export default function App() {
  const [count, dispatch] = useReducer(reducer, 0);

  // event handlers can be created with different actions
  const decreaseByOne = function () {
    dispatch({ type: "decrease"});
  };

  const decreaseByTwo = function () {
    dispatch({ type: "decrease", payload: 2 });
  };

  const increaseByFour = function () {
    dispatch({ type: "increase", payload: 4 });
  };
}
```
