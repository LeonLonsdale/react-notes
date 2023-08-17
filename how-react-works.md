# Components, Instances, and Elements

## Components

- Component describe a piece of the overall user interface.
- A javascript function that returns React elements (element tree), usually written as JSX.
- A blueprint / template that can be reused in some cases.

## Instances

- An instance of a component is created each time we use the component.
- An instance therefore is a virtual manifestation of the component.
- Each instance can therefore hold its' own State and Props, as well as its own lifecycle.

## Elements

- Each instance returns one or more React Elements
- JSX is converted to React.createElement function calls
- As these functions are called, React Elements are the result.
- The element is an immunitable javascript object that react keeps in memory.
- The object contains all the information needed to create DOM elements for the component instance.
- So react Elements are converted to DOM elements which are displayed on-screen by the browser.

## Misc

- Console logging a component shows us the instance. `console.log(<Component />);`.

# Rendering

## Process

1. Rendering is triggered by state updates
2. Render Phase - In the Render phase, React calls component functions and figures out how the DOM should be updated.[1]
3. Commit Phase - React writes to the DOM, updating, inserting, and deleting elements.
4. Browser Paint

[1] In react, rendering is NOT updating the DOM or displaying elements on screen. Rendering is internal within React and does not produce visual changes.

## Triggering a Render

- 2 ways a render can be triggered:
  - Initial Render (first load) of the application
  - State is updated in one or more component instances (re-render)
- Render process is triggered for the entire application, but the entire DOM is not updated as a result.
  - the process is not triggered immediately, but for when the JS engine has time to do it.
  - setState calls are batched into event handlers.

## Render Phase

- React checks through the component tree for component instances that triggered the re-render, and call the corresponding component functions
- This creates updated react elements.
- All the react elements together create the new `virtual DOM`
- The virual DOM is simply a tree of all the react elements created from all instances in the component tree.
- When a component is re-rendered, all children of that component, and their children (etc.) are also rerendered.
- The new virtual DOM is then reconciled using Reacts reconciler `Fiber`, which produces a new Fiber tree.
- The Fiber tree is used to write the new DOM.
- The Render Phase is handled by `React`

### Reconsiling

React decides which DOM elements actually need to be updated, deleted, or inserted in order to reflect the latest state changes. It does this to avoid having to reload the entire DOM on every update, which is slow and demanding.

- During the first load, Fiber takes the entire React Element Tree (Virtual DOM) and builds the Fiber Tree
- This is an internal tree where for each component instance and DOM element in the App there is one Fiber.
- Fibers are not recreated on every render, so the Fiber tree is a mutable data structure. Once it's been created, it is mutated over and over again.
- This allows the Fiber tree to keep track of state, props, side effects, used hooks etc.
- Each Fiber contains a queue of work such as updating state, etc. Fibers are also known as `a unit of work`.
- Fibers are structured in a `linked list` format.
- Work can be performed by Fibers `asynchronously`.

Reconciliation works by working through the entire tree step by stype, analysing what needs to change between the current and updated Fiber trees, based on the new, Virtual DOM. This process of comparing elements step-by-step is called `diffing`.

The resulting DOM mutations are kept in a `list of effects` which will be used in the commit phase to mutate the DOM.

## Commit Phase

- The `list of effect` is read and applies them one by one to the existing DOM tree.
- This is synchronous, meaning the DOM is updated in one go. This can't be interrupted. This is necessary so that the DOM doesn't display partial results.
- Once done, the workInProgress fiber tree becomes the current fiber tree for the next render cycle.
- The browser then re-paints the screen with the updated DOM.
- The Commit Phase is handled by `ReactDOM` and the Painting is handled by the browser.

## Recap

