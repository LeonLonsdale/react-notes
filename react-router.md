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

[Back to Contents](./README.md) - [Back to Top](#)

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

[Back to Contents](./README.md) - [Back to Top](#)

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

[Back to Contents](./README.md) - [Back to Top](#)

## NavLink

React Router also provides us with a `NavLink` element. This is specifically for Navigation lists, and provides additional features such as applying an active class to the currently active page.

It works in the same way as `Link`.

[Back to Contents](./README.md) - [Back to Top](#)

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

[Back to Contents](./README.md) - [Back to Top](#)

## Index Route

If our main route should always display one of the nested routes (for example on initial render, when it has no content of its own) we can use `index routes`. This is achieved simply by adding the `index` key word to the route.

```jsx
<Route path="app" element={<App />}>
  <Route index element={<TodoList />} />
  <Route path="nested" element={<Nested />} />
  <Route path="todolist" element={<TodoList />} />
</Route>
```

[Back to Contents](./README.md) - [Back to Top](#)

## Storing State in the URL

The URL is a great place to store UI state and an alternative to useState in some situations.

Some benfits include:

- You can share the URL with others, and they will render the same view. For exampel if you've selected the size and colour of a T-Shirt and want to share it with a friend.
- You can bookmark the URL to return to it later without losing progress.
- The state is available to all components without having to use prop drilling.
- The data can easily be passed to the next page.

We can store state in the URL using `params` and the `query string`

```
www.myapp.com/app/products/1381239216391?size=l&colour=black

param = 1381239216391 // would be replaced with an actual username.
query string = ?size=l&colour=black
```

We would need links set up that include the details we want to store/pass along:

```html
<Link to=`${productId}?size=${size}&colour=${colour}`> ... </Link>
```

### Params

To achieve this, we first give our params an alias within a route:

```html
<Route path="products/:id" element="{...}" />
<!-- :username here is the alias -->
```

We can then, in our component, access this alias by using the `useParams()` hook provided by React Router

```js
import { useParams } from "react-reouter-dom";

const User = () => {
  const params = useParams();
  // params will be an object of all the params
  // there can be multiple under different alias's in the route
  // the object properties will take the name of the alias's provided
  // { id: 1381239216391 }
  // so this could be destructured from the start
  // const { id } = useParams();
};
```

### Query String

React Router also provides us with a hook for the search query. This works similar to useState in the syntax. It gives us a variable and a function.

The values aren not immediately accessible, so the variable we receive back comes with a get property that we use.

```js
import useSearchParams from "react-router-dom";

const User = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const size = searchParams.get("size");
  const colour = searchParams.get("colour");
};
```

We can then change the search params with the function we now have available. For example, if the user wanted to take a look at a white T shirt instead of a black T shirt:

```js
onClick={() => setSearchParams({ size: 'l', colour: 'white' })};
```

[Back to Contents](./README.md) - [Back to Top](#)

## Programatic Navigation

Sometimes we want certain actions to take the user to a specific route, but those actions may not necessarily involve clicking a link or a button - for exaple: form submission, clicking a location on a map.

React Router provides us with `useNavigate()` for just this purpose. This returns a navigate function into which we pass the url parameter as an arguament.

```js
import { useNavigate } from "react-router-dom";

const MyComponent = () => {
  const navigate = useNavigate();
  return <div onClick={() => navigate("param")}>// ...</div>;
};
```

Additionally, we may occasionally want to include back or forward buttons. This is simply achieved by passing in the number of steps forward or backward we want the button to take us. Negative numbers for back, and positive numbers for forward.

```js
navigate(-1); // back 1
navigate(1); // forward 1
```
