# Effects

## The useEffect Hook

The useEffect hook is another hook provided by React. As with the useState hook, it needs to be imported. It allows us to synchronise a component with an external system - for example create a server connection or send an analytics log based on a React state.

```js
import {useState, useEffect} from 'react';
```

The useEffect hook needs to be used inside our component function, and accepts a callback function as an argument.

useEffect will always run after the first render, and by default will also run after every other re-render, but this can be changed.

We can tell it to only run on the first render, and never again by passing in an empty array as a second argument:

```js
useEffect(myFunction(), []);
```

Or, if we only want our effect to run for specific states, we can pass an array of those states as a second argument:

```js
useEffect(myFunction(), [state1, state2]);
```

So, to be clear:

```js
useEffect(myFunction); // runs on every render, for all states
useEffect(myFunction, [count]); // runs on every render only for the 'count' state
useEffect(myFunction, []); // runs only on the first render and never again
```
