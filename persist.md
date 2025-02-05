# usePersistState Hook Documentation

This document describes the `usePersistState` custom React hook. This hook provides a way to manage state that persists across app restarts using MMKV for storage.

## Usage

```javascript
import usePersistState from 'esoftplay/persist'; // Adjust path as needed

function MyComponent() {
  const [name, setName, getName] = usePersistState('userName', 'Guest'); // Key, default value

  const handleNameChange = (newName) => {
    setName(newName);
  };

  return (
    <div>
      <p>Name: {name}</p>
      <input type="text" value={name} onChange={(e) => handleNameChange(e.target.value)} />
      <button onClick={() => setName('New Name')}>Set Name</button>
      <button onClick={() => setName((prevName) => prevName + '!')}>Append to Name</button>
      <button onClick={() => setName(undefined)}>Clear Name</button>
    </div>
  );
}
```

## API Reference

### `usePersistState<T>(persistKey: string, defaultValue?: T): [T, (newState: T | ((prevState: T) => T)) => void, () => T]`

A custom React hook that manages state persisted in MMKV.

* **Type Parameter:**
    * `T`: The type of the state.
* **Parameters:**
    * `persistKey`: The key used to store the state in MMKV.
    * `defaultValue?`: The initial value of the state if no value is found in MMKV. Optional.
* **Returns:** An array containing three elements:
    1. `currentState`: The current state value (`T`).
    2. `setState`: A function that takes a new state value or a function that receives the previous state and returns a new state value.  This function persists the new state to MMKV.
    3. `getState`: A function that returns the current state value. This is useful to get the most up-to-date value from the ref.

### `setState(newState: T | ((prevState: T) => T))`

Updates the state value and persists it to MMKV.

* **Parameters:**
    * `newState`: The new state value or a function that accepts the previous state and returns the new state.
* **Returns:** `void`

### `getState()`

Returns the current state value.

* **Returns:** The current state value (`T`).

## How it Works

The hook uses MMKV to store and retrieve the state.  On initialization, it attempts to retrieve the state from MMKV using the provided `persistKey`. If a value is found, it's parsed as JSON; otherwise, the `defaultValue` is used.  The state is stored in a `ref` to ensure the component always has the latest value even before the next render.

The `setState` function updates the `ref` and persists the new value to MMKV.  It uses `JSON.stringify` to store the value and handles the case where the value is `undefined` by removing the item from MMKV.  The hook uses `useState` (specifically the `rerender` function) to trigger a re-render after the state is updated.

The `useEffect` hook with the empty dependency array ensures that `isMountedRef` is set correctly and cleaned up on unmount.

## Benefits

* **Persistence:** State is preserved across app restarts.
* **Simplified State Management:**  Provides a clean and easy way to manage persistent state.
* **Uses MMKV:** MMKV is known for it's speed and efficiency.

## Example: Handling Complex Objects

```javascript
import usePersistState from 'esoftplay/persist';

function MyComponent() {
  const [user, setUser] = usePersistState('userProfile', { name: 'Guest', age: 0 });

  const handleNameChange = (newName) => {
    setUser({ ...user, name: newName }); // Update name, keep other properties
  };

  const handleAgeChange = (newAge) => {
    setUser(prevUser => ({ ...prevUser, age: newAge })); // Functional update for age
  };

  return (
    <div>
      <p>Name: {user.name}</p>
      <input type="text" value={user.name} onChange={(e) => handleNameChange(e.target.value)} />
      <p>Age: {user.age}</p>
      <input type="number" value={user.age} onChange={(e) => handleAgeChange(parseInt(e.target.value) || 0)} />
    </div>
  );
}
```

This example demonstrates how to use `usePersistState` with a complex object.  It shows how to update specific properties of the object while preserving others, both using direct updates and functional updates.  This pattern is crucial when dealing with objects or arrays in `usePersistState` to avoid accidentally overwriting data.  Make sure to spread the previous state (`...user` or `...prevUser`) when updating properties of an object.
