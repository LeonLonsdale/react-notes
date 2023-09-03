# Props

## Props Intro

Props allow us to write configurable components. The term `prop` is short for properties, and refers to the object properties - because all elements created in react are objects. Therefore, passing data into a component is done by creating properties and values for that object.

Components use these props to communicate with each other - every parent component can pass some information to its child components by giving them props.

Using props we can pass any data to the component from HTML Attributes to javascript data

[Back to Contents](./README.md) - [Back to Top](#)

## Passing Props to Components

We pass props to a component in a similar way to assigning attributes to HTML:

```
key=value
```

In general, we wrap value in `""` to pass in strings, and `{}` to pass in numbers or javascript to be evaluated.

We can pass as many props as we like to the component. The important thing to remember is that the component receives these props in a single object of key-value pairs.

```jsx
export default function Profile () {
  return (
    <Avatar imageId={'asd2313adad'} size={100} />
    <PersonData person={{name: 'Leon L', age: 39, email: 'some@email.addr'}} />
  )
}
```

Here the props `imageId` and `size` are passed to the Avatar component, while the prop `person` is passed to the PersonData component.

The receiving component will receive the props in an object format:

```js
// Avatar will receive
{ imageId: 'asd2313adad', size: 100 }

// PersonData will receive
{ person: {name: 'Leon L', age: 39, email: 'some@email.addr'}}
```

Within the components, we can then deconstruct these objects:

```jsx
// Avatar component
export default function Avatar({imageId, size}) {
  return (
    <img src={imageId} width={size} height={size} />
  )
};

// PersonData component
export default function PersonData({person}) {
  return (
    <div>
      <p>Name: {person.name}</p>
      <p>Age: {person.age}</p>
      <p>Email: {person.email}</p>
    </div>
  )
}
```

If one of our props is an object that is defined outside of the prop call, we can simply pass the prop through and spread it at the same time.

```jsx
export default function Profile () {
  const person = { name: 'Leon L', age: 39, email: 'some@email.addr' }
  return (
    // Avatar
    <PersonData person={...person} />
  )
}

/// PersonData component
export default function PersonData({name, age, email}) {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  )
}
```

[Back to Contents](./README.md) - [Back to Top](#)

## Setting a Default Value for a Prop

We do this in the same way as we would in JavaScript - we set the default value when deconstructing the prop object.

```jsx
export default function PersonData({
  name = "Joe Bloggs",
  age = 30,
  email = "none provided",
}) {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}
```

[Back to Contents](./README.md) - [Back to Top](#)

## The Key Prop

Whenever you iterate over data and render a list in React, React needs some way to track the individual list items rendered so that it can track whether re-rendering is required later. To help React do this, we need to pass in a `key prop`. Key should be some kind of unique identifier and is passed in the same way as other props, but on the parent element of the response.

```jsx
// iterate over an array of items to produce a list
<li key={item.id}>
  <p>{item.name}</p>
  <p>{item.description}</p>
  <p>{item.price}</p>
</li>
```

[Back to Contents](./README.md) - [Back to Top](#)
