# UseTask

This custom hook provides a way to process an array of data items using an asynchronous task function. It manages the execution of the task for each item, ensuring that the tasks are processed sequentially and providing callbacks for completion and overall success/failure.

## Installation

This hook does not have any external dependencies beyond React itself.

## Usage

```javascript
import { UseTasks } from 'esoftplay/cache/use/tasks/import';


const task = UseTask() //declare outside main function

function MyComponent() {
  const [sync, reset] = task((item: any) => new Promise((next) => {
    setTimeout(next, 1000) // Simulate async task
  }), () => {
    // ondone task execute
    console.log("All items processed successfully!");
  })

  const handleReset = () => {
    reset();
    console.log("Task reset.");
  };

  const data = [1, 2, 3, 4, 5];

  return (
    <View>
      <Button title="Process Data" onPress={() => sync(data)} />
      <Button title="Reset Task" onPress={handleReset} />
    </View>
  );
}
```

## API

### `UseTask<T>(): (task: (item: T) => Promise<void>, onDone?: () => void) => [(data: T[], success?: ((isSuccess: boolean) => void) | undefined) => void, () => void]`

Returns a function that takes a `task` function and an optional `onDone` callback as arguments. This returned function, when called, returns an array containing:

1.  `run`: A function to start processing the data: `(data: T[], success?: (isSuccess: boolean) => void) => void`
2.  `reset`: A function to reset the task processing: `() => void`

## Parameters (Inner Function)

*   `task`: A function that takes a single item from the data array and returns a Promise. This function performs the asynchronous operation on each item.
*   `onDone`: (Optional) A callback function that is called when all items have been processed.

## Parameters (`run` Function)

*   `data`: The array of data items to process.
*   `success`: (Optional) A callback function that is called when the processing is initiated. It receives a boolean value indicating whether the processing started successfully (`true`) or not (`false`). The processing might not start if it's already in progress or if the data array is empty.

## Functionality

1.  **Sequential Processing:** The `run` function processes the data items sequentially, waiting for the `task` function to complete for each item before moving on to the next.

2.  **Asynchronous Tasks:** The `task` function is expected to return a Promise, allowing for asynchronous operations (e.g., API calls, file operations).

3.  **Completion Callback:** The `onDone` callback is called when all items in the data array have been processed.

4.  **Success Callback:** The `success` callback is called when the processing is *initiated*. It receives a boolean indicating whether the processing started (true) or was already in progress or the data array was empty (false).

5.  **Task Reset:** The `reset` function resets the `onProcess` flag, allowing the task to be started again.

6.  **Processing State:** The `onProcess` variable tracks whether the task is currently running.

## Important Considerations

*   **Asynchronous Operations:** The `task` function should handle asynchronous operations.
*   **Sequential Execution:** The data items are processed sequentially.
*   **Callbacks:** The `onDone` and `success` callbacks provide a way to handle completion and initiation status.
*   **Task Reset:** The `reset` function allows resetting the task processing.
*   **Error Handling:** The `try...catch` block handles errors that might occur during the task execution.  However, you might need more robust error handling within the `task` function itself.
*   **`onProcess` Flag:** This flag is crucial for preventing concurrent execution of the task.  It ensures that the task is only started if it's not already running.
*   **Empty Data Handling:** The `run` function checks if the data array is empty and calls the `success` callback with `false` if it is.
*   **`success` Callback Timing:** The `success` callback is called *before* the processing starts, not when it finishes.  It simply indicates whether the processing *started* successfully.  This is important to understand to avoid potential race conditions.
* **Type Safety:** The hook uses generics (`<T>`) to provide type safety for the data items. This is a good practice as it helps to prevent errors and improve code maintainability.
* **Clearer Callbacks:**  The use of separate callbacks for initiation (`success`) and completion (`onDone`) makes it easier to handle different stages of the task processing.
* **Preventing Concurrent Executions:** The `onProcess` flag effectively prevents concurrent executions of the task.  This is essential for ensuring that the tasks are processed sequentially and that there are no conflicts or race conditions.
