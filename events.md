# Events

## Handling Events

To handle events in React, we first define an event handler:

```jsx
export default function EventHandler() {
  const handleClick = (e) => {
    console.log('You clicked me!', {event});
  }
  return ()
}
```

With the event handler built, we simply tell the event to use that handler. We do this differently the the typical way with `addEventListener`. In JSX, we pass the handler through as a prop to the event within the HTML element:

```jsx
export default function EventHandler() {
  const handleClick = (e) => {
    console.log('You clicked me!', {event});
  };
  return <button onClick={handleClick}>Click Me</button>;
}
```

## More Events

### Clipboard

- onCopy
- onCut
- onPaste

### Composition

- onCompositionEnd
- onCompositionStart
- onCompositionUpdate

### Keyboard Events

- onKeyUp
- onKeyPress
- onKeyDOwn

### Focus Events

- onFocus
- onBlur

### Form Events

- onChange
- onInput
- onInvalid
- onReset
- onSubmit

### Mouse Events

- onClick
- onContextMenu
- onDoubleClick
- onDrag
- onDragEnd
- onDragEnter
- onDragExit
- onDragLeave
- onDragOver
- onDragStart
- onDrop
- onMouseDown
- onMouseEnter
- onMouseLeave
- onMouseMove
- onMouseOut
- onMouseOver
- onMouseUp

### Pointer Event

- onPounterDown
- onPointerMove
- onPointerUp
- onPointerCancel
- onGotPointerCapture
- onLostPointerCapture
- onPointerEnter
- onPointerLeave

## Events Object

As with regular JavaScript event handling, the event object is passed to the event handler function. We can make use of this event object as normal - such as to use `preventDefault`.

```jsx
export default function EventHandler() {
  const handleClick = (event) => {
    event.preventDefault();
    console.log('You clicked me!', {event});
  }
  return ()
}
```
