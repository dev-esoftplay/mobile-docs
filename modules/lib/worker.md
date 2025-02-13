# LibWorker

This component provides a way to use a JavaScript worker within a React Native application using a `WebView`. It allows you to execute JavaScript functions in the worker thread and receive results back in the main thread. This can be useful for offloading computationally intensive tasks or performing operations that might otherwise block the main thread.

## Installation

This component relies on `react-native-webview` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-webview esoftplay
# or
yarn add react-native-webview esoftplay
```

## Usage

```javascript
import { LibWorker, LibWorkerProperty } from 'esoftplay/cache/lib/worker/import';

// ... in your component:
<LibWorker /> // Important: Render the component somewhere in your app

// To use a worker function:
const myFunction = LibWorkerProperty.createTask("myWorkerFunction", "param1", "param2");

myFunction((result) => {
  console.log("Result from worker:", result);
});

// Define your worker functions in a separate file (e.g., out.js):
// export async function myWorkerFunction(param1, param2) {
//   // Perform some calculations or operations
//   return `Result: ${param1} ${param2}`;
// }

```

## API

### `LibWorker` (Component)

Renders a hidden `WebView` that acts as the JavaScript worker environment.  It's crucial to render this component somewhere in your application for the worker to function.

### `createTask(functionName: string, ...params: string[]): (res: (data: string) => void) => void`

Creates a function that can be used to execute a task in the worker.

*   `functionName`: The name of the function to execute in the worker.
*   `params`: An array of parameters to pass to the worker function.
*   Returns a function that takes a callback function as an argument.  This callback will be called with the result from the worker.

## Props (`LibWorker` Component)

This component does not take any props.

## Functionality

1.  **WebView as Worker:** The component uses a hidden `WebView` as the execution environment for the JavaScript worker.

2.  **Task Creation:** The `createTask` function creates a closure that, when called, triggers a message to be sent to the worker.

3.  **Message Passing:** Messages are passed between the main thread and the worker using `window.ReactNativeWebView.postMessage` and the `onMessage` event handler.

4.  **Worker Function Definition:** The worker functions are defined in a separate JavaScript file (e.g., `out.js`) that is injected into the `WebView`.

5.  **Result Handling:** The `onMessage` handler receives the results from the worker and calls the appropriate callback function.

6.  **Task Management:** The `tasks` map keeps track of pending tasks and their associated callbacks.

7.  **Ready State:** The `isReady` ref tracks whether the `WebView` is ready to receive tasks.

8.  **`useGlobalSubscriber`:** The `useGlobalSubscriber` hook from `esoftplay` is used to manage and trigger worker tasks.

9. **`createTimeout`:** The `createTimeout` function is used for retry mechanism if worker is not ready.

## Important Considerations

*   **`react-native-webview`:** This component relies on `react-native-webview` for the worker functionality.
*   **Worker File (`out.js`):** You need to create a separate JavaScript file (e.g., `out.js`) that defines the functions you want to execute in the worker. This file should be imported or required by the `LibWorker` component.
*   **Message Format:** The messages passed between the main thread and the worker are JSON strings.  Make sure you stringify and parse them correctly.
*   **Error Handling:** Implement error handling to gracefully handle cases where the worker function throws an error or the communication between the main thread and the worker fails.
*   **Performance:** Offloading tasks to a worker can improve performance, but be mindful of the overhead of message passing.  For very simple tasks, it might be more efficient to execute them directly in the main thread.
*   **`useGlobalSubscriber`:** The use of `useGlobalSubscriber` suggests a more complex architecture where worker tasks can be triggered from various parts of the application.  Understand how `useGlobalSubscriber` works in `esoftplay` to use this component effectively.
*   **`injectedJavaScript`:** The `injectedJavaScript` prop is used to inject the worker code (`out.js`) and a "ready" message into the `WebView`.  The "ready" message is used to signal that the worker is initialized and ready to receive tasks.
*   **`originWhitelist`:** The `originWhitelist` prop is set to `["*"]`. This is generally not recommended for production apps due to security concerns.  You should restrict the origin whitelist to only the domains that your app needs to access.
*   **`dummyPageToBypassCORS`:** The `source` prop uses a dummy page URL. This is likely a workaround to bypass Cross-Origin Resource Sharing (CORS) issues when loading the worker code.  However, relying on this workaround can be risky.  A better approach would be to serve the worker code from the same origin as your app or configure your server to allow cross-origin requests.
* **Task IDs:** The component uses task IDs to keep track of pending tasks and their associated callbacks. This is essential for handling asynchronous responses from the worker correctly.
* **Retry Mechanism:** The `dispatch` function includes a retry mechanism using `createTimeout`. If the WebView is not ready when a task is submitted, the task is resubmitted after a short delay. This ensures that tasks are eventually executed even if the WebView is not immediately available.
* **`incognito`:** The `incognito` prop is set to `true`. This is likely for security purposes and prevents the WebView from storing cookies or other data.
