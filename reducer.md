# The useReducer Hook

## Overview

The `useReducer` hook is a more advanced way to manage state.

It is typically used where there are multiple pieces of state that are related to each other, while useState woud be used for single, indepenedent pieces of state.

Differing from useState, with useReducer, we don't simply tell it what the new state is but we give it an `action` so that the reducer function can work out what the new state is, and which piece(s) of state are affected.

## Using useReducer

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

`initialState` may be an object, in which case we could directly deconstruct it:

```js
const initialState = {
  status: "", // active / ended / ready
  scores: {
    playerOne: 0,
    playerTwo: 0,
  },
};

const reducer = () => {};

const [{ status, scores }, dispatch] = useReducer(reducer, initialState);
```

We create the reducer function according to the actions we want it to perform before returning the state. The reducer function always receives the initial state, and an action. The action is the value passed into the dispatch function. This is typically done with a `switch statement`.

```js
function reducer(state, action) {
  switch (action.type) {
    case "case1":
      return {
        //new state
      };
    case "case2":
    // ...
    default:
      throw new Error("Unknown action");
  }
}
```

The action is passed into the dispatch function which hands it to the reducer function. The action can then be used by that reducer function to return the updated state.

The action is typically an object. It is somewhat of a convention for that object to be in the form of `{ type: 'type', payload: 'payload' }`.

For a fuller example:

```js
// initial state

const initialState = {
  status: '', // active / ended / ready
  scores: {
    playerOne: 0,
    playerTwo: 0,
  }
}

// initiate the reducer function
function reducer(state, action) {
  switch (action.type) {
    case 'increase':
      return {
        ...state,
        scores[action.player]: state.scores[action.player] + payload ? payload : 1
        };
    case 'decrease':
      return {
        ...state,
        scores[action.player]: state.scores[action.player] - payload ? payload : 1
        };
    case 'gameOver':
        return { ...state, status: 'ended' };
    case 'started':
        return { ...state, status: 'active' };
    default:
      throw new Error('Unknown action');
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, initialState);

  // or

  const [{status, scores}, dispatch] = useReducer(reducer, initialState);

  // event handlers can be created with different actions
  const decreaseByOne = function () {
    dispatch({ type: "decrease", player: 'playerOne' });
  };

  const decreaseByTwo = function () {
    dispatch({ type: "decrease", payload: 2, player: 'playerTwo' });
  };

  const increaseByFour = function () {
    dispatch({ type: "increase", payload: 4, player: 'playerOne' });
  };
}
```

The status property of the state object can be used for conditional rendering, amongst other things:

```jsx
export default function App() {
  // useReducer
  // event handlers
  return (
    <Header />
    <Main>
      {status === 'ready' && <StartScreen />}
      {status === 'active' && <GameScreen />}
      {status === 'ended' && <ScoresScreen />}
    </Main>
    <Footer />
  )
}
```

## Deciding when to use useReducer

If the answer to any of the below are `yes`, we should probably use useReducer.

1. Do we have more than one piece of state that frequently need to be updated together?
2. Do we have complex state? - more than 3 or 4 pieces of related state.
3. Do we have a lot of event handlers that are making the component large and confusing?

## Recap

1. When we call useReducer, we pass in a reducer function and the initial state.
2. The useReducer hook returns our state object and a dispatch function.
3. State used with useReducer is typically an object of related pieces of state
4. The dispatch function is used to trigger state updates (in a similar way to using a setState function).
5. When we call the dispatch function, we pass in an action object.
6. An action is an object that describes how state should be updated, typically containing an action type and a payload property.
7. The dispatch function hands over (dispatches) the action to the reducer function.
8. The reducer function contains all the logic for updating the state, and decouples logic from components.
9. The reducer is a pure function, with no side effects, and always returns a new state.
10. The reducer function uses the action object to identify what the new state will be, and returns that state.
