# Hooks

## What is a React Hook?

- Special functions that allow us to hook into reacts internals
- Basically API's that give us access to internal React functionality. Such as:
  - Creating and accessing state
  - Registering side effects in the Fiber tree.
- All react hooks start with the word `use`
- We can create our own custom hooks.

## Built-in Hooks

- useState
- useEffect
- useReducer
- useContext
- useRef
- useCallback
- useMemo
- useTransition
- useDeferredValue
- useLayoutEffect
- useDebugValue
- useImperativeHandle
- useId

## Rules of Hooks

1. Hooks can only be called at the top level.

   - That means not inside conditionals, loops, nexted functions, or after early returns.
   - This is because hooks are always called in the same order.

2. Hooks can only be called from React functions.
   - Only call hooks inside a function component or a custom hook.

## Hook call order

- On initial render, React creates a Fiber tree.
- Each Fiber contains the props, state, queue of work, and `a linked list of hooks`.
- Within the linked list, each hook contains a reference to the next hook.
- If a hook was conditionally called on the initial render, and therefore, included in the linked list, but the conditions are not met on subsequent renders, the linked list breaks.
- It is done this way because Fibers are not recreated on every render, and so the list is also not recreated.