1. The **Rendering Phase** is triggered either by initial load or a state change - not immediately, but scheduled for when the JS engine has time. setState calls in Event Handler Functions are also batched.
2. The rendering phase calls component functions for instances that need to be re-rendered (including their children). This creates new react elements which are used to create a new **Virtual DOM**.
3. The new Virtual DOM is then compared with the existing **Fiber Tree** in a process called **Reconciliation**.
4. Reconciliation uses a diffing algorithm to compare the new Virtual DOM with the Fiber Tree to identify changes.
5. Each react and DOM element has a **Fiber** in the Fiber Tree which holds the state, props, and a queue of work. Identified DOM updates for each element are stored in the **queue of work** during reconciliation.
6. This process is asynchronous and can be paused, resumed, prioritised, and tasks can be split.
7. The output of reconciliation is an updated Fiber Tree and a **List of Effects**.
8. The list of effects is used in the **Commit Phase** by **ReactDOM** which looks at each effect one at a time, and applying the changes to the DOM`.
9. This is done synchronously so cannot be interrupted, so that a consistent UI is possible.
10. Once the this process completes, and the browser identifies the DOM changes, the browser re-paints the UI.

# Diffing

_Assumptions_

1. **Two elements of different types will produce different trees.**

   _Same Position, Different Element_

   So, DOM or React elements in the same position are different, React will assume that the entire sub-tree is no longer valid, and will remove them, including their state and props from the DOM.

   _Same Position, Same Element_

   Elements will be kept, including children and state. Changes to props will result in mutation of DOM Element attributes.

   The key prop can be used in instances where this is not thte behaviour we're hoping for, and we want a new component instance with new state.

2. Elements with a stable key prop stay the same across renders.

## Key Prop

- A special prop that we can use to tell the diffing algorithm that an element is unique.
- Allows React to distinguish between multiple instances of the same component type.
- When a key stays the same across renders, the element remains in the DOM, even if the position in the tree changes.
- When a key changes between renders, the element will be destroyed and a new one created, even if the poision in the tree is the same as before.

### Keys in Lists

In a list with no keys, if a new item is added to the start of the list, the original items in the list will be the same but change position. With no stable key, these elements will be destroyed and rerendered by React. With a stable key, however, react knows that it doesn't have to rerender them, and keeps them in teh DOM.

### Key prop to reset state.

In some cases, the props provided to a component may change, but the component itself remains the same and in the same position. If the state is not changing then this will not be rerendered when the prop content changes - when the same element is in the same position, the state is preserved. We can therefore use a Key to let react know that something changed, so the state is reset.

# Logic in React

1. Render Logic
2. Event Handler Functions

## Render Logic

- Code that lives in the top level of the componenet function
- Participates in describing how the component view looks.
- Executed every time the component renders

Examples: state declarations, variable declarations, the return block.

## Event Handler Functions

- Pieces of code executed as a consequence of the event the handler is listening to.
- Contain code that actually does something like update state, perform a HTTP request, read an input field, or navigate to a new page.

Examples: Functions called onChange, onClick, onSubmit etc.,

# Pure Components

Some definitions first:

- Side Effect: dependancy on modofication of any data outside the function scope. Example: mutating external variables, http requests, writing to DOM.

- Pure Functions: Functions without side effects. The output is always the same given the same input.

**Rules for Pure Components**

1. Components must be pure in terms of render logic: Given teh same props, the component instance should always return the same JSX.
2. **Render logic** must not produce any side effects.
   - So no network requests / API calls
   - No timers
   - No direct work with the DOM
   - No mutation of objects or variables outside of the function scope, including props.
   - Do not update state (or refs).
3. Do side effects in event handler functions. There is a hook for registering side effects (useEffect)

# State Update Batching

Essentially, the Render & Commit doesn't start until all the states affected by the Event Handler have been completed. That is to say, if an Event Handler changes 3 states, each state is not updated one at a time and thus triggering 3 Render & Commits. They'll all be updated before triggering a single Render & Commit.

This can have some unexpects consequences. Imagine:

```js
const reset = () => {
  setName('');
  console.log(name);
  setEmail('');
  setPassword('');
};
```

In the example above, since state is stored in the Fiber Tree during rendering, and at the time of the `console.log` call, the re-render has not yet taken place, the `name` state will still be the previous state and not the new state. This is called `stale state`. This is true even if a single state is being updated.

This is the reason callbacks are used when setting state based on the current state:

```js
setIsTrue((cur) => !cur);
```

If automatic batches is an issue at any point, the state can be wrapped in `ReactDOM.flushSync()`.

# Events

## Event Propagation and Delegation in the DOM

When an event happens, such as a click, it takes place at the root of the document and, during the `capturing phase` the event object travels down through each element until it reaches the `event target element`. We can then handle the event by putting a handler function on that target element. The event object then travels all the way back up to the root of the document in what is known as the `bubbling phase`.

1. During capturing and bubbling phases, the event object goes through every single parent and child element one by one between the root and the target element.
2. By default Event Handlers listen to events during the bubbling phase. This means that if we have a click event on both a button and its parent, clicking the button will trigger both event handlers. This can be stopped using the `stopPropagation()` method.

This allows event delegation - put the event handler on a parent element, and check what the target was there. This prevents the need to have multiple copies of the same event handler on multiple child elements.

React actually bundles all onClick (and onSubit, onChange etc) events into a single onClick event handler that handles them all, on the root DOM container node of the fiber tree. This means that React performs event delegation by default.
