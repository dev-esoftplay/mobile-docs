# LibGlobal

This component manages a global "ready" state and conditionally renders its children based on that state. It's useful for displaying a loading view or waiting for certain initialization tasks to complete before showing the main content of your application.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibGlobal } from 'esoftplay/cache/lib/global/import';

// In your app's main component or a high-level component:
<LibGlobal waitForFinish={true} waitingView={<MyCustomLoadingView />}>
  {/* Your main application content */}
  <AppNavigator />
</LibGlobal>

// Setting the global "ready" state (e.g., after initialization):
globalReady.set(true); // Assuming globalReady is accessible (e.g. from a context)

// Or, if you need to perform asynchronous initialization before setting globalReady to true.
// For example, fetching configuration from server.

// In your initialization logic:
someAsyncFunction().then(() => {
  globalReady.set(true);
});

```

## Props

| Prop          | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `waitForFinish` | `boolean` | Yes      | If `true`, the component will wait for the global "ready" state to become `true` before rendering its children. If `false`, the children are always rendered.                                                                                                                                                                                                                                            |
| `waitingView` | `any`   | No       | Optional. A React element to render while `waitForFinish` is `true` and the global "ready" state is `false`. If not provided, a default loading indicator (`LibLoading` from `esoftplay`) is displayed.                                                                                                                                                                                                                                                                                                                            |
| `children`    | `any`   | No       | The content to be rendered when the global "ready" state is `true` (or if `waitForFinish` is `false`).                                                                                                                                                                                                                                                                                                                             |

## Global State

The component uses a global state (from `esoftplay`) to manage the "ready" state.  This state is persisted using `persistKey: "__globalReady"`.  The `onFinish` callback is called when the state is initially set to `true`.

## Functionality

1. **Global "Ready" State:** The component uses `useGlobalState` to create and manage the `globalReady` state. This state is persisted.

2. **Conditional Rendering:**
    * If `waitForFinish` is `true`:
        * If the global "ready" state is `true`, the `children` are rendered.
        * If the global "ready" state is `false`, the `waitingView` is rendered (or `LibLoading` if `waitingView` is not provided).
    * If `waitForFinish` is `false`, the `children` are always rendered, regardless of the global "ready" state.

3. **`onFinish` Callback:** The `onFinish` callback associated with the `globalReady` state is called when the state is initially set to `true`.  This is only called the *very first time* the state becomes true after the app starts.

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library and its global state management. Ensure `esoftplay` is correctly set up in your project.
* **Global State:**  Using a global state is convenient for this use case, but be aware of the implications of global state management in larger applications.  Consider using a more structured state management solution (like Redux or Context API) if your application's state becomes more complex.
* **Persistence:** The `persistKey` option ensures that the "ready" state is persisted across app restarts.
* **`waitForFinish` Prop:** The `waitForFinish` prop controls the conditional rendering behavior.  Set it to `true` if you want to wait for initialization to complete.
* **`waitingView` Prop:** The `waitingView` prop allows you to customize the loading view.
* **`onFinish` Callback:** The `onFinish` callback is only executed the very first time the global state becomes true. It's not called on subsequent state changes.
* **Initialization Logic:** You'll need to set the `globalReady` state to `true` when your application's initialization tasks are complete.  This is typically done in a high-level component or in your app's initialization logic.
* **Accessibility:** Consider accessibility implications.  If the content changes based on the global "ready" state, ensure that users are notified of these changes (e.g., using ARIA attributes or announcements).  Provide appropriate feedback to users while the `waitingView` is displayed.
