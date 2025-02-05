# useGlobalSubscriber Hook Documentation

This document describes the `useGlobalSubscriber` custom React hook. This hook provides a simple mechanism for managing global state and subscribing to changes in that state.  It's useful for situations where you need to share state across multiple, potentially unrelated components in your application.

## Installation

This is a custom hook, so it doesn't require separate installation. Just include it in your project.

## Usage

```javascript
import useGlobalSubscriber from './path/to/useGlobalSubscriber'; // Adjust path

// Create a global state manager (typically in a shared utility file)
const globalState = useGlobalSubscriber(0); // Initial value of 0

function ComponentA() {
  const value = globalState.getValue();
  const subscribe = globalState.useSubscribe((newValue) => {
    console.log("Component A received:", newValue);
  });

  const increment = () => {
    globalState.trigger(value + 1);
  };

  return (
    <div>
      <p>Value in Component A: {value}</p>
      <button onClick={increment}>Increment (updates global state)</button>
    </div>
  );
}

function ComponentB() {
  const value = globalState.getValue();
  const subscribe = globalState.useSubscribe((newValue) => {
    console.log("Component B received:", newValue);
  });

  return (
    <div>
      <p>Value in Component B: {value}</p>
    </div>
  );
}

// ... other components ...
```

## API Reference

### `useGlobalSubscriber(defaultValue?: any): useGlobalSubscriberReturn`

Creates a global state manager.

* **Parameters:**
    * `defaultValue?`: The initial value of the global state. Optional.
* **Returns:** An object with the following methods:

#### `useGlobalSubscriberReturn` Interface:

* **`getValue(): any`:** Returns the current value of the global state.
* **`reset(): void`:** Resets the global state to its `defaultValue`.
* **`useSubscribe(func: Function): Function`:** Subscribes a function to changes in the global state.  Returns the `notify` function, but it is not necessary to use it directly.
* **`trigger(newValue?: any): void`:** Updates the global state and notifies all subscribers.

### `getValue()`

Returns the current value of the global state.

### `reset()`

Resets the global state to its default value.

### `useSubscribe(func: Function)`

Subscribes a function to changes in the global state.  The provided function will be called whenever the global state is updated using `trigger`.

* **Parameters:**
    * `func`: The function to subscribe. This function will receive the new state value as an argument.
* **Returns:** The `notify` function, but it is not necessary to use it directly.  The `notify` function will already be called internally by the `useSubscribe` function.

### `trigger(newValue?: any)`

Updates the global state and notifies all subscribers.

* **Parameters:**
    * `newValue?`: The new value of the global state. If not provided, the most recent value will be used.
* **Returns:** `void`

## How it Works

The hook maintains a `Set` of subscriber functions.  `useSubscribe` adds a function to this set.  `trigger` updates the global state and then iterates over the set, calling each subscriber function with the new value. `InteractionManager.runAfterInteractions` is used to ensure that state updates and notifications are run after any current interactions have completed, improving performance and avoiding potential issues with concurrent updates.  When the last subscriber unsubscribes, the global state is reset to its default value.

## Benefits

* **Global State Management:** Provides a simple way to manage state that needs to be accessible by multiple components.
* **Simplified Subscriptions:**  Makes it easy for components to subscribe to changes in the global state.
* **Preventing stale closures** The use of `InteractionManager.runAfterInteractions` helps prevent stale closures by ensuring that updates are processed after interactions have finished.
* **Automatic Reset:** Resets the global state when no components are subscribed, helping with memory management.

## Example: Sharing Data Between Components

```javascript
import useGlobalSubscriber from './path/to/useGlobalSubscriber';

const userState = useGlobalSubscriber({ name: 'Guest' });

function UserProfile() {
  const user = userState.getValue();
  const subscribe = userState.useSubscribe(); // No callback needed here, only for subscribing

  return <p>Name: {user.name}</p>;
}

function UserSettings() {
  const user = userState.getValue();
  const subscribe = userState.useSubscribe(); // No callback needed here, only for subscribing

  const updateName = (newName) => {
    userState.trigger({ ...user, name: newName });
  };

  return (
    <div>
      <input type="text" value={user.name} onChange={(e) => updateName(e.target.value)} />
    </div>
  );
}
```

In this example, `UserProfile` and `UserSettings` both use the same `userState` object.  Changes made in `UserSettings` will be reflected in `UserProfile` because both components are subscribed to the global state changes.
