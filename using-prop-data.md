# Using Prop Data

## Expressions

Expressions are rendered simply using `{}` to be evaluated as JavaScript - see [Evaluating JS Expressions in JSX](./jsx-in-detail.md#evaluating-js-expressions-in-jsx)

## Conditionals

The most efficient way to use conditionals with prop data is to use ternary operators.

```jsx
// A component that displays if the user is old enough to drive

export default function CanDrive({dob}) {
  const age = //some calculation
  return (
    <p> You {age <= 17 ? 'are too young to drive' : 'can drive'}</p>
  )
}

// or

export default function CanDrive({dob}) {
  const age = //some calculation
  const message = `You ${age <= 17 ? 'are too young to drive' : 'can drive'}`;
  return (
    <p>{ message }</p>
  )
}
```

However, regulare if else statements do also work.

With conditionals we can also decide if an element is displayed.

```jsx
// display username if logged in

export default function Profile({isLoggedIn, username}) {
  return (
    {isLoggedIn ? <p>username</p> : null}
  )
}

// or short circuit

export default function Profile({isLoggedIn, username}) {
  return (
    {isLoggedIn && <p>username</p>}
  )
}
```

And of course, styling

```jsx
// display game results

export default function GameResults({isWinner}) {
  return (
    <div className={isWinner ? 'game-result__winner' : 'game-result__loser'}>
      <p>You {isWinner ? 'Win!' : 'Lose 😩'}</p>
    </div>
  );
}
```

## Render Arrays

By default if you attempt to render an array using React, it will simply render all values contained inside the array as a single string.

For example:

```jsx
export default function Test() {
  const myArray = [1, 2, 3, 4, 5];
  return (
    <p>{myArray}</p> // renders '12345'
  );
}
```

If we want to render each item in the array as it's own element, the best approach is to create a new array that contains the elements, and then render the new array.

```jsx
export default function Test() {
  const myArray = [1,2,3,4,5];
  return (
    {myArray.map(e => <p>{e}</p>)};
  )
}

// note: You still have to escape JSX using {} inside the map, even though the map itself is already escaped.
```
