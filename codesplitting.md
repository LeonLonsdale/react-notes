# Code Splitting & Bundle Size

## Overview

- The `bundle` is the JavaScript file that is sent to the client by the server. It contains all the application code.
- Downloading the bundle loads the entire app at once, turning it into a Single Page Application.
- The `Bundle Size` is the amount of JavaScript the user has to download to start the application.
- Overall, optimising the bundle size to minimise the required download time, is one of the most important actions to take when optimising application performance.
- `Code Splitting` is splitting the bundle into multple parts that can be downloaded over time, as and when needed (`lazy loading`).

## Lazy Loading Components

React provides us with a function specifically for lazy loading:

```js
import { lazy } from "react";
```

It is typical to use this `lazy()` function at the route level to split pages into their own js files and only load them when they are clicked. To do this, we replace the standard component import method.

The `lazy()` function accepts a callback as an arguament. We use this callback to import our file.

```js
// replace:
import Homepage from "./pages/Homepage";

//with
const Homepage = lazy(() => import("./pages/Homepage"));
```

This may, however, cause some unwanted effects for the user as they wait for the file to download and be executed.

For this, we should make use of the `Suspense API` to display some indication that something is happening to the user, such as a loading spinner.

See [Suspending Lazy-Loaded Routes](./suspense.md#suspending-lazy-loaded-routes)

[Back to Contents](./README.md) - [Back to Top](#)
