# UseWorker

This custom hook provides a way to run a function on the UI thread using React Native Reanimated's `runOnUI`.  It's designed for offloading work to the UI thread, particularly animations or UI updates that need to be performant.  It also provides a callback mechanism to receive the result back on the JavaScript thread.

## Installation

This hook requires `react-native-reanimated`. Ensure it's installed:

```bash
npm install react-native-reanimated
# or
yarn add react-native-reanimated
```

You'll also need to configure Reanimated.  Refer to the Reanimated documentation for setup instructions.

## Usage

```javascript
import { UseWorker } from 'esoftplay/cache/use/worker/import';

function MyComponent() {
  const myWorkerFunction = (args) => {
    'worklet'; // Important: Add 'worklet' directive
    const result = args.reduce((acc, val) => acc + val, 0); // Example calculation
    return result;
  };

  const runWorker = UseWorker(myWorkerFunction);

  const handlePress = () => {
    runWorker([1, 2, 3, 4, 5], (result) => {
      console.log("Result from worker:", result);
    });
  };

  return (
    <View>
      <Button title="Run Worker" onPress={handlePress} />
    </View>
  );
}
```

## API

### `UseWorker(func: (args: any[]) => void): (args: any[], callback: (res: any) => void) => void`

Returns a function that takes an array of arguments (`args`) and a callback function (`callback`) as arguments.

## Parameters

*   `func`: The function to be executed on the UI thread.  **It's crucial to add the `'worklet'` directive at the beginning of this function.** This function should take an array of arguments as input and return a value.

## Parameters (Returned Function)

*   `args`: An array of arguments to be passed to the worker function.
*   `callback`: A function to be called with the result returned by the worker function.  This callback will be executed on the JavaScript thread.

## Functionality

1.  **UI Thread Execution:** The provided `func` is executed on the UI thread using `runOnUI`.

2.  **`'worklet'` Directive:** The `'worklet'` directive is essential for functions that are intended to run on the UI thread using Reanimated.

3.  **Callback on JS Thread:** The `clbk` function is defined within the hook and is responsible for calling the user-provided `callback` function.  It uses `runOnJS` to ensure that the callback is executed on the JavaScript thread.

4.  **Result Passing:** The result of the `func` execution on the UI thread is passed to the `clbk` function, which then calls the user-provided `callback` function on the JavaScript thread.

## Important Considerations

*   **`react-native-reanimated`:** This hook relies on `react-native-reanimated` for running functions on the UI thread.  Make sure it's installed and configured correctly.
*   **`'worklet'` Directive:**  **The `'worklet'` directive is mandatory for functions that will be executed on the UI thread using Reanimated.**  Without it, the code will not work as expected.
*   **UI Thread vs. JS Thread:**  Understand the difference between the UI thread and the JavaScript thread.  UI-related updates and animations should ideally be performed on the UI thread for performance reasons.  This hook helps in offloading work to the UI thread.
*   **Callback Execution:** The `callback` function is guaranteed to be executed on the JavaScript thread.
*   **Argument Passing:** The `args` are passed as an array to the worker function.
*   **Return Value:** The worker function (`func`) should return a value, which will be passed to the `callback` function.
*   **Performance:** Using `runOnUI` can significantly improve the performance of UI-related operations, especially animations and complex calculations.
* **Reanimated Setup:** Ensure that you have correctly set up Reanimated in your project. This usually involves adding the Reanimated plugin to your Babel configuration.
* **`runOnJS`:** The use of `runOnJS` within the `clbk` function is crucial. It ensures that the user-provided `callback` function is executed on the JavaScript thread.  This is important because UI thread code cannot directly interact with JavaScript code or update React state. `runOnJS` bridges the gap between the UI thread and the JavaScript thread.
* **`runOnUI` Execution:** The `runOnUI(run)()` syntax is used to execute the `run` function (which contains the `clbk` and `func` calls) on the UI thread. The double parentheses `()` at the end are necessary to immediately execute the function returned by `runOnUI`.
* **'worklet' Directive Placement:** The `'worklet'` directive must be placed at the very beginning of the function that will be executed on the UI thread (in this case, the `func` argument passed to the `UseWorker` hook).  It's also important to ensure this function is not an arrow function.  Arrow functions are not compatible with the `'worklet'` directive.
* **Use Cases:** This hook is particularly useful for performing complex calculations, animations, or UI updates that could potentially block the JavaScript thread and cause performance issues.  By offloading this work to the UI thread, you can keep the JavaScript thread free to handle other tasks and ensure a smoother user experience.
