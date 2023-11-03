# Redux

- 3rd Party Library to manage global state
- Standalone library that is easy to integrate with React using `react-redux`
- All global state is stroed in one globally accessible store, which can be easily updated using `actions`.
- When the global store is updated, all components that consume data from that store will be re-rendered.
- It is conceptually similar to using ContextAPI + useReducer.

There are two versions:

- Classic Redux
- Redux Toolkit (more modern)

## When should we use Redux?

It's a good idea to use Redux (or similar) when dealing with a lot of Global UI state that needs to be frequently updated.

## How does Redux Work?

1. An event handler in a component calls an action creator - this writes the actions
2. The actions are passed to a dispatch
3. The dispatch passes on our action to the Store
4. The store contains multiple reducer functions, and the current state\*
5. Thew new state is output
6. The applicable components re-redner.

\*Each reducer is a pure function that calculates the next state (state transition) based on the action and the current state. There should be one reducer for each application feature. For example; shopping cart, user data, themes.

## Using Oldschool Redux

Install:

```
npm i redux
```

It's common practice to keep redux stores in a separate file. For example, store.js.

### Initial State

We can set up initial state in the same way as with useReducer.

```js
const initialState = {
  properties: values,
};
```

### Reducer Functions

Again, this is done in the same way as with useReducer.

```js
function reducer(state = initialState, action) {
  switch (action.type) {
    // cases
    default:
      return state;
  }
}
```

### Create a Store

To create a store we need to import the createStore function from Redux.

```js
import { createStore } from "redux";
```

Note: this function is deprecated, but is left in the package for learning purposes.

Calling this function returns the store object which can be saved. When calling, we pass in the reducer function.

```js
const store = createStore(reducer);
```

our `store` object now has access to the following methods.

```js
store.dispatch();
store.getState();
```

### Dispatch

The dispatch method works in a similar way to dispatches with useReducer - we pass in an action. This can either be an `action object` or an `action creator function`.

```js
store.dispatch({ type: "theType", payload: "value" });

// or

store.dispatch(actionCreator());
```

### Action Creators

An action creator is simply a pure function that returns the action object.

```js
function deposit (amount) {
    return { { type: 'account/deposit', payload: amount } }
}

// or

const deposit = (amount) => ({ type: 'account/deposit', payload: amount });

// remember that if using arrow functions, the function must be declared before it is called.
```

### Actions

Since a store will typically contain multiple reducers - one for each app feature - it is standard practice to use the feature name or some form of categorisation in the payload type field. The payload can also be an object of multiple values.

```js
{ type: 'account/deposit', payload: amount }
{ type: 'account/withdraw', payload: amount }
{ type: 'user/emailUpdate', payload: { oldEmail, newEmail } }
```
