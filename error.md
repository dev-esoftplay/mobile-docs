# Error Handling and Reporting Utility Functions Documentation

This document describes a set of utility functions for handling and reporting errors within a React Native application, integrated with Expo, `esoftplay`, and MMKV.

## Installation

These functions rely on various libraries. Ensure you have the necessary dependencies installed:

```bash
npm install expo-constants expo-file-system react-native mmkv
# And any other dependencies used by esoftplay, LibCurl, UserClass, etc.
```

## Usage

```javascript
import { setError, reportApiError, sendTm, getError } from 'esoftplay/error'; // Adjust path

// Setting a general error
setError(new Error("Something went wrong!"));

// Reporting an API error
reportApiError(fetchResponse, "API request failed.");

// Sending a Telegram message
sendTm("A message to send to Telegram.");

// Retrieving and reporting a stored error
getError();
```

## API Reference

### `getTime()`

Generates an ISO 8601 timestamp string with GMT+7 timezone.

* **Returns:** A string representing the current time in ISO 8601 format with GMT+7.

### `setError(error?: any)`

Stores an error object in MMKV for later reporting.

* **Parameters:**
    * `error?`: The error object to store.  Converts the error to a string for storage.
* **Returns:** `void`

### `reportApiError(fetch: any, error: any)`

Reports an API error to Telegram (in development, reports to a specific group; in production, reports to configured Telegram IDs).

* **Parameters:**
    * `fetch`: The fetch request object (for debugging purposes).
    * `error`: The error message.
* **Returns:** `void`

### `sendTm(message: string, chat_id?: string, bot?: string, res?: (result: any) => void)`

Sends a message to Telegram.

* **Parameters:**
    * `message`: The message to send.
    * `chat_id?`: The Telegram chat ID. If not provided, uses the IDs from the configuration.
    * `bot?`: The Telegram bot token. If not provided, uses a default bot token.
    * `res?`: An optional callback function to handle the result of the Telegram API call.
* **Returns:** `void`

### `getError()`

Retrieves a stored error from MMKV and reports it to Telegram.  Clears the error from MMKV after reporting.  Handles specific known errors differently (e.g., ignores some, reports others to a specific group).

* **Returns:** `void`

### `cleanCache()`

Attempts to clear the application's cache directory.

* **Returns:** `void`

### `myErrorHandler(e: any, isFatal: any)`

A custom error handler that logs errors in non-development mode and calls the default error handler.

* **Parameters:**
    * `e`: The error object.
    * `isFatal`: A boolean indicating if the error is fatal.
* **Returns:** `void`

## Global Error Handling

The `ErrorUtils.setGlobalHandler` function is used to set `myErrorHandler` as the global error handler.  This ensures that all uncaught errors are processed by your custom error handling logic.

## Error Reporting to Telegram

The functions use `LibCurl` to send error reports to Telegram. The Telegram bot token and chat IDs are configurable (in production) via the `esp.config()` method.  In development, error reports are sent to a specific Telegram group.

## Error Storage with MMKV

Errors are stored in MMKV using a key specific to the application's domain.  This allows errors to persist across app restarts and be retrieved later for reporting.

## Expo Integration

The functions utilize `expo-constants` to gather information about the application (name, version, SDK version, etc.) for inclusion in error reports.  `expo-file-system` is used to clear the cache.

## esoftplay Integration

The functions rely on `esoftplay` for configuration (`esp.config()`), user information (`UserClass`), and routing information (`UserRoutes`).  They also use `LibCurl` from `esoftplay` for making HTTP requests (to Telegram).

## Example (Combined)

```javascript
import { setError, reportApiError, sendTm, getError } from 'esoftplay/error';

try {
  // Some code that might throw an error
  throw new Error("An example error.");
} catch (error) {
  setError(error); // Store the error
  reportApiError(null, error.message); // Report the error (API context might be null here)
  sendTm("An error occurred in the app."); // Send a general Telegram message
}

// Later, perhaps on app start:
getError(); // Check for and report any stored errors

// Example of sending a Telegram message to a specific chat:
sendTm("Hello from the app!", "-1234567890"); // Replace with the actual chat ID
```

This documentation provides a comprehensive overview of the error handling and reporting utility functions.  Remember to configure the Telegram bot token and chat IDs appropriately for your production environment.  Adjust import paths as needed.
