# Component Design

## Design

Components should be designed such that they're flexible, and any re-usable parts are their own component. State should be `lifted` as high as needed - meaning to declare it as high up the component hierarchy as is necessary so that the information is available where the highest level of logic is, and state can't be passed up the chain.

We should attempt to decouple our logic from our presentation when designing our components:

- `presentational` components - do not have state and are primarily focused on displaying something in the UI.
- `logical` components - have state or some related logic, but are not specifically focused on UI.

Additionally, aim to minimise the logic in components - where possible write utility / reusable functions.

[Back to Contents](./README.md) - [Back to Top](#)

## Passing Functions as Props

In JavaScript, functions are `first-class objects`, so we can pass them around. Passing functions as props allow us to make our app more configurable.

We pass functions as props in the same way as any other prop, usually at the parent level so that the function can be passed down through children, while retaining access to them in the parent.

Imagine a ToDo app in which we have a list of ToDo's and we want a button on each ToDo that allows the user to delete it. The component structure may look like this:

```
App > Main > ToDoList > ToDo
```

Here, our state could be either at Main or ToDoList, depending on how we want to re-render the display. In either case, our on click event is going to be on ToDo. So we want our click event on ToDo to update the State on ToDoList. Any click handler function written on ToDo is not going to have access to our state setter. In this instance, then we could pass a function as a prop.

```js
// ToDoList component (parent)

const ToDos = [
  // array of todos
];

const ToDoList = () => {
  // create our state
  const [todos, setTodos] = useState(ToDos);
  // create our event handler function
  const handleDelete = (id) => {
    setTodos((oldTodos) => oldTodos.filter((t) => t.id !== id));
  };
  // return a todo component for each item in the state
  // pass in a reference to the handle delete function as a prop.
  return (
    <div className="ToDos">
      {todos.map((t) => (
        <li>
          <Todo key={t.id} todo={t} handleDelete={handleDelete} />
        </li>
      ))}
    </div>
  );
};

export default ToDoList;

// ToDo component (child)

const ToDo = ({ todo, handleDelete }) => {
  return (
    // todo design
    <button onClick={() => handleDelete(todo.id)}>Delete Todo</button>
  );
};
```

Now, with this example, the `handleDelete` function can be called onclick on the child element and pass in the ID. This then has access to update the state on the parent component.

[Back to Contents](./README.md) - [Back to Top](#)

## State as Props

Sometimes we want a particular component to re-render on state change, so this is the component in which we want to declare our state. However, it's not always the component that is actually going to render the state data on screen. To achieve this goal, we declare our state and pass it as a proper down via child components until the state reaches the component that will display it.

Note, however, that using this method means you cannot update the state from the rendering child component. State updates will still need to take place either in the parent component in which the state is declared, OR, in a child component that a state-updating function has been passed to.

[Back to Contents](./README.md) - [Back to Top](#)
