# Forms

## Controlled Components

By default, html forms have their own state, used to record user inputs prior to submission. This is not the same state as the react state.

In React, we want our React state to know exactly what is happening in the form at any given moment. To do that we create a `controlled component`.

`One Source of Truth`: We want our React state to be the single source of truth. It must control what is shown and what happens when a user types inputs - using `onChage()` to record each keypress from the user, into the state. This is a `controlled component`.

This does mean that each input in a form must have it's own state.

For example, take a username field on a login form:

```js
import {useState} from 'react';

function UsernameInput() {
  // create our state and set it to an empty string
  const [username, setUsername] = useState('');
  // onChange handler, gets input value from event object and update state
  const updateUsername = (e) => {
    setUsername(e.target.value);
  };
  // username input field, value displayed is state
  // onChange triggers handler. Any keypress in the field triggers this.
  return (
    <formfield>
      <label for="username">Enter username</label>
      <input
        type="text"
        placeholder="username"
        value={username}
        id="username"
        onChange={updateUsername}
      />
    </formfield>
  );
}
```

What's happening here?

1. User presses any key while in the username input field.
2. The `onChange` event registers this and triggers `updateUsername`
3. `updateUsername` gets the new input field value from the event object and applies this value to the state, overwriting the previous state.
4. This triggers a re-render of the `UsernameInput` component.

For all intents and purposes, it will look as though the user is typing as normal in the input field, however, the field is being re-rendered on every keypress.
