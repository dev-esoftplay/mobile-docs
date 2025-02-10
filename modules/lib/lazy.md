# LibLazy

This component defers rendering of its children until after all interactions are complete using `InteractionManager.runAfterInteractions`. This can be useful for improving initial load performance by delaying the rendering of less critical UI elements.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibLazy } from 'esoftplay/cache/lib/lazy/import';

<LibLazy>
  {/* This component will only render AFTER interactions are complete */}
  <MyComponent />
</LibLazy>
```

## Props

| Prop      | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `children` | `any`   | No       | The content to be rendered. This can be any valid React element or component.  If not provided, nothing will be rendered.                                                                                                                                                                                                                                                                                                                                                        |

## Functionality

1. **`useSafeState` Hook:** The `done` state variable, initialized to `false`, is used to track whether the component should render its children.

2. **`useEffect` Hook:** The `useEffect` hook is used to schedule the rendering of the children.

3. **`InteractionManager.runAfterInteractions`:** The `InteractionManager.runAfterInteractions` function is called within the `useEffect` hook. This ensures that the `setDone(true)` call is executed *after* all current interactions (like animations, transitions, or state updates) have completed.

4. **Conditional Rendering:** The component conditionally renders its `children` based on the `done` state.  If `done` is `true`, the `children` are rendered. If `done` is `false` (initially), `null` is rendered.

5. **Cleanup:** The `useEffect` hook returns a cleanup function (`int.cancel()`). This is important to prevent memory leaks. If the component unmounts before the `InteractionManager` callback is executed, the `int.cancel()` call will prevent the `setDone(true)` call from happening, avoiding a state update on an unmounted component.

## Important Considerations

* **`esoftplay` Dependency:** This component uses `useSafeState` from `esoftplay`. Ensure `esoftplay` is correctly installed.
* **Interaction Timing:**  `InteractionManager.runAfterInteractions` is crucial for ensuring that the rendering of the children is deferred until after all interactions are finished.  This prevents potential performance issues or visual glitches during initial load.
* **Use Cases:** This component is most useful for deferring the rendering of non-critical UI elements that are not immediately needed.  For example, you might use it to delay rendering complex components, images, or lists until after the main content of the screen has loaded.
* **Performance:** Deferring the rendering of less critical UI elements can improve the perceived performance of your application, especially on slower devices.
* **Alternative to Suspense:** `LibLazy` provides a similar function to React's Suspense component, but it's specifically designed to work with `InteractionManager` and doesn't require the same setup as Suspense (e.g., using `React.lazy` and `Suspense` boundaries).  `LibLazy` might be easier to use for simple cases where you just want to delay rendering until after interactions.
* **Accessibility:**  Consider accessibility implications. If the content is delayed, ensure that users are aware of this (e.g., using a loading indicator or placeholder).  Make sure that the delayed content is still accessible to users with disabilities once it is rendered.
