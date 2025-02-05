# useSafeState Hook Documentation

This document describes the `useSafeState` custom React hook. This hook provides a way to manage state while ensuring that state updates are not performed after the component has unmounted, preventing memory leaks and errors.

## Installation

This is a custom hook, so it doesn't require separate installation. Just include it in your project.

## Usage

```javascript
import useSafeState from 'esoftplay/state'; // Adjust path as needed

function MyComponent() {
  const [count, setCount, getCount] = useSafeState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };

  const delayedIncrement = () => {
    setTimeout(() => {
      setCount(prevCount => prevCount + 1); // Safe update, even if component unmounts
    }, 1000);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={delayedIncrement}>Increment (Delayed)</button>
      <p>Current Count (from getter): {getCount()}</p>
    </div>
  );
}
```

## API Reference

### `useSafeState<T>(defaultValue?: T): [T, (newState: T | ((prevState: T) => T)) => void, () => T]`

A custom React hook that manages state safely, preventing updates after the component has unmounted.

* **Type Parameter:**
    * `T`: The type of the state.
* **Parameters:**
    * `defaultValue?`: The initial value of the state. Optional.
* **Returns:** An array containing three elements:
    1. `currentState`: The current state value (`T`).
    2. `setState`: A function that takes a new state value or a function that receives the previous state and returns a new state value. This function updates the state safely.
    3. `getState`: A function that returns the current state value.

### `setState(newState: T | ((prevState: T) => T))`

Updates the state value safely.

* **Parameters:**
    * `newState`: The new state value or a function that accepts the previous state and returns the new state.
* **Returns:** `void`

### `getState()`

Returns the current state value.

* **Returns:** The current state value (`T`).

## How it Works

The hook uses a `ref` to hold the current state value.  The `setState` function checks the `isMountedRef` before updating the state.  The `useEffect` hook sets `isMountedRef` to `true` when the component mounts and to `false` when the component unmounts. This ensures that `setState` will not update the state after the component has unmounted. The `getter` method returns the most recent value.

## Benefits

* **Prevents Memory Leaks:** Avoids setting state after a component has unmounted, which can lead to memory leaks.
* **Prevents Errors:**  Eliminates errors that can occur when trying to update the state of an unmounted component.
* **Safe Asynchronous Operations:**  Allows you to safely perform asynchronous operations that might resolve after the component has unmounted.

## Example: Handling Asynchronous Operations

```javascript
import useSafeState from 'esoftplay/state';

function MyComponent() {
  const [data, setData] = useSafeState(null);

  useEffect(() => {
    let isMounted = true; // Local isMounted variable (alternative approach)

    const fetchData = async () => {
      try {
        const response = await fetch('/api/data');
        const json = await response.json();
        if (isMounted) { // Check the local isMounted variable
          setData(json);
        }
      } catch (error) {
        if (isMounted) { // Check the local isMounted variable
          console.error("Error fetching data:", error);
          setData({error: error.message});
        }
      }
    };

    fetchData();

    return () => {
      isMounted = false; // Set local isMounted to false on unmount
    };
  }, []);

  return (
    <div>
      {data ? (
        <pre>{JSON.stringify(data, null, 2)}</pre>
      ) : (
        <p>Loading data...</p>
      )}
    </div>
  );
}

```

This example demonstrates how to use `useSafeState` with an asynchronous data fetching operation. The `isMounted` variable within the `useEffect` ensures that the state is only updated if the component is still mounted when the fetch request resolves.  This prevents errors that could occur if the component unmounts before the data is fetched.  This is a very common use case for `useSafeState`.

