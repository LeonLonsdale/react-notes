# Suspense API

This API allows specific components to 'suspend' or wait for something else to happen. For example, suspending a page while it loads when using lazy loading.

```js
import { Suspense } from "react";
```

## Suspending Lazy Loaded Routes

To suspend lazy-loaded routes, we wrap our routes in the suspense component and provide it with the `fallback prop`. The fallback prop will be our loading component.

```html
<Suspense fallback={<Spinner />}>
    <Routes>
        <Route index element={<Homepage />} />
        <Route path='product' element={<Product />} />
    </Routes>
</Suspense>
```
