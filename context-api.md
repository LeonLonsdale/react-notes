# Context API

## About the Context API

Context API is a solution to prop drilling. It allows deeply nested components to access state that a context shares, without having to pass the props down through the tree.

It allows the broadcasting of global state to the entire app.

The first part of the context API is the `provider` - a react component that gives all child components access to a `value`. The `provider` can be put anywhere within the component tree, but it is most common for this to be at the top level.

The `value` is the data that we want the provider to broadcast. It may contain one or more state variables, as well as setter functions.

Finally, we have the `consumers` which are the components that read the value.

`Consumers` are automatically re-rendered whenever the `value` is updated.

[Back to Contents](./README.md) - [Back to Top](#)

## Using Context API

### 1. Creating the Context

This returns a component so it is standard practice to use a capitalised name.

```js
import createContext from "react";

const Context = createContext();
```

A context should usually be created per-state that requires one. Multiple different states should not be passed as a value in a single context.

[Back to Contents](./README.md) - [Back to Top](#)

### 2. Creating the Provider

The Provider should be the parent component of the child components that will be consumers.

```jsx
return (
  <Context.Provider>
    <ChildComponents />
    <ChildComponents />
  </Context.Provider>
);
```

[Back to Contents](./README.md) - [Back to Top](#)

### 3. Pass the value to the provider

The value is passed in to the `Context.Provider` component as a prop, and would usually be an object of the data.

```jsx
const [todos, setTodos] = useState();
const [todo, setTodo] = useState();
const [searchResults, setSearchResults] = useState();

const TodoContext = createContext();

const handleAddTodo = () => { ... }
const handleDeleteTodo = () => { ... }

return (
    <TodosContext.Provider value={{
        todos,
        onAddTodo: handleAddTodo,
        onDeleteTodo: handleDeleteTodo,
    }}>
        <TodoList />
        <NewTodoForm />
    </TodosContext.Provider>
    );
```

[Back to Contents](./README.md) - [Back to Top](#)

### 4. Consume value

To consume the value, we use the `useContext` hook from react and pass the context into it. We do this in the top level of the child component that needs access to the value.

This returns the value object, which can be immediately destructured.

```jsx
import useContext from 'react';

const TodoList = () => {
    const { todos, onAddTodo, onDeleteTodo } = useContext(TodosContext)

    return ()
}

```

[Back to Contents](./README.md) - [Back to Top](#)

## Custom Provider & Hook

We can go a step further and refactor our code, creating a custom provider in a separate file.

This is achieved by movine all applicable state, event handlers, and effects, as well as context declarations into a new component.

```js
// new provider file TodosProvider.js
import { useState, useContext } from 'react';

const Context = useContext();

function TodosProvider ({ children }) {
  const [todos, setTodos] = useState();
  const handleAddTodo = () => { ... }
  const handleDeleteTodo = () => { ... }

  return (
    <Context.Provider value={{
      todos,
      onAddTodo: handleAddTodo,
      onDeleteTodo: handleDeleteTodo
    }}>
    { chilren }
    </Context.Provider>
  )
}

export { Context, TodosProvider }

// Original file now looks like:

import { useContext, useState } from 'react';
import { Context, TodosProvider } from 'TodosProvider.js';

const App = () => {
  const [todo, setTodo] = useState();
  const [searchResults, setSearchResults] = useState();

  return (
    <TodosProvider>
      <TodoList />
      <NewTodoForm />
    </TodosProvider>
  )
}

```
