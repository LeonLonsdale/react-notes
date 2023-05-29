# Component Design

## Design

Components should be designed such that they're flexible, and any re-usable parts are their own component. State should be `lifted` as high as needed - meaning to declare it as high up the component hierarchy as is necessary so that the information is available where the highest level of logic is, and state can't be passed up the chain.

We should attempt to decouple our logic from our presentation when designing our components:

- `presentational` components - do not have state and are primarily focused on displaying something in the UI.
- `logical` components - have state or some related logic, but are not specifically focused on UI.
