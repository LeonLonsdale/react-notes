# React Router

## Setup

React Router doesn't come with React so it has to be downloaded separately:

```
npm i react-router-dom
```

and then imported into the file it's going to be used in:

```js
import { BrowserRouter, Routes, Route } from "react-router-dom";
```

## Basic Use

The BrowserRouter, Routes, and Route component functions imported must all be used to tell React how to use our routes:

```js
const App = () => {
    return (
        <BrowserRouter>
        <Routes>
            <Route path='/' element={<Homepage />} />
            <Route path='about' element={<About />} />
            <Route path='contact' element={Contact />} />
            <Route path='*' element={PageNotFound />} />
        </Routes>
        </BrowserRouter>
    )
}
```

Each route must have a path and the element that should be loaded:

```html
<Route path="about" element="{<About" />} />
```

The element is set up in the same way as a regular React component function:

```js
// Homepage.jsx
const Homepage = () => {
    return (
        // some stuff
    )
}
export default Homepage;
```

It is convention to create a `pages` directory within the `src` directory to store these elements/pages in.

## Linking Between Pages

A simple anchor tag would trigger a new page request which is not what we want in a single page application. React Router provides us with a `Link` element to create links.

It needs to be imported:

```js
import { Link } from "react-router-dom";
```

It can then be used to create a link. It accepts a `to` prop, which is the route we want it to point at.

```html
<Link to='/store'>Store</Link>
```

## NavLink

React Router also provides us with a `NavLink` element. This is specifically for Navigation lists, and provides additional features such as applying an active class to the currently active page.

It works in the same way as `Link`.

## Nested Routing

A nested route is information in our path in addition to the original route.

For example, if our initial route is `www.mypage.com/app` then a nested route would be `www.mypage.com/app/nested`.

With a nested route in react, we can display specific components within the parent route whilst the rest of the interface remains the same.

To achieve this with React Router we need no special functions ore imports, we simply pass the nested routes in as children to our main routes, in a similar way as passing in children props to a react functional component.

```jsx
<Route path="app" element={<App />}>
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```

We do, however, need a special React Router provided element to render the resulting component in the correct place - we can't use conditionals to display the correct component when routing.

Instead we import and use:

```js
// start of file
import { Outlet } from "react-router-dom";

// where we want the compnent(s) within the layout
<Outlet />;
```

Again, this is similar to using `{children}` as we would to display children props.

## Index Route

If our main route should always display one of the nested routes (for example on initial render, when it has no content of its own) we can use `index routes`. This is achieved simply by adding the `index` key word to the route.

```jsx
<Route path="app" element={<App />}>
  <Route index element={<TodoList />} />
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```
