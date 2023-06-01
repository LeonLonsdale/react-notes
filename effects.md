# Effects

## The useEffect Hook

The useEffect hook is another hook provided by React. As with the useState hook, it needs to be imported. It allows us to synchronise a component with an external system - for example create a server connection or send an analytics log based on a React state.

```js
import {useState, useEffect} from 'react';
```

The useEffect hook needs to be used inside our component function, and accepts a function as an argument.

useEffect will always run after the first render, and by default will also run after every other re-render, but this can be changed.
