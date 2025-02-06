# Timeout Utilities

This library provides utility functions for managing timeouts, intervals, and debouncing in React applications.

## Functions

### `createTimeout()`

Creates a timeout function.

```typescript
import { createTimeout } from 'esoftplay/timeout'; // Import the utility

const timeout = createTimeout();

timeout.set(() => {
  // Code to execute after the timeout
  console.log("Timeout completed!");
}, 1000); // 1000 milliseconds (1 second)

timeout.clear(); // Clear the timeout if needed
```

**Methods:**

*   `set(callback: Function, delay: number)`: Sets a timeout. The `callback` function will be executed after the specified `delay` in milliseconds.
*   `clear()`: Clears the timeout if it's still active.

### `useTimeout()`

A React hook that provides a timeout function.  It automatically clears the timeout when the component unmounts.

```typescript
import { useTimeout } from 'esoftplay/timeout';

function MyComponent() {
  const setTimeout = useTimeout();

  const handleClick = () => {
    setTimeout(() => {
      // Code to execute after the timeout
      console.log("Timeout completed!");
    }, 1000);
  };

  return (
    <button onClick={handleClick}>Start Timeout</button>
  );
}
```

**Returns:**

*   `set(callback: Function, delay: number)`:  A function to set a timeout, identical in behavior to the `set` method of `createTimeout()`.

### `createInterval()`

Creates an interval function.

```typescript
import { createInterval } from 'esoftplay/timeout';

const interval = createInterval();

interval.set(() => {
  // Code to execute repeatedly at the interval
  console.log("Interval tick!");
}, 500); // 500 milliseconds (0.5 seconds)

interval.clear(); // Clear the interval
```

**Methods:**

*   `set(callback: Function, delay: number)`: Sets an interval. The `callback` function will be executed repeatedly at the specified `delay` in milliseconds.
*   `clear()`: Clears the interval.

### `useInterval()`

A React hook that provides an interval function. It automatically clears the interval when the component unmounts.

```typescript
import { useInterval } from 'esoftplay/timeout';

function MyComponent() {
  const setInterval = useInterval();

  useEffect(() => {
    setInterval(() => {
      // Code to execute repeatedly
      console.log("Interval tick!");
    }, 500);
  }, [setInterval]); // Add setInterval to the dependency array

  return (
    <div>Interval running</div>
  );
}
```

**Returns:**

*   `set(callback: Function, delay: number)`: A function to set an interval, identical in behavior to the `set` method of `createInterval()`.


### `createDebounce()`

Creates a debounce function.  This ensures a function is only called once after a certain amount of time has elapsed since the last time it was invoked.

```typescript
import { createDebounce } from 'esoftplay/timeout';

const debounce = createDebounce();

const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  debounce.set(() => {
    // Code to execute after the debounce period
    console.log("Input value:", event.target.value);
  }, 300); // 300 milliseconds
};
```

**Methods:**

*   `set(callback: Function, delay: number)`: Sets a debounced function. The `callback` will be executed after the specified `delay` in milliseconds *since the last call* to `set`.
*   `clear()`: Clears any pending debounced calls.

### `useDebounce()`

A React hook that provides a debounced function. It automatically clears any pending debounced calls when the component unmounts.

```typescript
import { useDebounce } from 'esoftplay/timeout';

function MyComponent() {
  const setDebouncedValue = useDebounce();
  const [inputValue, setInputValue] = useState("");

  const handleInputChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setInputValue(event.target.value);
    setDebouncedValue(() => {
      // Code to execute after the debounce period
      console.log("Debounced input value:", event.target.value);
    }, 300);
  };

  return (
    <input type="text" value={inputValue} onChange={handleInputChange} />
  );
}

```

**Returns:**

*   `set(callback: Function, delay: number)`: A function to set a debounced function, identical in behavior to the `set` method of `createDebounce()`.
```
