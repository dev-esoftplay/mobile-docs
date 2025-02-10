# LibObject

This module provides a utility class `m` (and static functions) for working with immutable data structures using the `immhelper` library. It offers a chainable API for performing common operations like pushing, unshifting, splicing, setting, updating, and more.

## Installation

This module relies on `immhelper`. Ensure you have it installed:

```bash
npm install immhelper
# or
yarn add immhelper
  ```

## Usage

```javascript
import { LibObject } from 'esoftplay/cache/lib/object/import';

// Using the class:
const libObject = new LibObject([1, 2, 3]);

const updatedArray = libObject
  .push(4)
  .unshift(0)
  .set(5)(2) // Set the element at index 2 to 5
  .value(); // Get the updated array

console.log(updatedArray); // Output: [0, 1, 2, 5, 4]

// Using static functions:
const newArray = LibObject.push([1, 2, 3], 4);
console.log(newArray); // Output: [1, 2, 3, 4]

const updatedObj = LibObject.set({ a: 1, b: 2 }, 3)('b');
console.log(updatedObj); // Output: { a: 1, b: 3 }

// Remove keys from an array of objects:
const arrWithKeysRemoved = LibObject.removeKeys([{ a: 1, b: 2 }, { a: 3, b: 4 }], ['b']);
console.log(arrWithKeysRemoved); // Output: [{a: 1}, {a: 3}]

// Replace an item in an array:
const arrWithItemReplaced = LibObject.replaceItem([1, 2, 3], (item) => item === 2, 4);
console.log(arrWithItemReplaced); // Output: [1, 4, 3]
```

## API

### Class `m`

#### Constructor

*   `constructor(array: any)`: Creates a new `LibObject` instance with the initial array.

#### Methods

*   `value(): any`: Returns the current value of the data structure.
*   `push(value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Appends elements to the end of the array.
*   `unshift(value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Adds elements to the beginning of the array.
*   `splice(index: number, deleteCount: number, value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Removes and/or inserts elements at a specific index.
*   `unset(index: number | string, ...indexs: (string | number)[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Removes elements at specific indices or keys from an object.
*   `set(value: any): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Sets the value at a specific index or key.
*   `update(callback: (lastValue: any) => any): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Updates the value using a callback function.
*   `assign(obj1: any): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Assigns (merges) an object to the current value. Performs a deep copy of the assigned object.
*   `removeKeys(deletedItemKeys: string[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Removes specified keys from an array of objects or an object.
*   `replaceItem<T>(filter: (item: T, index: number) => boolean, newItem: T): (cursor?: string | number, ...cursors: (string | number)[]) => this`: Replaces an item in an array or object based on a filter function.
*   `cursorBuilder(command: string, array: any, value: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => this`: A helper function to create chained calls.

### Static Functions

The `LibObject` class also provides static functions that mirror the instance methods. These can be used directly without creating a `LibObject` instance.  They take the data structure as the first argument.

*   `LibObject.push(array: any, value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.unshift(array: any, value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.splice(array: any, index: number, deleteCount: number, value ?: any, ...values: any[]): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.unset(obj: any, index: number | string, ...indexs: (string | number)[]): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.set(obj: any, value: any): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.update(obj: any, callback: (lastValue: any) => any): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.assign(obj: any, obj1: any): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.removeKeys(arrayOrObj: any, deletedItemKeys: string[]): (cursor?: string | number, ...cursors: (string | number)[]) => any`
*   `LibObject.replaceItem<T>(arrayOrObj: any, filter: (item: T, index: number) => boolean, newItem: T): (cursor?: string | number, ...cursors: (string | number)[]) => any`

## Helper Functions

*   `deepCopy(o: any)`: Creates a deep copy of an object or array.
*   `_removeKeys(objOrArr: any, keysToRemove: string[])`: Removes keys from an object or an array of objects.
*   `_replaceItem(data: any, predicate: (item: any, index: number) => boolean, newItem: any)`: Replaces an item in an array or object based on a predicate.

## Important Considerations

*   **Immutability:** This module uses `immhelper` to ensure immutability.  All operations return a *new* object or array; they do not modify the original data structure.
*   **Chaining:** The methods of the `LibObject` class are chainable, allowing you to perform multiple operations in a concise way.
*   **Cursors:** The `cursor` and `cursors` arguments allow you to target nested properties within the data structure.  They are joined with a dot (`.`) to create a path.
*   **Static vs. Instance Methods:** The module provides both static functions and instance methods.  Use the static functions when you don't need to chain operations.  Use the instance methods when you want to chain.
*   **Deep Copy:** The `assign` method and the helper function `deepCopy` perform deep copies to ensure that nested objects are also cloned.
* **Type Safety:** While the code provides some type hints, ensure you have proper type definitions for your data structures for full type safety.
