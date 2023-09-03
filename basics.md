# React

## Intro to React

React is a Frontend Library that helps us build user interfaces.

The interfaces are built using `components`. Components are a piece of the user interfaces that can be reused across the interfaces, they are a javascript function that returns JSX.

Documentation can be found at [react.dev](https://react.dev).

[Back to Contents](./README.md) - [Back to Top](#)

## Intro to JSX

JSX stands for Javascript Syntax Extension. It allows us to write markup that looks like HTML directly in our JavaScript, but it has to be transpiled into real JavaScript to actually work.

```jsx
export default function App() {
  return (
    <div>
      <h1>Hello world!</h1>
    </div>
  );
}
```

[Back to Contents](./README.md) - [Back to Top](#)

## Basic React Structure

- Highest level component is conventionally called `App.jsx`
- Component files and functions are named capitalised.
- Components are called within the App component.
- The App component is rendered, which in trun renders the other components.

```jsx
export default function App() {
  return (
    <>
      <Header />
      <Body />
      <Footer />
    </>
  );
}
```

[Back to Contents](./README.md) - [Back to Top](#)
