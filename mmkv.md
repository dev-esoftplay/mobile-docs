# FastStorage

This module provides a simple wrapper around `react-native-mmkv` for asynchronous and synchronous data storage.  It offers methods for retrieving, setting, and removing data, as well as clearing the entire storage.

## Installation

Make sure you have `react-native-mmkv` installed in your project. If not, follow the installation instructions in the library's documentation.

```bash
npm install react-native-mmkv
# or
yarn add react-native-mmkv
```

## Usage

```javascript
import FastStorage from 'esoftplay/mmkv';

// Setting data
FastStorage.setItem('myKey', 'myValue');

// Retrieving data synchronously
const valueSync = FastStorage.getItemSync('myKey');
console.log(valueSync); // Output: myValue

// Retrieving data asynchronously
FastStorage.getItem('myKey')
  .then(value => {
    console.log(value); // Output: myValue
  });

// Removing data
FastStorage.removeItem('myKey');

// Clearing all data
FastStorage.clear();
```

## API

### `getItemSync(key: string): string | null | undefined`

Retrieves a string value associated with the given key synchronously.

* **Parameters:**
    * `key` (string): The key of the value to retrieve.

* **Returns:**
    * `string | null | undefined`: The string value associated with the key, or `null` if the key exists but the value is null, or `undefined` if the key does not exist.

### `getItem(key: string): Promise<string | null | undefined>`

Retrieves a string value associated with the given key asynchronously.

* **Parameters:**
    * `key` (string): The key of the value to retrieve.

* **Returns:**
    * `Promise<string | null | undefined>`: A Promise that resolves with the string value associated with the key, or `null` if the key exists but the value is null, or `undefined` if the key does not exist.

### `setItem(key: string, value: string): void`

Sets the value associated with the given key.

* **Parameters:**
    * `key` (string): The key to associate the value with.
    * `value` (string): The string value to store.

* **Returns:**
    * `void`

### `removeItem(key: string): void`

Removes the value associated with the given key.

* **Parameters:**
    * `key` (string): The key of the value to remove.

* **Returns:**
    * `void`

### `clear(): void`

Clears all data stored in the MMKV instance.

* **Returns:**
    * `void`


## Example

```javascript
import FastStorage from esoftplaymmkv';

async function saveData() {
  FastStorage.setItem('name', 'John Doe');
  const name = await FastStorage.getItem('name');
  console.log(`Name: ${name}`);

  const age = FastStorage.getItemSync('age'); // Will be undefined as it's not set
  console.log(`Age: ${age}`);

  FastStorage.removeItem('name');
  const nameAfterRemoval = await FastStorage.getItem('name');
  console.log(`Name after removal: ${nameAfterRemoval}`); // Output: undefined

  FastStorage.clear();
  const allKeys = await FastStorage.getItem('anyKey'); // will return undefined
  console.log(`anyKey after clear: ${allKeys}`); // Output: undefined
}

saveData();
```

## Important Considerations

* This implementation only stores and retrieves string values.  If you need to store other data types, you'll need to serialize them to strings (e.g., using `JSON.stringify()`) before storing and deserialize them after retrieval (e.g., using `JSON.parse()`).
* `getItemSync` provides synchronous access, which can be useful in certain situations, but be mindful of potential performance implications, especially when dealing with large amounts of data.  For most cases, the asynchronous `getItem` is recommended.