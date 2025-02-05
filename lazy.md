# useLazyState Hook Documentation

This document describes the `useLazyState` custom React hook.  This hook provides a way to manage state updates lazily, preventing unnecessary re-renders. It's particularly useful when dealing with frequent state changes where you only want to trigger a re-render after a certain condition is met or at a less frequent interval.

## Installation

This is a custom hook, so it doesn't require separate installation.  Just include it in your project.

## Usage

```javascript
import useLazyState from 'esoftplay/lazy'; // Adjust path as needed

function MyComponent() {
  const [count, setCount, getCount] = useLazyState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1); // Increment the count but don't re-render yet

    // ... some other logic ...

    // Trigger re-render when ready
    setCount(getCount())(); // This will trigger a re-render with the latest count
  };

  const directSet = () => {
    setCount(5); // Set the value directly, but doesn't trigger re-render yet
    setCount(getCount())(); // This will trigger a re-render with the latest count
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment (Lazy)</button>
      <button onClick={directSet}>Set Directly (Lazy)</button>
    </div>
  );
}
```

## API Reference

### `useLazyState<T>(initialState?: T)`

A custom React hook that manages state updates lazily.

* **Type Parameter:**
    * `T`: The type of the state.
* **Parameters:**
    * `initialState?`: The initial value of the state.  Optional.
* **Returns:** An array containing three elements:
    1. `currentState`: The current state value (`T`). This value is obtained via a ref, so it's always up-to-date, even before a re-render.
    2. `setState`: A function that takes a new state value or a function that receives the previous state and returns a new state value.  Calling this function updates the ref holding the state, but it *returns* a function that, when called, triggers the re-render.
    3. `getState`: A function that returns the current state value. This is useful to get the most up-to-date state before triggering a re-render.

### `setState(newValue: T | ((prev: T) => T))`

Updates the state value stored in the ref.

* **Parameters:**
    * `newValue`: The new state value or a function that accepts the previous state and returns the new state.
* **Returns:** A function. When this returned function is called, it triggers a re-render using the latest state value.

### `getState()`

Returns the current state value stored in the ref.

* **Returns:** The current state value (`T`).

## How it Works

The hook uses a `ref` to hold the state value.  The `setState` function updates the value in the ref immediately.  However, the re-render is only triggered when the function that `setState` *returns* is called. This allows you to perform multiple state updates without causing intermediate re-renders.  `getState` is provided to access the most recent state value in the ref before triggering the re-render.

## Benefits

* **Performance Optimization:**  Reduces the number of re-renders, especially when dealing with frequent state updates.
* **Control over Re-renders:**  Provides fine-grained control over when re-renders occur.
