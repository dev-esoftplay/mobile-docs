# Storage Utility Documentation

This document describes the `Storage` utility, which provides a simple interface for storing and retrieving data using Expo's `FileSystem` API. It also includes functionality for sending files to Telegram.

## Installation

This utility relies on Expo's `FileSystem` API. Ensure you have the necessary dependencies installed:

```bash
expo install expo-file-system
```

## Usage

```javascript
import Storage from 'esoftplay/storage'; // Adjust path

// Setting an item
await Storage.setItem('myKey', 'myValue');

// Getting an item
const value = await Storage.getItem('myKey');

// Removing an item
await Storage.removeItem('myKey');

// Clearing all items
Storage.clear();

// Sending a file to Telegram
await Storage.sendTelegram('myKey', 'A message for Telegram', () => {
  console.log("File sent successfully!");
}, (error) => {
  console.error("Failed to send file:", error);
}, '-1001234567890', (originalName) => `modified_${originalName}`); // Example chat ID and custom file name function

```

## API Reference

### `Storage.getDBPath(key: string): string`

Generates the file path for a given key.  The path is within the Expo cache directory, in a subdirectory named `lib-storage-cache`.  The key is transformed to be filename-safe.

* **Parameters:**
    * `key`: The key for the data.
* **Returns:** The full file path.

### `Storage.getItem(key: string): Promise<string | null>`

Retrieves data associated with a given key.

* **Parameters:**
    * `key`: The key for the data.
* **Returns:** A `Promise` that resolves with the stored value (parsed as JSON) or `null` if the key does not exist or an error occurs.

### `Storage.setItem(key: string, value: string): Promise<string>`

Stores data associated with a given key.  The value is stringified using `JSON.stringify` before being written to the file system.

* **Parameters:**
    * `key`: The key for the data.
    * `value`: The data to store (will be converted to string).
* **Returns:** A `Promise` that resolves with the stored value.

### `Storage.removeItem(key: string): Promise<string>`

Removes the data associated with a given key.

* **Parameters:**
    * `key`: The key for the data to remove.
* **Returns:** A `Promise` that resolves with the removed key.

### `Storage.clear(): void`

Clears all stored data by deleting the `lib-storage-cache` directory.

* **Returns:** `void`

### `Storage.sendTelegram(key: string, message: string, onDone?: () => void, onFailed?: (reason: string) => void, chat_id?: string, customFileName?: (originalName: string) => string): Promise<void>`

Sends a file stored under the given key to Telegram using the Telegram Bot API.

* **Parameters:**
    * `key`: The key of the file to send.
    * `message`: The caption for the Telegram message.
    * `onDone?`: A callback function called when the file is sent successfully.
    * `onFailed?`: A callback function called if sending the file fails.  Receives the error message or result from Telegram.
    * `chat_id?`: The Telegram chat ID to send the file to. Defaults to a specific group ID.
    * `customFileName?`: An optional function that allows modifying the file name before sending.  Receives the original file name (without extension) and returns the new name.
* **Returns:** A `Promise` that resolves when the operation is complete.  Does not resolve with a value.

## File Storage

Data is stored in the Expo cache directory within a folder named `lib-storage-cache`.  Each key has a corresponding file named `<key-with-slashes-replaced-by-dashes>.txt`.  Values are stringified using `JSON.stringify` before being written to the file.

## Telegram Integration

The `sendTelegram` function uses the Telegram Bot API to send files.  It requires a bot token and a chat ID.  A default chat ID is provided, but it's recommended to configure this. The function uses `FormData` to create the request body, including the file and caption.  It provides callbacks for success and failure.  It also allows customization of the file name sent to Telegram.

## Error Handling

The `getItem` function handles potential errors during file reading and JSON parsing by returning `null`.  The `sendTelegram` function provides `onFailed` callbacks to handle errors during file sending.

## Example (Combined)

```javascript
import Storage from 'esoftplay/storage';

async function saveData(key, data) {
  try {
    await Storage.setItem(key, JSON.stringify(data));
    console.log(`Data with key "${key}" saved successfully.`);
  } catch (error) {
    console.error(`Error saving data:`, error);
  }
}

async function loadData(key) {
  try {
    const data = await Storage.getItem(key);
    if (data) {
      console.log(`Data with key "${key}" loaded successfully:`, data);
      return JSON.parse(data)
    } else {
      console.log(`No data found for key "${key}".`);
      return null;
    }
  } catch (error) {
    console.error(`Error loading data:`, error);
    return null;
  }
}

async function sendFileToTelegram(key, message, chatId) {
  try {
    await Storage.sendTelegram(key, message, () => {
      console.log("File sent to Telegram successfully!");
    }, (error) => {
      console.error("Error sending file to Telegram:", error);
    }, chatId);
  } catch (error) {
    console.error("Error in sendFileToTelegram:", error);
  }
}


// Example usage:
saveData('myObject', { name: 'John Doe', age: 30 });
loadData('myObject');

// Assuming you have a file stored with the key 'myObject', and a Telegram chat ID.
sendFileToTelegram('myObject', 'Important document', '-1001234567890');
```

This documentation provides a comprehensive overview of the `Storage` utility and its functionalities. Remember to adjust import paths as needed.
