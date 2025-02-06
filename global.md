# useGlobalState Hook Documentation

This document describes the `useGlobalState` hook, a powerful utility for managing global state in React Native applications.  It provides a simple and efficient way to share state across components, persist data, and even synchronize with a remote server.

## Installation

This hook depends on the following packages:

- `@react-native-async-storage/async-storage`
- `esoftplay` (internal library, details not publicly available)
- `esoftplay/mmkv` (internal library, details not publicly available)
- `esoftplay/storage` (internal library, details not publicly available)
- `react`
- `react-fast-compare`

You'll need to have these packages installed in your project.  For `@react-native-async-storage/async-storage`, follow the official React Native documentation.  For the `esoftplay` libraries, refer to your internal documentation or contact the library maintainers.

## Usage

```typescript
import useGlobalState from 'esoftplay/global'; 

// Initialize a global state variable
const useCounter = useGlobalState<number>(0, { persistKey: 'counter' });

function MyComponent() {
  const [count, setCount] = useCounter.useState();

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={() => setCount(count + 1)} />
    </View>
  );
}
```

## API

### `useGlobalState<T>(initValue: T, options?: useGlobalOption): useGlobalReturn<T>`

Initializes and returns the global state management functions.

**Type Parameters:**

- `T`: The type of the state value.

**Arguments:**

- `initValue: T`: The initial value of the state.
- `options?: useGlobalOption`: An optional object containing configuration options.

**Returns:**

An object of type `useGlobalReturn<T>` containing the following functions:

#### `useGlobalReturn<T>` Interface

```typescript
interface useGlobalReturn<T> {
  useState: () => [T, (newState: T | ((newState: T) => T)) => void, () => T],
  get: (param?: string, ...params: string[]) => T,
  set: (x: T | ((old: T) => T)) => void,
  reset: () => void,
  listen: (cb: (value: T) => void) => () => void
  sync: () => () => void,
  connect: (props: useGlobalConnect<T>) => any,
  useSelector: (selector: (state: T) => any) => any;
}
```

#### `useGlobalOption` Interface

```typescript
interface useGlobalOption {
  persistKey?: string; // Key for persisting data.
  inFile?: boolean; // Use file storage (esoftplay specific).
  inFastStorage?: boolean; // Use MMKV storage (esoftplay specific).
  listener?: (data: any) => void; // Callback on state change.
  useAutoSync?: useGlobalAutoSync; // Configuration for automatic synchronization.
  jsonBeautify?: boolean; // Beautify JSON when persisting.
  isUserData?: boolean; // Flag for user data (esoftplay specific).
  loadOnInit?: boolean; // Load data from storage on initialization.
  onFinish?: () => void; // Callback after initial load.
}

interface useGlobalAutoSync {
  url: string; // URL for synchronization.
  post: (item: any) => Object; // Function to transform data for POST request.
  delayInMs?: number; // Delay between sync attempts.
  isSyncing?: (isSync: boolean) => void; // Callback to indicate syncing status.
}

interface useGlobalConnect<T> {
  selector?: (props: T) => any; // Selector function for connected components.
  render: (props: Partial<T> | T) => any; // Render function for connected components.
}
```

#### Functions within `useGlobalReturn<T>`

- `useState()`: Returns a tuple containing the current state value, a setter function, and the current value.  Similar to React's `useState` but manages global state.

- `get(param?: string, ...params: string[])`: Retrieves the state value, optionally accessing nested properties using dot notation.

- `set(x: T | ((old: T) => T))`: Updates the state value. Accepts either a new value or a function that receives the previous value and returns the new value.

- `reset()`: Resets the state to its initial value and clears any persisted data.

- `listen(cb: (value: T) => void)`: Registers a listener function that will be called whenever the state changes. Returns a function to unsubscribe the listener.

- `sync()`: Returns a function that triggers the synchronization process if `useAutoSync` is configured.

- `connect(props: useGlobalConnect<T>)`: Connects a component to the global state, allowing it to re-render when the state changes.  Similar to `connect` from Redux.

- `useSelector(selector: (state: T) => any)`:  Allows selecting specific parts of the global state.  Similar to `useSelector` from Redux.


## Examples

```typescript
// Simple counter example
const useCounter = useGlobalState<number>(0, { persistKey: 'counter' });
const [count, setCount] = useCounter.useState();
setCount(count + 1); // Increment the counter

// Accessing nested properties
const useUser = useGlobalState<{ name: string; address: { city: string } }>({ name: '', address: { city: '' } }, {});
const city = useUser.get('address', 'city');

// Auto synchronization example
const useItems = useGlobalState<any[]>([], {
  persistKey: 'items',
  useAutoSync: {
    url: '/api/items',
    post: (item) => ({ item }),
    isSyncing: (isSync) => { /* Update UI based on syncing status */ },
  },
});

const [items, setItems] = useItems.useState();
setItems([...items, { name: 'New Item' }]); // This will trigger auto-sync
```

## Persisting Data

The `persistKey` option allows you to persist the state across app restarts using AsyncStorage (or MMKV/file storage if specified).  Provide a unique key for each piece of state you want to persist.

## Auto Synchronization

The `useAutoSync` option enables automatic synchronization with a remote server.  You provide the URL, a function to transform the data for the POST request, and optional callbacks for handling syncing status.

## Connecting Components

The `connect` function simplifies connecting components to the global state.  You provide a `selector` function to choose which part of the state the component needs and a `render` function to display the data.

## User Data Reset

If `isUserData` is true and `persistKey` is provided, the state will be reset when the user data is cleared (esoftplay specific).

This documentation provides a comprehensive overview of the `useGlobalState` hook.  Refer to the source code for more detailed information.  Contact the library maintainers for any issues related to the `esoftplay` specific functionalities.