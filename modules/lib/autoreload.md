# LibAutoreload

This module provides a simple way to set up automatic, recurring execution of a callback function in React Native, ensuring the callback runs after all interactions are complete. It uses `InteractionManager` to prevent updates during animations or transitions, improving performance and user experience. This version is designed for use within a React `useEffect` hook.

## Installation

This library doesn't require any external dependencies beyond what's included in React Native. Just include the code in your project.

## Usage within `useEffect`

```javascript
import React, { useEffect } from 'react';
import { LibAutoreload } from 'esoftplay/cache/lib/autoreload/import';

function MyComponent() {
  useEffect(() => {
    const myUpdateFunction = () => {
      // Perform your update logic here. For example:
      console.log("Auto-reload triggered!");
      // You might update state, fetch new data, etc.
    };

    // Set up auto-reload every 3 seconds
    LibAutoreload.set(myUpdateFunction, 3000);

    // Return the clear function to be called on unmount
    return () => LibAutoreload.clear();
  }, []); // Empty dependency array ensures this runs only once on mount and unmount

  // ... rest of your component
  return (...);
}

export default MyComponent;
```

## API

### `set(callback: () => void, duration?: number): void`

Sets up an interval to call the provided callback function repeatedly. The callback will be executed after all interactions are complete using `InteractionManager.runAfterInteractions`.

* **Parameters:**
    * `callback` (`() => void`): The function to be called repeatedly. This function should not accept any arguments.
    * `duration` (number, optional): The time interval between calls, in milliseconds. Defaults to 6000 (6 seconds) if not provided.

* **Returns:**
    * `void`

### `clear(): void`

Clears the currently set auto-reload interval, preventing further calls to the callback function.

* **Returns:**
    * `void`

## Example (within `useEffect`)

```javascript
import React, { useEffect, useState } from 'react';
import { LibAutoreload } from 'esoftplay/cache/lib/autoreload/import';

function MyComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('/api/data');
        const newData = await response.json();
        setData(newData);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    fetchData(); // Initial fetch

    const updateData = () => {
      fetchData();
    };

    LibAutoreload.set(updateData, 5000); // Update every 5 seconds

    return () => LibAutoreload.clear(); // Clear on unmount
  }, []);

  return (
    <div>
      {/* ... display your data ... */}
      {data ? <pre>{JSON.stringify(data, null, 2)}</pre> : <p>Loading...</p>}
    </div>
  );
}

export default MyComponent;

```

## Important Considerations

* **`InteractionManager`:** This library uses `InteractionManager.runAfterInteractions` to ensure that the callback is executed after all interactions (like animations and transitions) are complete. This is crucial for preventing UI glitches and ensuring a smooth user experience. Avoid performing heavy or time-consuming operations directly within the callback if they might block user interactions.
* **Clearing the Interval (within `useEffect`):**  The most important change in this version is how the interval is cleared. By returning `LibAutoreload.clear()` from the `useEffect` callback, you ensure that the interval is cleared when the component unmounts. This prevents memory leaks and ensures the callback isn't executed after the component is no longer mounted.
* **Callback Function:** The callback function should not accept any arguments.
* **Duration:** Be mindful of the `duration` you set. Calling the callback too frequently can impact performance. Choose a duration that balances the need for updates with the potential impact on resources.