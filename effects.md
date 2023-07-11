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
useEffect(myFunction); // runs on every render, for all states.
useEffect(myFunction, [count]); // runs on every render only for the 'count' state.
useEffect(myFunction, []); // runs only on the first render (on mount) and never again.
```

# Fetching data from an API

The useEffect hook can be useful when fetching information from an API. We do not want to fetch this information on every render of the page.

```js
import {useState} from 'react';
const RANDOM_QUOTE_URL = 'https://inspo-quotes-api.herokuapp.com/quotes/random';

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

To solve this we use `useEffect`. However, similar to useState, useEffect does not like async functions. To work around this we wrap our function in another function and then immediately invoke it.

```js
import {useState, useEffect} from 'react';
//...
const [quote, setQuote] = useState({text: '', author: ''});

useEffect(() => fetchQuote(), []);
```
