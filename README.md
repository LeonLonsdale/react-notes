These are my notes on the use of React for UI development. They have been taken whilst working through various courses on Udemy. They are in complete and should not be considered a replacement for official docs, but if they're helpful to anybody, feel free to star the repo.

# Contents

## React Introduction

- ### Basics

  - [React Intro](./basics.md#intro-to-react)
  - [About JSX](./basics.md#intro-to-jsx)
  - [Basic App Structure](./basics.md#basic-react-structure)

- ### JSX

  - [JSX Rules](./jsx-in-detail.md#jsx-rules)
  - [Fragments](./jsx-in-detail.md#fragments)
  - [Evaluating Expressions](./jsx-in-detail.md#evaluating-js-expressions-in-jsx)
  - [Importing & Exporting Components](./jsx-in-detail.md#importing-and-exporting-components)
  - [Styling Components](./jsx-in-detail.md#styling-components)

- ### How react works

  - [Components, Instances, and Elements](./how-react-works.md#components-instances-and-elements)
    - [Components](./how-react-works.md#components)
    - [Instances](./how-react-works.md#instances)
    - [Elements](./how-react-works.md#elements)
  - [Rendering](./how-react-works.md#rendering)
    - [Process](./how-react-works.md#process)
    - [Triggering a Render](./how-react-works.md#triggering-a-render)
    - [Render Phase](./how-react-works.md#render-phase)
      - [Reconciling](./how-react-works.md#reconsiling)
    - [Commit Phase](./how-react-works.md#commit-phase)
    - [Recap](./how-react-works.md#recap)
  - [Diffing](./how-react-works.md#diffing)
    - [Key Props](./how-react-works.md#key-prop)
  - [Logic in React](./how-react-works.md#logic-in-react)
  - [Pure Components](./how-react-works.md#pure-components)
  - [Events](./how-react-works.md#events)
  - [Component Lifecycle](./how-react-works.md#component-lifecycle)

## Props

- ### Overview

  - [Props Intro](./props.md#props-intro)
  - [Passing Props to Components](./props.md#passing-props-to-components)
  - [Setting Default Prop Values](./props.md#setting-a-default-value-for-a-prop)
  - [Key Prop](./props.md#the-key-prop)

- ### Using Prop Data

  - [Expressions](./using-prop-data.md#expressions)
  - [Conditionals](./using-prop-data.md#conditionals)
  - [Rendering arrays with Map](./using-prop-data.md#render-arrays)

## Component Design

- [Component Design & Decomposition](./component-design.md#component-design)
- [Functions as Props](./component-design.md#passing-functions-as-props)
- [State as Props](./component-design.md#state-as-props)

## Hooks

- ### Overview

  - [What are hooks?](./hooks.md#what-is-a-react-hook)
  - [Built-in Hooks](./hooks.md#built-in-hooks)
  - [Hook Rules](./hooks.md#rules-of-hooks)
  - [Hook call order](./hooks.md#hook-call-order)

- ### State

  - [State Intro](./state.md#intro-to-usestate)
  - [Initialising State](./state.md#initialising-state)
  - [Setting State](./state.md#setting-state)
  - [When does react re-render?](./state.md#when-does-react-re-render)
  - [State Batching & Callbacks](./how-react-works.md#state-update-batching)

- ### Effects

  - [The useEffect Hook](./effects.md#the-useeffect-hook)
  - [Fetching Data](./effects.md#fetching-data-from-an-api)
  - [Cleanup](./effects.md#cleanup)
  - [When are Effects executed?](./effects.md#when-are-effects-executed)
  - [Cancel HTTP Fetch Requests](./effects.md#cancel-a-http-fetch-request-with-cleanup)

- ### Ref

  - [Overview](./refs.md#overview)
  - [Using useRef](./refs.md#using-refs)
  - [Use Cases](./refs.md#use-cases)

- ### Custom Hooks

  - [About Custom Hooks](./hooks.md#about-custom-hooks)

- ### Reducer
  - [Overview](./reducer.md#overview)
  - [Using useReducer](./reducer.md#using-usereducer)
  - [When to use](./reducer.md#deciding-when-to-use-usereducer)
  - [Recap](./reducer.md#recap)

## Context API

- [About Context API](./context-api.md#about-the-context-api)
- [Using Context API](./context-api.md#using-context-api)
- [Custom Providers](./context-api.md#custom-provider)
- [Custom Hook](./context-api.md#custom-hook)

## Suspense API

- [About Suspense API](./suspense.md#suspending-lazy-loaded-routes)

## Forms

- [Controlled Components](./forms.md#controlled-components)
- [htmlFor Property](./forms.md#the-htmlfor-property)
- [Working with multiple inputs](./forms.md#forms-with-multiple-inputs)
- [Form validation](./forms.md#form-validation)

## Events

- [Handling Events](./events.md#handling-events)
- [Event List](./events.md#more-events)
- [Events Object](./events.md#events-object)

# React Router

- [Setup](./react-router.md#setup)
- [Basic Use](./react-router.md#basic-use)
- [Linking between Pages](./react-router.md#linking-between-pages)
- [NavLink](./react-router.md#navlink)
- [Nested Routing](./react-router.md#nested-routing)
- [Index Route](./react-router.md#index-route)
- [Using URL for State](./react-router.md#storing-state-in-the-url)
- [Programatic Navigation](./react-router.md#programatic-navigation)
  - [useNavigate Hook](./react-router.md#imperative-with-the-usenavigate-hook)
  - [Navigate component](./react-router.md#declarative-with-the-navigate--component)

# Performance Optimisation

- [Memoisation](./memoisation.md)
  - [What is memoisation?](./memoisation.md#what-is-memoisation)
  - [memo](./memoisation.md#memo)
  - [useMemo & useCallback](./memoisation.md#usememo--usecallback)
  - [Lazy Loading Components](./codesplitting.md#overview)

# Redux

- [When should I use Redux?](./redux.md#when-should-we-use-redux)
- [How does Redux work?](./redux.md#how-does-redux-work)
- [Using Oldschool Redux](./redux.md#using-oldschool-redux)
  - [InitialState](./redux.md#initial-state)
  - [Reducer Functions](./redux.md#reducer-functions)
  - [Create a Store](./redux.md#create-a-store)
  - [Dispatch](./redux.md#dispatch)
  - [Action Creators](./redux.md#action-creators)
  - [Actions](./redux.md#actions)
