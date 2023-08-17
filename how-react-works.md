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

**Triggering**

- The process starts with a trigger. A trigger can be:
  - The first load of the app; or
  - A state change.
- The trigger just triggers the Render Phase

**Render Phase**

- The Render Phase does not produce any visual output.
- The Render Phase calls the component functions for component instances that need a re-render, to create new React Elements.
  - This, in turn, calls all child component functions whether they need a re-render or not.
- The new React Elements are used to create a Virtual DOM (A tree of react elements).

**Reconciliation - Part of Render Phase**

- The new Virtual DOM is then compared with the Existing Fiber Tree (which is a representation of the Virtual DOM before the re-render was called) to create an Updated Fiber tree which represents the current state of the elements. This process is called Reconciliation.
- Reconciliation uses a diffing algorythm to identify the differences between the old and new Virtual DOMs.
  - There is a Fiber in the Fiber tree for each react element and DOM element.
    - The fiber holds the component state, props, and a queue of work.
    - After diffing, the queue of works will contain the DOM updates that are needed for that element.
- The Output of Reconciliation is an updated Fiber Tree and a list of effects (list of DOM updates).
- This is all done Asynchronously and can be split, prioritised, paused and resumed.

**Commit Phase**

- The list of effects is them used in the Commit Phase, processed by ReactDOM and appled to the DOM one at a time.
  - This is syncrhonous, and cannot be interrupted, to prevent partial UI display.

**Browser Painting**

- When the Browser identifies the DOM update, it initiates a Brwoser Paint phase where the changes are updated onscreen.

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
