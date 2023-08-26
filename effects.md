# Effects

## The useEffect Hook

The useEffect hook is another hook provided by React. As with the useState hook, it needs to be imported. It allows us to synchronise a component with an external system - for example create a server connection or send an analytics log based on a React state.

```js
import { useState, useEffect } from "react";
```

The useEffect hook needs to be used inside our component function at the top level, and accepts a callback function as an argument.

useEffect is specifically for handling `Side Effects` that we want to run outside of an event handler - they used to keep a component synchronised with some external system. An effect should only do one thing - it should have a single purpose.

(Reminder: a side effect is when the function/component uses external code or performs any asynchronous processing. For example, fetching data from an API, or working with the DOM.)

useEffect will run after every render by default, but we can change this behaviour by passing in a `dependencey array` as a second argument.

Passing in an empty dependency array tells React only to run the effect on the initial render:

```js
useEffect(myFunction(), []);
```

Or, if we only want our effect to run only after changes to specific state or prop values, we can pass an array of those states or props:

```js
useEffect(myFunction(), [state1, state2, prop1]);
```

So, to be clear:

```js
useEffect(myFunction); // runs on every render, for all states.
useEffect(myFunction, [count]); // runs on every render only for the 'count' state.
useEffect(myFunction, []); // runs only on the first render (on mount) and never again.
```

In a way, this means useEffect is similar to an Event Listener, but in this case, the event it's listening for is a change to the state or prop provided to it within the `dependency array`. When the state or prop does change, React knows it needs to run the effect again.

# Fetching data from an API

The useEffect hook can be useful when fetching information from an API. We do not want to fetch this information on every render of the page.

```js
import { useState } from "react";
const RANDOM_QUOTE_URL = "https://inspo-quotes-api.herokuapp.com/quotes/random";

export default function QuoteFetcher() {
  // initiate state
  const [quote, setQuote] = useState({});

  // gets a quote from the API and sets the state
  async function fetchQuote() {
    const response = await fetch(RANDOME_QUOTE_URL);
    const jsonResponse = await response.json();
    const randomQuote = jsonResponse.quote;
    setQuote(randomQuote);
  }

  return (
    <div>
      <button onClick={fetchQuote}>Get Quote</button>
      <h1>{quote.text}</h1>
      <h2>{quote.author}</h2>
    </div>
  );
}
```

This snippet will display random quotes when a button is pressed. However, when the page first loads and the component is first rendered, only the button will be displayed - the state is initially empty, meaning our component includes empty `h1` and `h2` tags in the code.

We may want to display a random quote on the first render of the component, but not to go and fetch a new quote every time the page refreshes.

We may think that we can provide a function to useState to set an initial value, which we can in most situations, but `useState cannot accept async functions` - we'll be setting our state to a promise if we try this.

To solve this we use `useEffect`. However, similar to useState, useEffect does not like async functions. To work around this we wrap our asynchronous function in a synchronous function and then immediately invoke it.

```js
import { useState, useEffect } from "react";
//...
const [quote, setQuote] = useState({ text: "", author: "" });

useEffect(() => fetchQuote(), []);
```

This will, however, cause 2 renders to be performed. Once when the state is initialised and then again after the first useEffect execution. This can mean that on refresh, it will temporariliy display the previous quote and then load the new quote.

To counter this, a loader component me be useful.

## Cleanup

Cleanup is a function (cleanup function) that we can optionally return from our effect. The cleanup function runs on 2 occasions:

1. Before the effect is executed again; and
2. After a component has unmounted.

This gives us the ability to reset the side effect if necessary.

This is necessary whenever our side effect is still ongoing after the component has been re-rendered or unmounted.

For example:

- HTTP request is fired on initial render and refired on component rerender. We may then have a race condition bug, so we can cancel the request in our cleanup funciton.

A cleanup function must be returned from the effect.

```js
useEffect(() => {
  //... effect

  // Cleanup
  return () => {
    //... reverse effect
  };
});
```

## When are Effects executed?

1. Mount (initial Render)
2. Commit Phase
3. Browser Paint
4. **`Effect`**
5. Change to state/prop
6. Re-render
7. Commit
8. _`Layout Effect`_
9. Browser Paint
10. **`Cleanup`**
11. **`Effect`**
12. Unmount
13. **`Cleanup`**

## How to

### Cancel a HTTP Fetch request with Cleanup

```js
useEffect(
  function () {
    const controller = new AbortController(); // browser API

    async function fetchStuff() {
      const response = await fetch(url , { signal: controller.signal});
      // handle fetech response data
      // do stuff

      // cleanup
      function () {
        return controller.abort()
      }
    }
  }
)
```
