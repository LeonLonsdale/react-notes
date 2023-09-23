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

## Custom Provider

We can go a step further and refactor our code, creating a custom provider in a separate file.

This is achieved by movine all applicable state, event handlers, and effects, as well as context declarations into a new component.

```js
// new provider file TodosProvider.js
import { useState, useContext } from 'react';

const TodosContext = useContext();

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
    { children }
    </Context.Provider>
  )
}

export { TodosContext, TodosProvider }

// Original file now looks like:

import { useContext, useState } from 'react';
import { TodosContext, TodosProvider } from 'TodosProvider.js';

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

## Custom Hook

Currently, wherever the context is to be used, we must write `const { x, y, z} = useContext(TodosContext)`. In a large app, having to type this over and over again can be troublesome. Additionally, it's not always immediately clear which context it refers to.

To overcome this, we can create our own custom hook.

Our custom hook, should be kept in the same file as our context.

```js
// TodosProvider.js

import { useState, useContext } from "react";

const Context = useContext();

function TodosProvider({ children }) {
  // states and handlers

  return (
    <Context.Provider
      value={{
        todos,
        onAddTodo: handleAddTodo,
        onDeleteTodo: handleDeleteTodo,
      }}
    >
      {children}
    </Context.Provider>
  );
}

function useTodos() {
  const context = useContext(TodosContext);
  return context;
}

// export useTodos now instead of TodosContext
export { useTodos, TodosProvider };
```

We can now use

```js
const { x, y, z } = useTodos();
```

instead of

```js
const { x, y, z } = useContext(TodosContext);
```
