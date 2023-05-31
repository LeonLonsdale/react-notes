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

## The htmlFor Property

In the previous example, there is a small issue in that JSX does not recognise the `for` property when labelling forms.

```html
<label for="username">Enter username</label>
<input type="text" id="username" placeholder="username" />
```

This is due to a similar issue as seen when attempting to use `class` to apply styles - `for` is a reserved keyword in JavaScript, used in `for loops`.

The quick fix is to use `htmlFor`:

```html
<label htmlFor="username">Enter username</label>
<input type="text" id="username" placeholder="username" />
```

## Forms with multiple inputs

Where a form has multiple inputs, it can become a bit messy managing multiple states for all those inputs. It's therefore better to just have a single state which is an object containing a property for each input.

Using this method, it becomes important for each input field to have a `name` property which should match corresponding the state object property name. This allows us to access and update the correct state property by accessing the event object `target.name` field.

```js
import {useState} from 'react';

const UserSignupForm = () => {
  const defaultState = {
    firstName: '',
    lastName: '',
    username: '',
    password: '',
    email: '',
  };

  const [formData, setFormData] = useState(defaultState);

  const handleChange = (e) => {
    const field = e.target.name;
    const value = e.target.value;
    setFormData((oldData) => {...oldData, [field]: value})
  }

  return (
    <input type="text" name="firstName" id="firstName" placeholder="First name" value={formData.firstName} onChange={handleChange}>
  )
};
```
