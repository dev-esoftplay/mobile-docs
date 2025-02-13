# LibWorkloop

This component provides a mechanism for executing tasks sequentially in a "work loop" using a hidden `WebView`. It's designed to manage asynchronous operations and ensure they are executed one after another.

## Installation

This component relies on `react-native-webview` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-webview esoftplay
# or
yarn add react-native-webview esoftplay
```

## Usage

```javascript
import { LibWorkloop } from 'esoftplay/cache/lib/workloop/import';

// ... in your component:
<LibWorkloop /> // Important: Render this component somewhere in your app

// To add a task to the work loop:
LibWorkloop.execNextTix(() => {
  console.log("Task 1 executed");
  // Perform asynchronous operation 1
}, ["param1", "param2"]);

LibWorkloop.execNextTix(() => {
  console.log("Task 2 executed");
  // Perform asynchronous operation 2
});

LibWorkloop.execNextTix(() => {
  console.log("Task 3 executed");
  // Perform asynchronous operation 3
});
```

## API

### `LibWorkloop` (Component)

Renders a hidden `WebView` that acts as the work loop environment.  It's essential to render this component somewhere in your application for the work loop to function.

### `execNextTix(func: Function, params?: any[]): void` (Static Method)

Adds a task to the work loop.

*   `func`: The function to execute.
*   `params`: An optional array of parameters to pass to the function.

### `onMessage(e: any): void` (Static Method)

Handles messages from the WebView (used internally by the work loop).

## Props (`LibWorkloop` Component)

This component does not take any props.

## Functionality

1.  **WebView as Work Loop:** The component uses a hidden `WebView` to execute tasks sequentially.

2.  **Task Queue:** Tasks are stored in the `workloopTasks` array.

3.  **Sequential Execution:** The `execNextTix` function adds tasks to the queue. The `onMessage` handler is triggered by the `WebView` and executes the next task in the queue.

4.  **`InteractionManager`:** The `InteractionManager.runAfterInteractions` function is used to ensure that tasks are executed after any pending interactions have completed. This can be important for preventing conflicts with animations or other UI updates.

5.  **Task Completion:** After a task is executed, it is removed from the queue.  The `workloopHasTask` flag is used to prevent multiple concurrent executions of the work loop.

6.  **WebView Communication:** The `WebView` communicates with the React Native code using `window.ReactNativeWebView.postMessage` and the `onMessage` event handler.

7. **`componentDidMount`:**  The `componentDidMount` lifecycle method is used to perform some initialization tasks after the component is mounted. In this case, it checks if a user is logged in (`UserClass.state().get()`) and if notifications are enabled in the configuration (`esp.config().notification == 1`). If both conditions are met, it loads notification data using `LibNotification.loadData(true)`.

## Important Considerations

*   **`react-native-webview`:** This component relies on `react-native-webview`.
*   **Task Queue:** The `workloopTasks` array manages the tasks to be executed.
*   **Sequential Execution:** The work loop ensures that tasks are executed one after another.
*   **`InteractionManager`:** Using `InteractionManager` is a good practice to prevent conflicts with UI updates.
*   **WebView Communication:** The component uses message passing between the WebView and the React Native code.
*   **Task Parameters:** Tasks can optionally receive parameters.
*   **Hidden WebView:** The `WebView` is hidden (`height: 0, width: 0`).
*   **`originWhitelist`:**  The `originWhitelist` prop is set to `["*"]`.  This is generally not recommended for production apps due to security concerns.  You should restrict the origin whitelist to only the domains that your app needs to access.
* **`InteractionManager.runAfterInteractions`:** This is crucial for ensuring that the work loop tasks don't interfere with UI updates or animations.  By running the tasks after interactions, you avoid potential performance issues and ensure a smoother user experience.
* **Task Queue Management:** The component manages the task queue (`workloopTasks`) correctly, removing completed tasks and preventing concurrent executions.
* **`componentDidMount` Logic:** The logic in `componentDidMount` initializes notifications after the component mounts, but only if a user is logged in and notifications are enabled in the app's configuration. This is a common pattern for setting up background tasks or services that depend on user authentication or app settings.
* **`html` prop:**  The `html` prop is used to provide minimal HTML content to the WebView.  This is necessary for the WebView to function, even though it's hidden.  The provided HTML is very basic, just `<!DOCTYPE html><html></html>`.
* **`ref`:** The component uses a ref (`workloopRef`) to access the `WebView` instance.  This is necessary to inject JavaScript code and communicate with the WebView.
* **`workloopHasTask` flag:** This flag is used to prevent multiple concurrent executions of the work loop.  It ensures that only one task is processed at a time.
* **`injectJavaScript`:**  The component injects JavaScript code into the WebView to start the work loop and to signal the completion of a task.  This is how the React Native code communicates with the JavaScript code running in the WebView.
* **`setTimeout` in WebView:** The JavaScript code injected into the WebView uses `setTimeout` to introduce a small delay between tasks.  This might be necessary to prevent overloading the main thread or to allow for asynchronous operations within the tasks to complete.
* **Security:**  Be very careful about the code you inject into the WebView.  Avoid injecting any untrusted or user-supplied code, as this could create security vulnerabilities.  The `originWhitelist` should be carefully configured to restrict access to only trusted domains.  Avoid using `"*"` in production.
