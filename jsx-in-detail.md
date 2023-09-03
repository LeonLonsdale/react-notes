# JSX In Detail

## Importing and Exporting Components

Typically 1 component will be in its own file.

For example a Header component would be in a Header.jsx file. The header may contain a Navbar component which would be in a Navbar.jsx file, and a Navbar may contain reusable Navbar Items which would be in a NavbarItem.jsx file. The component functions should have the dame name as the file.

We import and export components using ES6 import/export syntax:

```jsx
// in the Header.jsx file

import Navbar from './Navbar.jsx';

export default function Header() {
  return (
    <Navbar />
    <img ... />
  )
}
```

[Back to Contents](./README.md) - [Back to Top](#)

## JSX Rules

- Self-closing elements such as `<br />` and `<input />` must explicityly be closed.
  ```jsx
  export default function LoginForm() {
    return (
      <form>
        <input type="password"> // this is not correct.
        <input type="password" /> // this is correct
      </form>
    )
  }
  ```
- Components may only return a single top-level element.

  ```jsx
  // this is not correct - its returning 3 top-level elements.
  export default function LoginForm() {
    return (
      <input type="text" />
      <input type="email" />
      <input type="password" />
    )
  }

  // this is correct - it's returning 1 top-level Form element.
  export default function LoginForm() {
    return (
      <form>
        <input type="text" />
        <input type="email" />
        <input type="password" />
      </form>
    )
  }
  ```

[Back to Contents](./README.md) - [Back to Top](#)

## Fragments

A fragment is a wrapper element used as a top-level element that we can use to wrap the returned content from our components. We use a fragment when we don't want to pollute our HTML with unnecessary elements, such as `<div></div>` which we might use to satisfy the rules of JSX - only a single top-level element can be returned.

A fragment is written `<></>` and wraps around the other returned elements, satisfying the JSX rule, but the fragment itself is never rendered in the resulting HTML.

[Back to Contents](./README.md) - [Back to Top](#)

## Evaluating JS Expressions in JSX

To evaluate regular JavaScript in JSX, we have to escape the JSX.

To escape JSX, we wrap our JavaScript in clurley bracers `{}`.

```jsx
export default function PointlessComponent() {
  return (
    <p>The sum of 4 and 9 is {4 + 9}</p>
  )
};
// evaluates 4 + 9 as JavaScript and renders: "The sum of 4 and 9 is 13".

export default function PointlessComponent() {
  const myName = "Leon";
  return (
    <p>Hello, my name is { myName }.</p>
  )
}
// evaluates the variable 'myName' to Leon, and renders: "Hello, my name is Leon"
```

[Back to Contents](./README.md) - [Back to Top](#)

## Styling Components

Conventionally we create a css stylesheet with the same name as the component. So a `Navbar.jsx` file would have a corresponding `Navbar.css` file. We can import this css file in the same way as a component:

```jsx
import "./Navbar.css";
```

When applying the styles to the component, it's important to remember that we're not working with HTML, but JavaScript, so we cannot use `class` as that's a reserved JavaScript keyword used for creating classes for OOP. Instead we have to use camelCased `className`. This is true for all attributes you'd usually type in HTML - they all need to be camelcase when applied into our JSX.

```jsx
export default function Navbar() {
  return (
    <nav className="nav">
      <Logo />
      <NavItems />
    </nav>
  );
}
```

[Back to Contents](./README.md) - [Back to Top](#)
